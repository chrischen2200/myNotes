# 1.环境下载，创建，运行项目

```shell
$ npm i -g @nestjs/cli	//下载nestjs脚手架
$ nest new project-name	//创建项目(project-name=>自定义项目名)
$ npm run start	//启动项目

-----------------------------
$ nest -v	//查看nestjs版本
$ nest --help //查看nest命令
```

# 2.创建模块

Nest中的控制器层负责处理传入的请求，并返回对客户端的响应

```shell
//自动创建新的controller模块，并在module中引入
$ nest g controller news //创建控制器（news=>控制器名）

//简写
$ nest g co demo  //创建控制器（demo=>自定义名）
$ nest g mo demo  //创建模块（demo=>自定义名）
$ nest g s demo   //创建Service（demo=>自定义名）

$ nest g resource user //根据指定风格，一次性生成CRUD模板（mvc，dto，entities）
$ nest g res user //简写
```

# 3.RESTful 版本控制 

一共有三种我们一般用第一种 更加语义化

| `URI Versioning`        | 版本将在请求的 URI 中传递（默认） |
| ----------------------- | --------------------------------- |
| `Header Versioning`     | 自定义请求标头将指定版本          |
| `Media Type Versioning` | 请求的`Accept`标头将指定版本      |

```js
import { NestFactory } from '@nestjs/core';
import { VersioningType } from '@nestjs/common';   //引入包
import { AppModule } from './app.module';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  app.enableVersioning({                            //版本控制配置
    type: VersioningType.URI,
  })
  await app.listen(3000);
}
bootstrap();
```

然后在user.controller 配置版本

Controller 变成一个对象 通过version 配置版本

```js
import { Controller, Get, Post, Body, Patch, Param, Delete, Version } from '@nestjs/common';
import { UserService } from './user.service';
import { CreateUserDto } from './dto/create-user.dto';
import { UpdateUserDto } from './dto/update-user.dto';

@Controller({
  path:"user",
  version:'1'
})
export class UserController {
  constructor(private readonly userService: UserService) {}

  @Post()
  create(@Body() createUserDto: CreateUserDto) {
    return this.userService.create(createUserDto);
  }

  @Get()
  // @Version('1')
  findAll() {
    return this.userService.findAll();
  }

  @Get(':id')
  findOne(@Param('id') id: string) {
    return this.userService.findOne(+id);
  }

  @Patch(':id')
  update(@Param('id') id: string, @Body() updateUserDto: UpdateUserDto) {
    return this.userService.update(+id, updateUserDto);
  }

```

#  3.Code码规范

200 OK

304 Not Modified 协商缓存了

400 Bad Request 参数错误

401 Unauthorized token错误

403 Forbidden referer origin 验证失败

404 Not Found 接口不存在

500 Internal Server Error 服务端错误

502 Bad Gateway 上游接口有问题或者服务器问题

# 4.装饰器

Controller Request （获取前端传过来的参数）
nestjs 提供了方法参数装饰器 用来帮助我们快速获取参数 如下

@Request()	req          
@Response()	res                   
@Next()	next
@Session()	req.session
@Param(key?: string)       		req.params/req.params[key]
@Body(key?: string)	        	  req.body/req.body[key]
@Query(key?: string)				req.query/req.query[key]
@Headers(name?: string)   	 req.headers/req.headers[name]
@HttpCode	

# 5.Session与验证码

session 是服务器 为每个用户的浏览器创建的一个会话对象 这个session 会记录到 浏览器的 cookie 用来区分用户

