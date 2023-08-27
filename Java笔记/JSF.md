# 1.Web.xml 

配置Faces Servlet拦截请求，由javax.faces.webapp.FacesServlet来处理，启动servlet的时候加载**自定义xml**文件

# 2.自定义.xml

配置managed-bean，jsp以及跳转导航（bean方法的返回值=>跳转面）

# 3.javabean和jsp

| javabean | jsp  |
| -------- | ---- |
| 属性     | 项目 |
| 方法     | 事件 |

# 4.JSF

一、JSF的运行原理（工作方式）

　　1.JSF应用是事件驱动的，当一个事件发生时（比如用户单击一个按钮），事件通知通过HTTP发往服务器，服务器端使用做FacesServlet的特殊servlet处理该通知，web容器里每一个jsf应用都有它自己的FacesServlet;

在后台，每一个jsf请求都触发了3件事情：

　　1）FacesServlet创建FacesContext(该对象中包含Web容器传给FacesServlet的service方法的ServletContext,ServletRequest,ServletRespons对象，在处理过程中主要就是修改这个FacesContext)

　　2）FacesServlet把控制权交给Lifecycle

　　3）Lifecycle分6个阶段处理FacesContext(也即jsf生命周期过程)

二、JSF生命周期

　　1. 重建视图阶段（Restore View Phase）

当请求JSF页面时，如点击按钮或链接，JSF开始重建视图阶段。在 这个阶段JSF建立页面的视图，给视图中的组件设置事件处理器、校验器，在FacesContext中保存视图。FacesContext含有所有处理请 求的信息，所以页面元素包括组件标签、事件处理器、转换器、校验器都要接触FacesContext。如果请求是第一次的请求，JSF在这个阶段产生一个 空的视图，生命周期进入显示应答阶段，这个空的视图会在页面返回的时候用到。如果请求是返回的请求，对应于这个页面的视图已经存在，JSF用存在客户端或 服务器端的信息重建视图。

　　2. 应用视图值阶段（Apply Request Values Phase）

在组件树重建后，每一个树上的组件用decode方法从请求中解出其新的值，这个值保存在组件中。如果值数据转换失败，产生与此组件相联系的错误，并入FacesContext的上下文，错误信息在其后的显示应答阶段显示。如果任何decode方法或事件监听器调用了当前FacesContext的renderResponse方法，则JSF直接跳到显示应答阶段。如果在这个阶段有事件产生，JSF广播事件到感兴趣的监听器。如果此时应用转到另一个web应用或应答不含有JSF组件，则调用FacesContext.renderComplete方法。在此阶段结束时，所有组件已得到新值，错误信息和事件已入队列。

　　3. 处理校验阶段（Process Validations Phase）

此阶段，JSF处理所有组件树上注册的校验器，检查设置了校验的组件属性，如果值不合法JSF在上下文（FacesContext）中加入错误信息，生命周期直接进入显示应答阶段，显示错误信息，如果有转换错误也会显示。如果任何validate方法或事件监听器调用上下文的renderResponse方法，JSF直接跳到显示应答阶段。

　　4. 更新模型值阶段（Update Model Values Phase）

在JSF确定数据合法之后，遍历组件树，从组件中取得相应值设置到服务器对象上。如果任何updateModels或监听器调用renderResponse方法，JSF直接跳到显示应答阶段。

　　5. 调用应用阶段（Invoke Application Phase）

此阶段，JSF除了应用级别事件，如：表格提交或到其它页面的链接等；重建视图时产生的事件广播到感兴趣的监听器上，JSF计算应答到新的页面。

　　6. 显示应答阶段（Render Response Phase）

此阶段，如果应用是JSP页面，JSF将控制转到JSP容器。如果是第一次请求，执行JSP页面是会把页面上显示的组件加到组件树中。当JSP容器遍历页面的标签时组件会将自己显示出来。如果是返回的请求且在其它阶段产生了错误，则显示原始页面并显示错误信息。

JSF提供了PhaseId类来代表生命周期阶段，本质上是一个枚举类，常常使用的常量：

**ANY_PHASE：**任意一个生命周期阶段
**RESTORE_VIEW：**恢复视图阶段
**APPLY_REQUEST_VALUES：**应用请求值阶段
**PROCESS_VALIDATIONS：**处理输入校验阶段
**UPDATE_MODEL_VALUES：**更新模型的值阶段
**INVOKE_APPLICATION：**调用应用阶段
**RENDER_RESPONSE：**生成响应阶段

三、流程图如下：

![img](https://images2018.cnblogs.com/blog/1233706/201804/1233706-20180417092201952-1270897721.png)

![img](https://images2018.cnblogs.com/blog/1233706/201804/1233706-20180417092533055-1350130118.png)