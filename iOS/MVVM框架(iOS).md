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
![image](https://raw.githubusercontent.com/YoffieYF/Yoffie/master/image/WechatIMG175.png)

* M(Model):数据模型层，用于数据模态化，所有的的Model都有一个基类BaseModel,BaseModel有对应的方法去进行数据持久化的操作。一般情况是一个api接口对应一个RequstModel与RespondModel。RequstModel与RespondModel分别继承于BaseModel。RequstModel用于存放api接口请求的json参数模型。RespondModel用于api接口返回的json数据模态话。同时RespondModel缓存对应数据的frame与富文本（PS：一些UI耗性能的frame计算与富文本计算）
* V(View):视图层，用于控制控件的布局计算，配置数据绑定。
* V(ViewController):视图转发器或者叫视图控制器，用于处理视图层的一些用户交互逻辑，数据的刷新，数据的请求，页面的跳转。
* M(ViewModel):数据处理层，用于服务器的api接口请求，与将服务器返回的json转Model（即操作数据模态化）。

## iOS(object-c)对MVVM框架的实现
###总的目录结构
![image](https://raw.githubusercontent.com/YoffieYF/Yoffie/master/image/MVVMMVVM总目录结构.png)

###AppDetegate目录
此目录主要是存放一些项目启动的预加载操作，如通知注册，友盟注册，第三方微信登入注册，UIWind的初始化等一些操作。每个操作需要以类扩展的方式去实现。如:注册类的操作就会新建一个AppDetegate的类扩展文件，命名为AppDetegate+Register,在这个文件编写具体的代码。
![image](https://raw.githubusercontent.com/YoffieYF/Yoffie/master/image/AppDelegate文件结构.png)
<br>
###Macros目录
此目录存放的是一些项目常用的宏定义，全局通知标志符定义，国际化语言定义。
![image](https://raw.githubusercontent.com/YoffieYF/Yoffie/master/image/MVVM宏定义目录.png)
<br>
###Utils目录
此目录存放的是一些从其他地方引入的第三方组件。如一些正则表达式。
![image](https://raw.githubusercontent.com/YoffieYF/Yoffie/master/image/MVVM(Utils)目录结构.png)
<br>
###Categories目录
此目录存放的是一些类扩展。如 NSString+Safe 字符串安全操作的类扩展。
![image](https://raw.githubusercontent.com/YoffieYF/Yoffie/master/image/MVVM(Utils)目录结构.png)
<br>
###MYCommon目录
此目录存放的是一些团队开发的组件。如一些正则表达式。
![image](https://raw.githubusercontent.com/YoffieYF/Yoffie/master/image/MVVM(MYCommon)目录结构.png)
<br>
###Class目录
Class目录主要是存放业务逻辑的实现代码。这个目录也是对MVVM模型的实现。分为了Common的目录与一些具体的业务逻辑目录。Common目录也可以把它看成通用模块，而业务逻辑目录也可以把它看成一个模块（那些业务逻辑会被独立成一个模块，需要根据当前的业务去划分）。此目录也是团队成员写入代码最多的目录。所以模块的划分与通用模块设计尤为重要。因此此模块会重点讲解，会为如下小节讲解:

* MVVM(Class)目录结构
* MVVM(Class-Common)目录结构
* MVVM(Class-common-model)目录
* MVVM(Class-common-model-requst)目录
* MVVM(Class-Common-Model-response)目录
* MVVM(Class-Common-ViewModel)目录
* MVVM(Class-XXX-Model) 目录
* MVVM(Class-XXX-View) 目录
* MVVM(Class-XXX-ViewController)目录
* MVVM(Class-XXX-ViewModel) 目录

#### MVVM(Class)目录结构
MVVM(Class)目录结构主要就是分Common目录与XXX目录（XXX代表具体的业务模块，如启动页广告模块）。
![image](https://raw.githubusercontent.com/YoffieYF/Yoffie/master/image/MVVM(Class)目录机构.png)

#### MVVM(Class-Common)目录结构
MVVM(Class-Common)主要是存放了通用的业务逻辑模块。如通用Cell,View,Model,ViewModel,ViewController。Cell,View都是一些通用的视图层可以提供给个个业务逻辑模块使用。ViewController是存放的是所有业务控制器层的基类，如导航栏点击返回的操作可以一并写入此文件，而个个对立的业务逻辑模块控制器层是继承此控制器层基类。而View与ViewController层相对简单因此不做过多的讲解。（如下图：）
![image](https://raw.githubusercontent.com/YoffieYF/Yoffie/master/image/MVVM(Class-Common).png)

### MVVM(Class-common-model)目录
MVVM(Class-common-model)目录Model分为了BaseModel与继承BaseModel的RequstModel跟ResponseModel。此目录结构存放的是一些项目通用的api接口数据参数。</br>
BaseModel最要是一些数据持久化的操作（如下图：）。
![image](https://raw.githubusercontent.com/YoffieYF/Yoffie/master/image/MVVM(Class-common-model).png)
RequstModel主要是定义请求api接口的参数（如下图：）。
![image](https://raw.githubusercontent.com/YoffieYF/Yoffie/master/image/MVVM(Class-common-model-requst).png)
ResponseModel主要是定义数据返回的参数（如下图：）。
![image](https://raw.githubusercontent.com/YoffieYF/Yoffie/master/image/MVVM(Class-Common-Model-respond).png)

### MVVM(Class-Common-ViewModel)目录
MVVM(Class-Common-ViewModel)目录主要是实现api接口的请求。而common下的ViewModel是项目通用的请求操作，是所有业务模块的基类。如几个业务逻辑模块都会使用到同一个接口请求，那么这个请求操作就应该放在通用的ViewModel下。（如下图：）
![image](https://raw.githubusercontent.com/YoffieYF/Yoffie/master/image/MVVM(Class-Common-ViewModel).png)

###MVVM(Class-XXX-Model)目录
MVVM(Class-XXX-Model)目录主要是独立业务逻辑模块的Model层。用于定义独立业务逻辑的api请求参数与返回参数。同时也可以缓存一些复杂UI的frame与对性能消耗大的操作。（如下图：）
![image](https://raw.githubusercontent.com/YoffieYF/Yoffie/master/image/MVVM(Class-Home-Model).png)

###MVVM(Class-XXX-View)目录
MVVM(Class-XXX-View)目录主要是独立业务逻辑模块的View层。用于定义一些控件视图的声明与位置，控件视图的交互回调。（如下图：）
![image](https://raw.githubusercontent.com/YoffieYF/Yoffie/master/image/MVVM(Class-Home-View).png)

###MVVM(Class-XXX-ViewController)目录
MVVM(Class-XXX-ViewController)目录主要是独立业务逻辑模块的ViewController层。用于定义页面的跳转，与被其他业务模块使用的方法。（如下图：）
![image](https://raw.githubusercontent.com/YoffieYF/Yoffie/master/image/MVVM(Class-Home-ViewController).png)

###MVVM(Class-XXX-ViewModel)目录
MVVM(Class-XXX-ViewController)目录主要是独立业务逻辑模块的ViewModel层。用于api接口请求的操作，所有的ViewModel都是继承BaseViewModel,如果此业务有独立的请求操作需要单独声明。（如下图：）
![image](https://raw.githubusercontent.com/YoffieYF/Yoffie/master/image/MVVM(Class-Home-ViewModel).png)
## 项目命名规则

