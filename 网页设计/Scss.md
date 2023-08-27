# Scss学习笔记

# 1. 基本语法与流程控制

$变量:值

## 1.1if与else

@if(?==?){ }

 @else{ }

## 1.2for循环

@for $item from 1 to6

@for $item from 1 through 6

## 1.3each循环

$colors:red,green,yellow

@each $item $colors{

​	$index= index($colors,$item);

​	li:nth-child(#{$index}){

​	background: $item	

​	}

}

## 1.4混入

@mixin 函数名($a:all,$b:1s){.    //:后面的是默认值

​	样式1: $a,$b

​	样式2: $a,$b

}

.box{

​	@include 函数名(实参a, 实参b)

}

# 2嵌套

```css
div{
  width:100px;
  height:100px;
  
  p{
    width:50px;
    height:50px;
    
    span{
      color:red;
    }
  }
}

ul{
  > li {                      // ‘>’ ：直接子元素
    		background:yellow;
        &:hover{										//& ：代表给自己加样式
        background-color:red;   
    }
    		&.active{
      	background-color:blue;   
    }
  }
}
```

# 3.继承与导入

继承：@extend

```scss
.base{
  width:100px;
  height:100px;
  outline:none;
}

.btn_primary{
  @extend .base;
  background-color:green;
}

.btn_default{
  @extend .base;
  background-color:gray;
}
```

导入：@import 

```scss
@import "./xxx.scss"  //使用公共css模块
```

