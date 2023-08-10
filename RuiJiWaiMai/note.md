### 开发阶段
2. 基本需求
3. 移动端进行改进
4. 针对系统进行优化升级，提高系统访问性

![img.png](img.png)

 #### 数据库设计
一共涉及十一张表：
![img_1.png](img_1.png)

登陆界面：
![img_2.png](img_2.png)
![img_3.png](img_3.png)
退出功能：

#### 代码开发
后台登录功能：
1）创建实体类Employee和employee表进行映射，可以直接导入资料中提供的实体类
登陆处理逻辑：
1. 将页面的密码password进行md5加密处理
2. 根据页面提交的用户名username查询数据库
3. 如果没有查询到则返回登陆失败结果
4. 查看员工状态，如果为已禁用状态，则返回员工已禁用结果
5. 登陆成功，将员工id存入Session并返回登陆成功结果

#### 后台登出功能：
1. 获取Session并清除employee id
2. 返回退出成功结果

#### 员工管理：
![img_4.png](img_4.png)

<font color=red>完善登录功能：</font>
在之前即使没有登陆直接输入首页的url也可以访问 这显然是不合理的
<font color=red>使用过滤器或者拦截器实现 这里我们使用过滤器</font>

![img_5.png](img_5.png)
实现步骤：
1. 创建自定义过滤器LoginCheckFilter
2. 在启动类上加上@ServletComponentScan注解
3. 完善过滤器的处理逻辑

-----
<font color="red">注意URL和URI的区别：</font>
假设现在有一个名为JavaWeb的项目，其中有一个名为TestServlet的serlvet，其doGet方法为：
```java
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        System.out.println("URL:"+request.getRequestURL().toString());
        System.out.println("URI:"+request.getRequestURI());
        System.out.println("ServletPath:"+request.getServletPath());
        
        }
```
控制台的输出如下：
![img_6.png](img_6.png)
即在这个请求中，URL为 http://localhost:8080/JavaWeb/TestServlet ，URI为 /JavaWeb/TestServlet ， ServletPath为 /TestServlet ，这就是三者的区别。

----
#### 新增员工功能：
![img_7.png](img_7.png)
员工表：employee 
username有一个unique索引，status有一个默认值为1（1代表正常，0代表禁用）
![img_8.png](img_8.png)

![img_9.png](img_9.png)

- 创建一个全局的异常处理器
![img_10.png](img_10.png)

#### 员工信息分页查询
![img_11.png](img_11.png)
![img_12.png](img_12.png)

#### 启用禁用员工账号
需求分析
![img_13.png](img_13.png)

<font color="red">important!</font>
![img_14.png](img_14.png)

js只能保证前16位的精度 所以穿给后端的Long型id值出现了错误
如何解决这个问题：
我们可以在服务端给页面响应json数据时进行处理 将long型数据统一转为String字符串

#### 编辑员工信息
![img_15.png](img_15.png)
1. 回显
2. 编辑
3. 保存

![img_16.png](img_16.png)
![img_17.png](img_17.png)
需要把用户id取出来

#### 公共字段自动填充
![img_18.png](img_18.png)

![img_19.png](img_19.png)
自定义一个公共字段填充类：
![img_20.png](img_20.png)
传进来的metaObject：
![img_21.png](img_21.png)
程序会先根据@TableField的注解中的fill属性的值来决定是否需要自动填充，然后再更具insertFill等方法的内容来进行填充操作

在这个类里面无法获取到HttpServletRequest 因为没有被@Controller标记
所以获取用户的id是一个问题

![img_22.png](img_22.png)
> 客户端发送的每次http请求，对应在服务端都会分配一个新的线程来处理，涉及到的类中的方法都属于相同的一个线程

所以这个时候就要用到
ThreadLocal
> 什么是ThreadLocal
>ThreadLocal并不是一个Thread，而是thread的局部变量。当使用ThreadLocal维护变量时，ThreadLocal为每个使用该变量的线程提供独立的变量副本，所以每一个线程都可以独立的改变自己的副本，而不会影响其他线程所对应的副本。
> ThreadLocal为每个线程提供一份存储空间，具有线程隔离的效果，只有在线程内才能获取到对应的值，线程外则不能访问
![img_23.png](img_23.png)

- 实现步骤
![img_24.png](img_24.png)

#### 新增分类
![img_25.png](img_25.png)

![img_26.png](img_26.png)
- 基本的接口和结构
![img_27.png](img_27.png)
- 开发步骤
![img_28.png](img_28.png)
![img_29.png](img_29.png)

#### 删除分类
![img_30.png](img_30.png)


#### 修改分类
#### 菜品管理
![img_31.png](img_31.png)
#### 文件上传
![img_32.png](img_32.png)
![img_33.png](img_33.png)

进行文件转存时，可以在配置文件中定义转存路径：

![img_36.png](img_36.png)

![img_35.png](img_35.png)

![img_34.png](img_34.png)
#### 文件下载
![img_37.png](img_37.png)

