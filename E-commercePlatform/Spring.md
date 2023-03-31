##### IoC(Inversion of Control)解决的核心问题：
- 谁负责创建组件
- 谁负责根据依赖关系组装组件
- 销毁时，如何按照依赖顺序正确销毁

在IoC模式下，控制权发生了反转，即从应用程序转移到了IoC容器，所有组件不再由应用程序自己创建和配置，而是由IoC容器负责，这样，应用程序只需要直接使用已经创建好并且配置好的组件。为了能让组件在IoC容器中被“装配”出来，需要某种“注入”机制，例如，BookService自己并不会创建DataSource，而是等待外部通过setDataSource()方法来注入一个DataSource
> 将组件的创建+配置与组件的使用相分离，并且，由IoC容器负责管理组件的生命周期

- 无侵入容器：应用程序无需实现Spring的特定接口，或者说，组件根本不知道自己在Spring的容器中运行。
    - 应用程序组件即可以在Spring的IoC容器中运行，也可以自己编写代码自行组装配置
    - 测试的时候并不依赖Spring容器，可单独进行测试，大大提高了开发效率

##### 装配Bean
一个具体的用户登陆注册的例子的工程结构：

![img.png](img.png)

![img_1.png](img_1.png)
- 每个<bean ...>都有一个id便是，相当于Bean的唯一ID
- 在userService Bean中，通过<property name="..." red=..."/>注入了另一个Bean
- Bean的顺序不重要，Spring根据依赖关系会自动正确初始化
把上述的XML配置文件用Java写出来，就想这样：
```java
UserService userService = new UserService();
MailService mailService = new MailService();
UserService.setMailService(mailService);
```
<font color="red">只不过Spring容器是通过读取XML文件后使用反射完成的</font>

如果注入的不是Bean，而是boolean、int、String这样的数据类型，则通过value注入，例如，创建一个HikariDataSource
![img_2.png](img_2.png)

最后，我们需要创建一个Spring的IoC容器实例，然后加在配置文件，让Spring容器为我们创建并装配好配置文件中指定的所有Bean，这只需要一行代码：
```java
ApplicationContext context=new ClassPathXmlApplicationContext("application.xml");
```
接下来，我们就可以从Spring容器中"取出"装配好的Bean然后使用它：
```java
UserService userService = context.getBean(UserService.class);
User user=userService.login("bob@example.com","password");
```
完整的main：
```java
public class Main{
    public static void main(String[] args){
        ApplicationContext context=new ClassPathXmlApplicationContext("application.xml");
        UserService user=context.getBean(UserService.class);
        User user=userService.login("bob@example.com","password");
        System.out.println(user.getName());
    }
}
```
- 我们可以看到Spring的容器就是ApplicationContext，它是一个接口，有很多实现类，这里我们选择ClassPathXmlApplicationContext，表示它会自动从classpath中查找指定的XML配置文件。
- 获得了ApplicationContext的实例，就获得了IoC容器的引用。从ApplicationContext中我们可以根据**Bean的ID**获取Bean，但更多时候我们是根据**Bean的类型**获取Bean的引用：
```java
UserService userService = context.getBean(UserService.class);
```

Spring还提供另一种IoC容器叫BeanFactory，使用方式和ApplicationContext蕾丝：
```java
BeanFactory factory = new XmlBeanFactory(new ClassPathResource("application.xml"));
MailService mailService=factory.getBean(MailService.class);
```
<font color="pink">BeanFactory和ApplicationContext的区别在于，BeanFactory的实现是按需创建，即第一次获取Bean时才创建这个Bean，而Application会一次创建所有的Bean。实际上ApplicationContext接口是从BeanFactory接口继承而来的，并且，ApplicationContext提供了一些额外的功能，包括国际化支持、事件和通知机制等。通常情况下，我们总是使用ApplicationContext，很少会使用BeanFactory</font>