我们使用的是[nestjs](https://so.csdn.net/so/search?q=nestjs&spm=1001.2101.3001.7020) 默认框架express 他也支持express 的插件 所以我们就可以安装express的session

```shell
$ npm i express-session --save
```

需要智能提示可以装一个声明依赖

```shell
$ npm i @types/express-session -D
```

然后在main.ts 引入 通过app.use 注册session

```js
import * as session from 'express-session'


app.use(session())
```

参数配置详解

| 参数    | 详解                                                         |
| ------- | ------------------------------------------------------------ |
| secret  | 生成服务端session 签名 可以理解为加盐                        |
| name    | 生成客户端cookie 的名字 默认 connect.sid                     |
| cookie  | 设置返回到前端 key 的属性，默认值为{ path: ‘/’, httpOnly: true, secure: false, maxAge: null }。 |
| rolling | 在每次请求时强行设置 cookie，这将重置 cookie 过期时间(默认:false) |

nestjs配置

```js
import { NestFactory } from '@nestjs/core';
import { VersioningType } from '@nestjs/common';
import { AppModule } from './app.module';
import * as session from 'express-session'
async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  app.enableVersioning({
    type: VersioningType.URI
  })
  app.use(session({ secret: "XiaoMan", name: "xm.session", rolling: true, cookie: { maxAge: null } }))
  await app.listen(3000);
}
bootstrap();
```

验证码插件 svgCaptcha

```shell
npm install svg-captcha -S
```

```js
import { Controller, Get, Post, Body, Param, Request, Query, Headers, HttpCode, Res, Req } from '@nestjs/common';
import { UserService } from './user.service';
import { CreateUserDto } from './dto/create-user.dto';
import { UpdateUserDto } from './dto/update-user.dto';
import * as svgCaptcha from 'svg-captcha';
@Controller('user')
export class UserController {
  constructor(private readonly userService: UserService) { }
  @Get('code')
  createCaptcha(@Req() req, @Res() res) {
    const captcha = svgCaptcha.create({
      size: 4,//生成几个验证码
      fontSize: 50, //文字大小
      width: 100,  //宽度
      height: 34,  //高度
      background: '#cc9966',  //背景颜色
    })
    req.session.code = captcha.text //存储验证码记录到session
    res.type('image/svg+xml')
    res.send(captcha.data)
  }
 
  @Post('create')
  createUser(@Req() req, @Body() body) {
    console.log(req.session.code, body)
    if (req.session.code.toLocaleLowerCase() === body?.code?.toLocaleLowerCase()) {
      return {
        message: "验证码正确"
      }
    } else {
      return {
        message: "验证码错误"
      }
    }
 
  }
}
```

# 6.Providers

Providers 是 Nest 的一个基本概念。许多基本的 Nest 类可能被视为 provider - service, repository, factory, helper 等等。 他们都可以通过 constructor 注入依赖关系。 这意味着对象可以彼此创建各种关系，并且“连接”对象实例的功能在很大程度上可以委托给 Nest运行时系统。 Provider 只是一个用 @Injectable() 装饰器注释的类。

## 6.1基本用法

module 引入 service 在 providers 注入 

```js
import { Module } from '@nestjs/common';
import { GirlService } from './girl.service';    //注入
import { GirlController } from './girl.controller';

@Module({
  controllers: [GirlController],
  providers: [GirlService],         //注入
})
export class GirlModule {}
```

在Controller 就可以使用注入好的service 了

```js
import {
  Controller,
  Get,
  Post,
  Body,
  Patch,
  Param,
  Delete,
} from '@nestjs/common';
import { GirlService } from './girl.service';
import { CreateGirlDto } from './dto/create-girl.dto';
import { UpdateGirlDto } from './dto/update-girl.dto';

@Controller('girl')
export class GirlController {
  constructor(private readonly girlService: GirlService) {}      //使用service

  @Post()
  create(@Body() createGirlDto: CreateGirlDto) {
    return this.girlService.create(createGirlDto);
  }

  @Get()
  findAll() {
    return this.girlService.findAll();
  }

  @Get(':id')
  findOne(@Param('id') id: string) {
    return this.girlService.findOne(+id);
  }

  @Patch(':id')
  update(@Param('id') id: string, @Body() updateGirlDto: UpdateGirlDto) {
    return this.girlService.update(+id, updateGirlDto);
  }

  @Delete(':id')
  remove(@Param('id') id: string) {
    return this.girlService.remove(+id);
  }
}

```

## 6.2service 第二种用法(自定义名称)

第一种用法就是一个[语法糖](https://so.csdn.net/so/search?q=语法糖&spm=1001.2101.3001.7020)

其实他的全称是这样的

```js
import { Module } from '@nestjs/common';
import { GirlService } from './user.service';
import { GirlController } from './user.controller';
 
@Module({
  controllers: [UserController],
  providers: [{
    provide: "Xiaoman",       //自定义名称
    useClass: GirlService
  }]
})
export class UserModule { }
```

自定义名称之后 需要用对应的Inject 取 不然会找不到的

```js
import {
  Controller,
  Get,
  Post,
  Body,
  Patch,
  Param,
  Delete,
} from '@nestjs/common';
import { GirlServiceService } from './girl.service';
import { CreateGirlDto } from './dto/create-girl.dto';
import { UpdateGirlDto } from './dto/update-girl.dto';

@Controller('girl')
export class GirlController {
  constructor(@Inject('Xiaoman') private readonly girlService: GirlService) {}      //使用service

  @Post()
  create(@Body() createGirlDto: CreateGirlDto) {
    return this.girlService.create(createGirlDto);
  }

  @Get()
  findAll() {
    return this.girlService.findAll();
  }

  @Get(':id')
  findOne(@Param('id') id: string) {
    return this.girlService.findOne(+id);
  }

  @Patch(':id')
  update(@Param('id') id: string, @Body() updateGirlDto: UpdateGirlDto) {
    return this.girlService.update(+id, updateGirlDto);
  }

  @Delete(':id')
  remove(@Param('id') id: string) {
    return this.girlService.remove(+id);
  }
}
```

## 6.3自定义注入值

通过 useValue

```js
import { Module } from '@nestjs/common';
import { UserService } from './user.service';
import { UserController } from './user.controller';
 
@Module({
  controllers: [UserController],
  providers: [{
    provide: "Xiaoman",
    useClass: UserService
  }, {
    provide: "JD",
    useValue: ['TB', 'PDD', 'JD']
  }]
})
export class UserModule { }
```

```js
@Inject('JD') private shopList:string[]
```

## 6.4工厂模式

如果服务 之间有相互的依赖 或者逻辑处理 可以使用 useFactory

```js
import { Module } from '@nestjs/common';
import { UserService } from './user.service';
import { UserService2 } from './user.service2';
import { UserService3 } from './user.service3';
import { UserController } from './user.controller';
 
@Module({
  controllers: [UserController],
  providers: [{
    provide: "Xiaoman",
    useClass: UserService
  }, {
    provide: "JD",
    useValue: ['TB', 'PDD', 'JD']
  },
    UserService2,
  {
    provide: "Test",
    inject: [UserService2],
    useFactory(UserService2: UserService2) {
      return new UserService3(UserService2)
    }
  }
  ]
})
export class UserModule { }
```

## 6.5异步模式

useFactory 返回一个promise 或者其他异步操作

```js
import { Module } from '@nestjs/common';
import { UserService } from './user.service';
import { UserService2 } from './user.service2';
import { UserService3 } from './user.service3';
import { UserController } from './user.controller';
 
@Module({
  controllers: [UserController],
  providers: [{
    provide: "Xiaoman",
    useClass: UserService
  }, {
    provide: "JD",
    useValue: ['TB', 'PDD', 'JD']
  },
    UserService2,
  {
    provide: "Test",
    inject: [UserService2],
    useFactory(UserService2: UserService2) {
      return new UserService3(UserService2)
    }
  },
  {
    provide: "sync",
    async useFactory() {
      return await  new Promise((r) => {
        setTimeout(() => {
          r('sync')
        }, 3000)
      })
    }
  }
  ]
})
export class UserModule { }
```

# 7.Module

## 7.1基本用法

当我们使用nest g res user 创建一个[CURD](https://so.csdn.net/so/search?q=CURD&spm=1001.2101.3001.7020) 模板的时候 nestjs 会自动帮我们引入模块

## 7.2共享模块

例如 girl 的 Service 想暴露给 其他模块使用就可以使用exports 导出该服务

```shell
import { Module } from '@nestjs/common';
import { GirlService } from './girl.service';
import { GirlController } from './girl.controller';

@Module({
  controllers: [GirlController],
  providers: [GirlService],
  exports: [GirlService]   //导出service
})
export class GirlModule {}
```

由于App.modules 已经引入过该模块 就可以直接使用girl 模块的 Service 

```js
import { Module } from '@nestjs/common';
import { AppController } from './app.controller';
import { AppService } from './app.service';
import { ArticleController } from './article/article.controller';
import { UserController } from './user/user.controller';
import { UserModule } from './user/user.module';
import { GirlModule } from './girl/girl.module';

@Module({
  imports: [UserModule, GirlModule],    //由于已经引入了该模块
  controllers: [AppController, ArticleController, UserController],
  providers: [AppService],
})
export class AppModule {}
```

```js
import { Controller, Get } from '@nestjs/common';
import { AppService } from './app.service';
import { GirlService } from './girl/girl.service';

@Controller()
export class AppController {
  constructor(
    private readonly girlService: GirlService,    //可以直接使用
    private readonly appService: AppService,
  ) {}

  @Get()
  getHello(): string {
    return this.girlService.findAll();
  }
}
```

## 7.3全局模块

@Global()

我们给 user 模块添加 @Global() 他便注册为全局模块

然后在app.moudule.ts模块中导入全局模块，其他模块中即可使用该全局模块了

## 7.4动态模块

动态模块主要就是为了给模块传递参数 可以给该模块添加一个静态方法 用来接受参数

```js
import { Module, DynamicModule, Global } from '@nestjs/common'
 
 
 
interface Options {
    path: string
}
 
@Global()
@Module({
 
})
export class ConfigModule {
    static forRoot(options: Options): DynamicModule {
        return {
            module: ConfigModule,
            providers: [
                {
                    provide: "Config",
                    useValue: { baseApi: "/api" + options.path }
                }
            ],
            exports: [
                {
                    provide: "Config",
                    useValue: { baseApi: "/api" + options.path }
                }
            ]
        }
    }
} 
```

然后在app.moudule.ts模块中导入全局模块：引入**模块.forRoot({path:参数})**

# 8.中间件

中间件是在路由处理程序 之前 调用的函数。 中间件函数可以访问请求和响应对象

中间件函数可以执行以下任务:

- 执行任何代码。

- 对请求和响应对象进行更改。
- 结束请求-响应周期。
- 调用堆栈中的下一个中间件函数。
- 如果当前的中间件函数没有结束请求-响应周期, 它必须调用 next() 将控制传递给下一个中间件函数。否则, 请求将被挂起。

## 8.1创建一个依赖注入中间件

要求我们实现use 函数 返回 req res next 参数 如果不调用next 程序将被挂起

首先，创建一个名为 `LoggingMiddleware` 的中间件类：

```js
import { Injectable, NestMiddleware, Logger } from '@nestjs/common';
import { Request, Response, NextFunction } from 'express';

@Injectable()
export class LoggingMiddleware implements NestMiddleware {
  private readonly logger = new Logger(LoggingMiddleware.name);

  use(req: Request, res: Response, next: NextFunction) {
    const { method, originalUrl } = req;
    const start = Date.now();

    res.on('finish', () => {
      const responseTime = Date.now() - start;
      const { statusCode } = res;

      this.logger.log(
        `${method} ${originalUrl} ${statusCode} ${responseTime}ms`,
      );
    });

    next();
  }
}
```

然后，在应用的模块中使用该中间件：

```js
import { Module, NestModule, MiddlewareConsumer } from '@nestjs/common';
import { AppController } from './app.controller';
import { AppService } from './app.service';
import { LoggingMiddleware } from './path-to-your-logging-middleware/logging.middleware';

@Module({
  imports: [],
  controllers: [AppController],
  providers: [AppService],
})
export class AppModule implements NestModule {
  configure(consumer: MiddlewareConsumer) {
    consumer.apply(LoggingMiddleware).forRoutes('*');
  }
}
```

## 8.2全局中间件

```js
import { NestFactory } from '@nestjs/core';
import { AppModule } from './app.module';
import { MyMiddleware } from './path-to-your-middleware/my.middleware';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);

  // 在这里引入中间件
  app.use(MyMiddleware);

  await app.listen(3000);
}
bootstrap();
```

## 8.3接入第三方中间件，例如 cors处理跨域

首先，安装`@nestjs/platform-express`模块（如果尚未安装）：

```shell
$ npm install @nestjs/platform-express
```

然后，在你的模块中应用`CorsMiddleware`中间件：

```js
import { Module, NestModule, MiddlewareConsumer } from '@nestjs/common';
import { AppController } from './app.controller';
import { AppService } from './app.service';
import { CorsMiddleware } from '@nestjs/platform-express';

@Module({
  imports: [],
  controllers: [AppController],
  providers: [AppService],
})
export class AppModule implements NestModule {
  configure(consumer: MiddlewareConsumer) {
    consumer.apply(CorsMiddleware).forRoutes('*');
  }
}
```

上述代码中，我们使用`CorsMiddleware`将跨域请求支持添加到所有路由（`'*'`）上。你也可以指定特定的路由或控制器，而不仅仅是`'*'`。

`CorsMiddleware`可以根据需要进行配置，例如允许特定的来源、方法和头部等。你可以通过传递一个配置对象来实现这些配置。以下是一个示例：

```js
import { Module, NestModule, MiddlewareConsumer } from '@nestjs/common';
import { AppController } from './app.controller';
import { AppService } from './app.service';
import { CorsMiddleware } from '@nestjs/platform-express';

@Module({
  imports: [],
  controllers: [AppController],
  providers: [AppService],
})
export class AppModule implements NestModule {
  configure(consumer: MiddlewareConsumer) {
    const corsOptions = {
      origin: 'http://example.com', // 允许的来源
      methods: 'GET,HEAD,PUT,PATCH,POST,DELETE', // 允许的 HTTP 方法
      credentials: true, // 是否允许发送凭证（如 cookies）
    };

    consumer.apply(CorsMiddleware(corsOptions)).forRoutes('*');
  }
}
```

这样，通过在NestJS中应用`CorsMiddleware`中间件，你就可以轻松处理跨域请求。

# 9. 上传图片和静态文件访问

## 9.1主要会用到两个包

multer   @nestjs/platform-express nestJs自带了

multer   @types/multer 这两个需要安装

在upload  Module 使用MulterModule register注册存放图片的目录

需要用到  multer 的  diskStorage 设置存放目录 extname 用来读取文件后缀 filename给文件重新命名

```js
import { Module } from '@nestjs/common';
import { UploadService } from './upload.service';
import { UploadController } from './upload.controller';
import { MulterModule } from '@nestjs/platform-express'
import {diskStorage} from 'multer'
import { extname,join } from 'path';
@Module({
  imports: [MulterModule.register({                  //导入文件上传模块
     storage:diskStorage({
        destination:join(__dirname,"../images"),
        filename:(_,file,callback) => {
           const fileName = `${new Date().getTime() + extname(file.originalname)}`
           return callback(null,fileName)
        }
     })
  })],
  controllers: [UploadController],
  providers: [UploadService]
})
export class UploadModule { }
```

## 9.2controller 使用

使用 UseInterceptors [装饰器](https://so.csdn.net/so/search?q=装饰器&spm=1001.2101.3001.7020) FileInterceptor是单个 读取字段名称 FilesInterceptor是多个

参数 使用 UploadedFile 装饰器接受file 文件

```js
import { Controller, Get, Post, Body, Patch, Param, Delete,UseInterceptors,UploadedFile } from '@nestjs/common';
import { UploadService } from './upload.service';
import {FileInterceptor} from '@nestjs/platform-express'
@Controller('upload')
export class UploadController {
  constructor(private readonly uploadService: UploadService) {}
  @Post('album')
  @UseInterceptors(FileInterceptor('file'))    //文件拦截
  upload (@UploadedFile() file) {              //文件上传方法
    console.log(file)
    return true
  }
}
```

### 9.3生成静态目录访问上传之后的图片

useStaticAssets prefix 是虚拟前缀

```js
import { NestFactory } from '@nestjs/core';
import { AppModule } from './app.module';
import {NestExpressApplication} from '@nestjs/platform-express'
import { join } from 'path'
async function bootstrap() {
  const app = await NestFactory.create<NestExpressApplication>(AppModule);
  app.useStaticAssets(join(__dirname,'images'),{      //静态目录
     prefix:"/xiaoman"                                 //设置虚拟目录
  })
  await app.listen(3000);
}
bootstrap();
```

# 10.下载图片

## 10.1download直接下载

```js
import { Controller, Post, UseInterceptors, UploadedFile, Get, Res } from '@nestjs/common';
import { UploadService } from './upload.service';
import { FileInterceptor, FilesInterceptor } from '@nestjs/platform-express'
import type { Response } from 'express'
import {join} from 'path'
@Controller('upload')
export class UploadController {
  constructor(private readonly uploadService: UploadService) { }
  @Post('album')
  @UseInterceptors(FileInterceptor('file'))
  upload(@UploadedFile() file) {
    console.log(file, 'file')
    return '峰峰35岁憋不住了'
  }
  
  //文件下载方法
  @Get('export')
  downLoad(@Res() res: Response) {
    const url = join(__dirname,'../images/1662894316133.png') //图片地址
    // res
    // console.log(url)
    res.download(url)
    // return  true
  }
}
```

##  10.2使用文件流的方式下载

可以使用compressing把他压缩成一个zip包

import {zip} from 'compressing'

```js
  @Get('stream')
  async down (@Res() res:Response) {
    const url = join(__dirname,'../images/1662894316133.png')
    const tarStream  = new zip.Stream()
    await tarStream.addEntry(url)
    
    res.setHeader('Content-Type', 'application/octet-stream');
   
    res.setHeader(
      'Content-Disposition',  
      `attachment; filename=xiaoman`,
    );
 
    tarStream.pipe(res)

  }
```

前端接受流

```js
const useFetch = async (url: string) => {
  const res = await fetch(url).then(res => res.arrayBuffer())
  console.log(res)
  const a = document.createElement('a')
  a.href = URL.createObjectURL(new Blob([res],{
    // type:"image/png"
  }))
  a.download = 'xiaman.zip'
  a.click()
}
 
const download = () => {
  useFetch('http://localhost:3000/upload/stream')
}
```

# 11.Rxjs

RxJs 使用的是[观察者模式](https://so.csdn.net/so/search?q=观察者模式&spm=1001.2101.3001.7020)，用来编写异步队列和事件处理。

[Observable](https://so.csdn.net/so/search?q=Observable&spm=1001.2101.3001.7020) 可观察的物件

Subscription 监听Observable

Operators 纯函数可以处理管道的数据 如 map filter concat reduce 等

# 12.响应拦截器

在 NestJS 中，响应拦截器（Response Interceptors）是一种用于在响应数据返回给客户端之前对数据进行处理的机制。拦截器可以在响应管道的最后阶段对响应数据进行转换、修改或包装，以满足特定的需求。

响应拦截器可以用于以下场景：

1. 格式化响应数据，将数据转换为特定的格式（例如 JSON）。
2. 对数据进行加工，例如添加额外的元数据。
3. 对错误进行处理，统一处理错误响应。
4. 实现数据缓存，将数据缓存起来以提高性能。
5. 加密或解密数据。
6. ...

以下是一个响应拦截器的简单示例：

```js
import { Injectable, NestInterceptor, ExecutionContext, CallHandler } from '@nestjs/common';
import { Observable } from 'rxjs';
import { map } from 'rxjs/operators';

@Injectable()
export class TransformInterceptor implements NestInterceptor {
  intercept(context: ExecutionContext, next: CallHandler): Observable<any> {
    return next.handle().pipe(
      map(data => ({
        statusCode: context.switchToHttp().getResponse().statusCode,
        data,
      })),
    );
  }
}

```

在上述示例中，我们创建了一个名为 `TransformInterceptor` 的拦截器。它实现了 `NestInterceptor` 接口，并覆写了 `intercept` 方法。在这个示例中，拦截器会在响应数据之前将数据转换为包含响应状态码和数据的对象。

要将拦截器应用到具体的控制器方法上，可以使用 `@UseInterceptors()` 装饰器，如下所示：

```js
import { Controller, Get, UseInterceptors } from '@nestjs/common';
import { TransformInterceptor } from './transform.interceptor';

@Controller('cats')
@UseInterceptors(TransformInterceptor)
export class CatsController {
  @Get()
  findAll(): string[] {
    return ['Meow', 'Purr'];
  }
}
```

在上述示例中，`TransformInterceptor` 被应用到 `findAll()` 方法上，当控制器方法返回数据时，拦截器会对返回的数据进行转换。

响应拦截器允许你在响应管道中对数据进行最终处理，以及在统一地、可重用地处理响应数据。

如果你想要将拦截器应用到特定的方法而不是整个控制器，可以使用以下方式：

```js
@Controller('cats')
export class CatsController {
  @Get()
  @UseInterceptors(TransformInterceptor)
  findAll(): string[] {
    return ['Meow', 'Purr'];
  }
}

```

# 13.异常拦截器

在 NestJS 中，异常拦截器（Exception Interceptors）用于捕获应用程序中发生的异常并进行处理。异常拦截器允许你在应用程序的各个地方统一处理异常，例如对异常进行日志记录、格式化响应、自定义错误消息等。

以下是如何创建和使用异常拦截器的步骤：

- 创建一个异常过滤器类，实现 `ExceptionFilter` 接口。

```js
import { ExceptionFilter, Catch, ArgumentsHost, HttpException } from '@nestjs/common';
import { Response } from 'express';

@Catch(HttpException)
export class HttpExceptionFilter implements ExceptionFilter {
  catch(exception: HttpException, host: ArgumentsHost) {
    const ctx = host.switchToHttp();
    const response = ctx.getResponse<Response>();
    const status = exception.getStatus();

    response
      .status(status)
      .json({
        statusCode: status,
        message: exception.message,
      });
  }
}
```

- 在控制器类或模块上使用 `@UseFilters()` 装饰器将异常拦截器应用到特定范围。

```js
import { Controller, Get, UseFilters } from '@nestjs/common';
import { HttpExceptionFilter } from './http-exception.filter';

@Controller('cats')
@UseFilters(HttpExceptionFilter)
export class CatsController {
  @Get()
  findAll(): string[] {
    throw new HttpException('Cats not found', 404);
  }
}
```

- 在应用程序启动文件 `main.ts` 中，创建应用实例并使用 `app.useGlobalFilters()` 方法应用异常过滤器。

  ```js
  import { NestFactory } from '@nestjs/core';
  import { AppModule } from './app.module';
  import { HttpExceptionFilter } from './http-exception.filter';
  
  async function bootstrap() {
    const app = await NestFactory.create(AppModule);
    app.useGlobalFilters(new HttpExceptionFilter());
    await app.listen(3000);
  }
  bootstrap();
  ```

# 14.管道

在 NestJS 中，管道（Pipes）是一种用于处理输入数据的机制，用于对数据进行验证、转换和过滤。管道允许你在数据到达控制器的方法之前或之后，对输入数据进行各种处理操作，以确保数据的有效性、格式正确性和一致性。

NestJS 管道的主要目的是将通用的数据处理逻辑从控制器中抽象出来，从而提高代码的可维护性和可读性。通过使用管道，你可以实现以下目标：

1. **数据验证：** 管道可以用于验证输入数据的有效性。例如，你可以使用管道确保用户提供的输入数据符合特定的格式和规则。
2. **数据转换：** 管道可以将输入数据转换为所需的格式。例如，你可以使用管道将字符串转换为数字、日期，或将日期格式化为特定的字符串。
3. **数据过滤：** 管道可以过滤掉不需要的数据，或从输入数据中提取所需的部分。
4. **默认值设置：** 管道可以在输入数据为空或未定义时，为数据设置默认值。
5. **错误处理：** 管道可以捕获异常并进行处理，例如在验证失败时抛出自定义错误消息。

## 14.1常用管道API

- **ValidationPipe：** `ValidationPipe` 是用于数据验证的管道，基于 `class-validator` 库。它可以验证数据的类型、格式和自定义规则，如果数据不满足验证规则，会抛出异常。

  ```js
  import { Controller, Get, Query, ValidationPipe } from '@nestjs/common';
  
  @Controller('cats')
  export class CatsController {
    @Get()
    findAll(@Query('id', ValidationPipe) id: number): string {
      return `You requested cat with ID: ${id}`;
    }
  }
  
  //在上述示例中，ValidationPipe 将验证查询参数中的 id 是否为有效的数字类型。如果 id 不是数字或不满足验证规则，ValidationPipe 会抛出异常。
  ```

- **ParseIntPipe：** `ParseIntPipe` 用于将输入数据解析为整数类型。它会尝试将输入字符串转换为整数，如果无法成功转换则会抛出异常。

  ```js
  import { Controller, Get, Query, ParseIntPipe } from '@nestjs/common';
  
  @Controller('cats')
  export class CatsController {
    @Get()
    findAll(@Query('page', ParseIntPipe) page: number): string {
      return `You requested page: ${page}`;
    }
  }
  
  //在上述示例中，ParseIntPipe 会尝试将查询参数中的 page 转换为整数类型。如果无法成功转换，ParseIntPipe 会抛出异常。
  ```

- **ParseFloatPipe：** `ParseFloatPipe` 用于将输入数据解析为浮点数类型。类似于 `ParseIntPipe`，它会尝试将输入字符串转换为浮点数，转换失败则抛出异常。

- **ParseBoolPipe：** `ParseBoolPipe` 用于将输入数据解析为布尔值类型。它支持解析 `'true'`、`'false'`、`'1'`、`'0'` 等字符串值为对应的布尔值，无法转换时会抛出异常。

- **ParseArrayPipe：** `ParseArrayPipe` 用于将输入数据解析为数组类型。你可以通过指定分隔符来分割输入字符串，从而生成数组。

- **ParseUUIDPipe：** `ParseUUIDPipe` 用于将输入数据解析为 UUID（通用唯一标识符）类型。它可以验证输入字符串是否符合 UUID 的格式，如果不符合则抛出异常。

- **ParseEnumPipe：** `ParseEnumPipe` 用于将输入数据解析为枚举类型。它会验证输入值是否在指定的枚举类型中，如果不在范围内则抛出异常。

- **DefaultValuePipe：** `DefaultValuePipe` 用于设置默认值。当输入数据为空或未定义时，会返回指定的默认值。

## 14.2管道验证DTO

在 NestJS 中，你可以使用管道（Pipes）来验证 DTO（数据传输对象）。DTO 是用于传递数据的对象，通常在控制器和服务之间传递数据。使用管道验证 DTO 可以确保传递的数据满足预期的规则和格式。

以下是如何在 NestJS 中使用管道来验证 DTO 的步骤：

- **安装验证器**
  - $ npm i --save class-validator class-transformer

**创建 DTO 类：** 首先，创建一个用于传递数据的 DTO 类。DTO 类是一个普通的 TypeScript 类，定义了数据的结构和格式。

```js
// cat.dto.ts

import { IsString, IsInt } from 'class-validator';

export class CreateCatDto {
  @IsString()
  name: string;

  @IsInt()
  age: number;
}
```

在上述示例中，`CreateCatDto` 定义了一个名为 `name` 的字符串属性和一个名为 `age` 的整数属性，并使用 `class-validator` 的装饰器进行验证。

**使用 ValidationPipe：** 在控制器方法中使用 `ValidationPipe` 来验证传入的 DTO。

```js
import { Controller, Post, Body, UsePipes } from '@nestjs/common';
import { CreateCatDto } from './cat.dto';
import { ValidationPipe } from '@nestjs/common';

@Controller('cats')
export class CatsController {
  @Post()
  @UsePipes(new ValidationPipe())
  create(@Body() createCatDto: CreateCatDto) {
    return `You created a cat: ${JSON.stringify(createCatDto)}`;
  }
}

```

在上述示例中，我们使用了 `@UsePipes()` 装饰器来将 `ValidationPipe` 应用于 `create()` 方法的 `createCatDto` 参数。当请求进入 `create()` 方法时，`ValidationPipe` 会自动对传入的 `createCatDto` 进行验证。

###### 注册全局DTO验证管道 

```
import {ValidationPipe} form '@nestjs/common'
app.useGobalPipes{new ValidationPipe()}
```

# 15.爬虫

工具；

- **cheerio**: 是jquery核心功能的一个快速灵活而又简洁的实现，主要是为了用在服务器端需要对DOM进行操作的地方，让你在服务器端和html愉快的玩耍。
- **axios** 网络请求库可以发送http请求

# 16.守卫

守卫（guard）
守卫有一个单独的责任。它们根据运行时出现的某些条件（例如权限，角色，访问控制列表等）来确定给定的请求是否由路由处理程序处理。这通常称为授权。在传统的 Express 应用程序中，通常由中间件处理授权(以及认证)。中间件是身份验证的良好选择，因为诸如 token 验证或添加属性到 request 对象上与特定路由(及其元数据)没有强关联。

**tips 守卫在每个中间件之后执行，但在任何拦截器或管道之前执行。**

**创建守卫类：** 创建一个守卫类，实现 `CanActivate` 接口或其他守卫接口。守卫类可以用于在请求到达路由处理程序之前或之后执行逻辑。

```shell
nest g gu [name]
```

```js
import { Injectable, CanActivate, ExecutionContext } from '@nestjs/common';
import { Observable } from 'rxjs';

@Injectable()
export class AuthGuard implements CanActivate {
  canActivate(context: ExecutionContext): boolean | Promise<boolean> | Observable<boolean> {
    // 在此处编写守卫逻辑，返回 true 表示允许请求继续处理，返回 false 则中断处理
    return true;
  }
}
```

**在路由处理程序上应用守卫：** 在控制器方法上使用 `@UseGuards()` 装饰器来应用守卫。

```js
import { Controller, Get, UseGuards } from '@nestjs/common';
import { AuthGuard } from './auth.guard';

@Controller('cats')
@UseGuards(AuthGuard)
export class CatsController {
  @Get()
  findAll(): string[] {
    return ['Meow', 'Purr'];
  }
}

```

**在全局应用守卫**

```js
app.useGlobalGuards(new AuthGuard())
```

### 针对角色控制守卫

SetMetadata 装饰器

第一个参数为key，第二个参数自定义我们的例子是数组存放的权限

guard 使用 Reflector 反射读取 setMetaData的值 去做判断这边例子是从url 判断有没有admin权限

```js
import { CanActivate, ExecutionContext, Injectable } from '@nestjs/common';
import { Observable } from 'rxjs';
import { Reflector } from '@nestjs/core'
import type { Request } from 'express'
@Injectable()
export class RoleGuard implements CanActivate {
  constructor(private Reflector: Reflector) { }
  canActivate(
    context: ExecutionContext,
  ): boolean | Promise<boolean> | Observable<boolean> {
    const admin = this.Reflector.get<string[]>('role', context.getHandler())
    const request = context.switchToHttp().getRequest<Request>()
    if (admin.includes(request.query.role as string)) {
      return true;
    }else{
      return false
    }
   
  }
}
```

# 17.自定义装饰器

以下是创建自定义装饰器的基本步骤：

1. **创建装饰器类：** 创建一个 TypeScript 类来定义你的自定义装饰器。装饰器类需要实现 `Decorator` 接口。

   ```js
   import { SetMetadata, createParamDecorator, ExecutionContext } from '@nestjs/common';
   
   // 自定义授权装饰器
   export const Roles = (...roles: string[]) => SetMetadata('roles', roles);
   
   // 自定义参数装饰器
   export const User = createParamDecorator((data: unknown, ctx: ExecutionContext) => {
     const request = ctx.switchToHttp().getRequest();
     return request.user;
   });
   ```

   在上述示例中，我们创建了两个自定义装饰器：`Roles` 和 `User`。`Roles` 装饰器用于给控制器或方法添加权限角色信息，而 `User` 装饰器用于从请求中提取用户信息。

2. **使用自定义装饰器：** 使用自定义装饰器来修饰控制器、方法、参数等地方。

   ```js
   import { Controller, Get, UseGuards } from '@nestjs/common';
   import { Roles } from './custom.decorators';
   import { RolesGuard } from './roles.guard';
   
   @Controller('cats')
   export class CatsController {
     @Get()
     @Roles('admin') // 使用自定义装饰器
     findAll(): string[] {
       return ['Meow', 'Purr'];
     }
   }
   ```

   在上述示例中，我们创建了两个自定义装饰器：`Roles` 和 `User`。`Roles` 装饰器用于给控制器或方法添加权限角色信息，而 `User` 装饰器用于从请求中提取用户信息。

# 18.Swagger接口文档

[swagger](https://docs.nestjs.com/openapi/introduction) 用于提供给前端接口文档

安装命令如下

```shell
npm install  @nestjs/swagger swagger-ui-express
```

### 在main.ts 注册swagger

```javascript
async function bootstrap() {
  const app = await NestFactory.create<NestExpressApplication>(AppModule);
  const options = new DocumentBuilder().setTitle('小满接口文档').setDescription('描述，。。。').setVersion('1').build()
  const document = SwaggerModule.createDocument(app,options)
  SwaggerModule.setup('/api-docs',app,document)
  await app.listen(3000);
}
bootstrap();
```

**为控制器添加装饰器：** 使用 `@ApiOperation()`, `@ApiResponse()`, `@ApiProperty()`, `@ApiTags()` 等装饰器来为你的控制器、方法和参数添加元数据。这些元数据将用于生成 API 文档。

```js
import { Controller, Get, Param } from '@nestjs/common';
import { ApiOperation, ApiParam, ApiResponse, ApiTags } from '@nestjs/swagger';

@ApiTags('cats')
@Controller('cats')
export class CatsController {
  @Get(':id')
  @ApiOperation({ summary: 'Find a cat by ID' })
  @ApiParam({ name: 'id', description: 'ID of the cat' })
  @ApiResponse({ status: 200, description: 'The found cat' })
  @ApiResponse({ status: 404, description: 'Cat not found' })
  findOne(@Param('id') id: string) {
    // 实际的方法实现
    return `Cat with ID ${id}`;
  }
}

```

## 19.连接数据库

ORM框架：Prisma，typeorm