#### 新增菜品
![img_38.png](img_38.png)

![img_39.png](img_39.png)
2 3步骤的功能已经在之前在CommonController实现过了

在进行新增菜品时 因为前端传来的数据涉及到Dish和DishFlavor连个实体类的内容 所以不能简单用Dish来接收参数，需要封装一个新的类DishDTO
> DTO(Data Transfer Object)数据传输对象，一般用于展示层与服务层之间的数据传输
 

![img_40.png](img_40.png)
<font color=red>这里前端提供的是json数据 所以一定要加@RequestBody注解</font>

在Request Header的content type里面查看
#### 菜品信息的分页查询
和前面差不多 引入了一个DishDtoInfoPage 因为Dish类里面没有CategoryName属性
#### 修改菜品
![img_41.png](img_41.png)

#### 套餐管理业务开发
![img_42.png](img_42.png)

![img_43.png](img_43.png)

![img_44.png](img_44.png)

![img_45.png](img_45.png)

#### 套餐信息分页查询
和前面差不多
#### 删除套餐
需要先停售再删除
![img_46.png](img_46.png)

#### 短信发送
使用阿里云短信服务
![img_61.png](img_61.png)
![img_62.png](img_62.png)
- 申请签名
- 申请模版
- 获取AccessKey（右上方头像）
- 权限授权
#### 导入地址簿
- 需求分析
- ![img_63.png](img_63.png)
- 数据表
![img_64.png](img_64.png)

#### 菜品展示
![img_65.png](img_65.png)

![img_66.png](img_66.png)

#### 购物车功能
![img_67.png](img_67.png)

- 交互
![img_68.png](img_68.png)

#### 用户下单
![img_69.png](img_69.png)

![img_70.png](img_70.png)

![img_71.png](img_71.png)

![img_72.png](img_72.png)

![img_73.png](img_73.png)

#### 部署
手动部署：直接打包成jar包 在linux使用java -jar XXX.jar运行
但这个方式会霸屏 后面需要改为后台运行

<font color=red>**nohup命令：**no hang up(不挂起) 用于不挂断地运行指定命令，推出终端不会影响程序的运行</font>

语法格式：nohup Command [Arg ...][&]

Command：要执行的命令
Arg：一些参数，可以指定输出文件
&：让命令在后台运行
举例：
nohup java -jar boot工程.jar &> hello.log &
ps -ef | grep java

可以kill -9 进程号来结束进程
> 在默认情况下，采用编号为15的TERM信号。TERM信号将终止所有不能捕获该信号的进程。对于那些可以捕获该信号的进程就要用编号为9的kill信号，强行“杀掉”该进程。 

##### 自动部署
1. 在Linux中安装Git
2. 在linux中安装maven
3. 编写shell脚本（拉取代码、编译、打包、启动）
4. 为用户授予执行Shell脚本的权限
5. 执行Shell脚本
![img_48.png](img_48.png)


<font color=red>为用户授权</font>
chmod change mode 控制用户对文件的权限的命令

-rw-r--r-- 分别代表 文件所有者 用户组 其他用户
![img_49.png](img_49.png)
比如： chmod 777 start.sh

- 设置静态ip：
![img_50.png](img_50.png)
红色部分是需要改的 要和虚拟机的ip对应

#### 优化
##### 缓存优化
![img_51.png](img_51.png)

##### 缓存短信验证码
![img_52.png](img_52.png)

#### 缓存菜品数据
![img_53.png](img_53.png)
按照分类来缓存数据
#####Spring Cache
- 介绍：
![img_54.png](img_54.png)
- 常用注解
![img_55.png](img_55.png)
<font color=red>在springboot项目中，使用缓存技术只需要在项目中导入相关缓存的技术依赖包，并在启动类上使用@EnableCaching开启缓存支持即可</font>

1. 改造DishController的list方法，先从Redis中获取菜品数据，如果有则直接返回，无需查询数据库；如果没有则查询数据库，并将查询到的1次品

![img_56.png](img_56.png)
value代表缓存的名称 每个缓存下面有多个key
![img_57.png](img_57.png)
#user.id #result.id 可以这样获得参数 动态构造key

这个缓存是基于内存的，一旦重启服务，数据就丢失了

![img_58.png](img_58.png)
如果要获取参数 这三种写法都可以

#### 在spring boot中使用spring cache的操作步骤（使用redis缓存技术
![img_59.png](img_59.png)

contion里面是没有#result的 unless里面有

- 实战
![img_60.png](img_60.png)

#### Mysql主从复制
Mysql主从复制是一个异步的复制过程，底层是基于Mysql数据库自带的**二进制日志**功能。就是一台或多台Mysql数据库（slave，即从库）从另一台Mysql数据库（master，即主库）进行日志的复制然后再解析日志并应用到自身，最终实现从库的数据和主库的数据保持一致。Mysql主从复制是Mysql数据库自带功能，无需借助第三方工具。

