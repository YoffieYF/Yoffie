# MVVM框架(iOS)
序语:个人从事iOS已经有5年了，接触的框架也有一些，使用最多的就是MVVM框架的代码结构，因此也对MVVM框架个人使用心得总结一下。(PS:本文不想涉及过多概念讲解，更多是项目结构的实现)
## 为什么要使用框架组织代码
随着开发的功能业务不断增多，代码的数量也在不断增加。开发团队也会不断的扩充，与人员的更换。那如何能高效的进行业务逻辑的开发呢？如何更好的进行团队协作呢？使用框架就是为了解决这些痛点的。
## iOS主流框架有那些
* MVC: MVC 即 Modal View Controller（模型 视图 控制器）。
* MVVM: MVVM就是在MVC的基础上分离出业务处理的逻辑到ViewModel层。
* MVVM + RAC: 在MVVM的基础上引入ReactiveCocoa，实现数据的双向绑定(即：KVO机制)。
* VIPER: MVVM的变种，增加了路由机制，对ViewModel做了更加具体的分离抽象。
* (PS:个人觉得MVVM的框架设计模式就能适用比较大型的项目开发了，框架更多的是概念性的东西，它只是一种代码结构的设计模式。不管使用那种框架只要对代码的结构有着严格的管理就OK了。)

## 细说MVVM框架
![image](https://github.com/YoffieYF/Yoffie/blob/master/image/WechatIMG175.png)<br>

* M(Model):数据模型层，用于数据模态化，所有的的Model都有一个基类BaseModel,BaseModel有对应的方法去进行数据持久化的操作。一般情况是一个api接口对应一个RequstModel与RespondModel。RequstModel与RespondModel分别继承于BaseModel。RequstModel用于存放api接口请求的json参数模型。RespondModel用于api接口返回的json数据模态话。同时RespondModel缓存对应数据的frame与富文本（PS：一些UI耗性能的frame计算与富文本计算）
* V(View):视图层，用于控制控件的布局计算，配置数据绑定。
* V(ViewController):视图转发器或者叫视图控制器，用于处理视图层的一些用户交互逻辑，数据的刷新，数据的请求，页面的跳转。
* M(ViewModel):数据处理层，用于服务器的api接口请求，与将服务器返回的json转Model（即操作数据模态化）。

## iOS(object-c)对MVVM框架的实现
