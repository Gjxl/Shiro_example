## 1.shiro是什么?

###### Shiro是Apache下的一个开源项目。shiro属于轻量级框架，相对于SpringSecurity简单的多，也没有SpringSecurity那么复杂。以下是我自己学习之后的记录。

###### 官方架构图如下：



![img](https://upload-images.jianshu.io/upload_images/15087669-df91fa9f6c3e40d7.png?imageMogr2/auto-orient/strip|imageView2/2/w/414/format/webp)

官方架构图

## 2.主要功能

### shiro主要有三大功能模块：

###### 1. Subject：主体，一般指用户。

###### 2. SecurityManager：安全管理器，管理所有Subject，可以配合内部安全组件。(类似于SpringMVC中的DispatcherServlet)

###### 3. Realms：用于进行权限信息的验证，一般需要自己实现。

## 3.细分功能

###### 1. Authentication：身份认证/登录(账号密码验证)。

###### 2. Authorization：授权，即角色或者权限验证。

###### 3. Session Manager：会话管理，用户登录后的session相关管理。

###### 4. Cryptography：加密，密码加密等。

###### 5. Web Support：Web支持，集成Web环境。

###### 6. Caching：缓存，用户信息、角色、权限等缓存到如redis等缓存中。

###### 7. Concurrency：多线程并发验证，在一个线程中开启另一个线程，可以把权限自动传播过去。

###### 8. Testing：测试支持；

###### 9. Run As：允许一个用户假装为另一个用户（如果他们允许）的身份进行访问。

###### 10. Remember Me：记住我，登录后，下次再来的话不用登录了。

（更多关于shiro是什么的文字请自行去搜索引擎找，本文主要记录springboot与shiro的集成）
首先先创建springboot项目，此处不过多描述。



##  4.URL匹配规则

（1）“?”：匹配一个字符，如”/admin?”，将匹配“ /admin1”、“/admin2”，但不匹配“/admin”

（2）“*”：匹配零个或多个字符串，如“/admin*”，将匹配“ /admin”、“/admin123”，但不匹配“/admin/1”

（3）“**”：匹配路径中的零个或多个路径，如“/admin/**”，将匹配“/admin/a”、“/admin/a/b”

*三、shiro过滤器*

![img](https://oscimg.oschina.net/oscnet/ef54d6ad0081ac57216a93b92326a537a02.jpg)

（1）anon：匿名过滤器，表示通过了url配置的资源都可以访问，例：“/statics/**=anon”表示statics目录下所有资源都能访问

（2）authc：基于表单的过滤器，表示通过了url配置的资源需要登录验证，否则跳转到登录，例：“/unauthor.jsp=authc”如果用户没有登录访问unauthor.jsp则直接跳转到登录

（3）authcBasic：Basic的身份验证过滤器，表示通过了url配置的资源会提示身份验证，例：“/welcom.jsp=authcBasic”访问welcom.jsp时会弹出身份验证框

（4）perms：权限过滤器，表示访问通过了url配置的资源会检查相应权限，例：“/statics/**=perms["user:add:*,user:modify:*"]“表示访问statics目录下的资源时只有新增和修改的权限

（5）port：端口过滤器，表示会验证通过了url配置的资源的请求的端口号，例：“/port.jsp=port[8088]”访问port.jsp时端口号不是8088会提示错误

（6）rest：restful类型过滤器，表示会对通过了url配置的资源进行restful风格检查，例：“/welcom=rest[user:create]”表示通过restful访问welcom资源时只有新增权限

（7）roles：角色过滤器，表示访问通过了url配置的资源会检查是否拥有该角色，例：“/welcom.jsp=roles[admin]”表示访问welcom.jsp页面时会检查是否拥有admin角色

（8）ssl：ssl过滤器，表示通过了url配置的资源只能通过https协议访问，例：“/welcom.jsp=ssl”表示访问welcom.jsp页面如果请求协议不是https会提示错误

（9）user：用户过滤器，表示可以使用登录验证/记住我的方式访问通过了url配置的资源，例：“/welcom.jsp=user”表示访问welcom.jsp页面可以通过登录验证或使用记住我后访问，否则直接跳转到登录

（10）logout：退出拦截器，表示执行logout方法后，跳转到通过了url配置的资源，例：“/logout.jsp=logout”表示执行了logout方法后直接跳转到logout.jsp页面

## 5、过滤器分类

（1）认证过滤器：anon、authcBasic、auchc、user、logout

（2）授权过滤器：perms、roles、ssl、rest、port