Mysql复制过程分为三步：
- master将改变记录到二进制日志（binary log）
- slave将master的binary log拷贝到它的中继日志（relay log）
- slave重做中继日志中的事件，将改变应用到自己的数据库中
![img_74.png](img_74.png)
> slave有一个专门的线程从主库的binary log读数据，然后写到自己的relay log
> master一般只有一个 slave一般有很多个

- 配置
![img_75.png](img_75.png)


- 主库配置

![img_76.png](img_76.png)
![img_77.png](img_77.png)
![img_78.png](img_78.png)

![img_79.png](img_79.png)

- 从库配置
![img_80.png](img_80.png)
![img_81.png](img_81.png)
![img_82.png](img_82.png)

#### 读写分离
![img_83.png](img_83.png)

**sharding-JDBC介绍**
![img_84.png](img_84.png)

![img_85.png](img_85.png)

![img_87.png](img_87.png)
读写分离策略：
round_robin 轮询

> 定义了配置以后他就会自动判断查询时使用从库，增删改时使用主库

![img_88.png](img_88.png)
![img_89.png](img_89.png)
> 在配置文件中允许bean定义覆盖配置项 因为Druid和shardingjdbc会创建两个名字相同的bean会产生冲突，打开这个意味着后创建的可以覆盖先创建的，避免了冲突

####Nginx
![img_90.png](img_90.png)
- Nginix命令
![img_92.png](img_92.png)

![img_93.png](img_93.png)

![img_94.png](img_94.png)

![img_95.png](img_95.png)

 #### Nginx具体应用
![img_96.png](img_96.png)
> 红色部分是固定写法 黑色部分可以自己改


使用Nginx部署静态资源比使用tomcat部署更高效

**反向代理**
![img_98.png](img_98.png)

![img_97.png](img_97.png)
客户无感知

- 配置反向代理
![img_99.png](img_99.png)
配置在反向代理服务器里面

**负载均衡**
![img_100.png](img_100.png)
基于反向代理来实现的 只不过负载均衡后面有很多台服务器 并通过负载均衡算法来进行分发
![img_101.png](img_101.png) 

- 默认轮询算法

**负载均衡算法**
![img_103.png](img_103.png)

![img_102.png](img_102.png)

#### 前后端分离
![img_104.png](img_104.png)

![img_105.png](img_105.png)

![img_107.png](img_107.png)

![img_108.png](img_108.png)

**接口（API接口）：就是一个http的请求地址，主要就是去定义：请求路径、请求方式、请求参数、响应数据等**
![img_109.png](img_109.png)

##### YApi
- 介绍

需要自己部署
![img_111.png](img_111.png)

##### Swagger
![img_112.png](img_112.png)

使用knife4j，底层集成了Swagger
![img_113.png](img_113.png)

![img_114.png](img_114.png)
注意填入Controller包路径

![img_115.png](img_115.png)

![img_116.png](img_116.png)

- 常用注解
![img_117.png](img_117.png)

**在类上使用@Api**
![img_118.png](img_118.png)

**在各个方法上使用@ApiOperation**
![img_119.png](img_119.png)

**property的说明**
![img_120.png](img_120.png)

##### 项目部署
- 部署架构
![img_121.png](img_121.png)

Nginx部署前端和反向代理 Tomcat部署后端

<font color="red">Springboot内置Tomcat 所以直接打包运行jar包就行</font>
> 要使用内置Tomcat，您只需要在Spring Boot项目的依赖中包含spring-boot-starter-web，Spring Boot会自动处理内置Tomcat的配置和启动。

- 部署环境说明
![img_122.png](img_122.png)
- 前端部署
![img_123.png](img_123.png)

在使用Nginx进行反向代理时 url路径往往和后端请求不一致 所以需要配置重写
> rewrite ^/api/(.*)$ /$1 break; 
> 
> rewrite：这是nginx中用于执行URL重写操作的指令。
1. ^/api/(.*)$：这是一个正则表达式模式，匹配以 /api/ 开头的URL路径。(.*) 表示捕获组，匹配任意字符。
2. /$1：这是重写的目标，它使用捕获组 $1 中的内容来构建新的URL路径。在这里，捕获组中的内容是去掉了 /api/ 的部分，所以新的路径就是去掉了 /api/ 的部分。
3. break：这是重写的标志，表示只执行当前的重写操作，不再继续匹配其他重写规则。
例如，如果有一个请求 /api/resource，根据这条nginx配置，它将被重写为 /resource。

![img_124.png](img_124.png)


### 遇到的问题
- springboot要和jdk的版本号对应 jdk1.8对应的是springboot 2.7.8
- BeanUtils.copyProperties(Object source,Object target);必须是两个对象 不能是两个list
- 当前端不是以JSon形式传过来很多相同类型的数据时 后端如果要接收为List 必须加Requetparam注解 springboot会帮忙自动将传过来的参数转为List
- 在使用spring cache注解缓存时 方法返
- 回的对象必须是可序列化的 所以我们自己定义的返回对象R必须实现序列化 加上 implements serializable