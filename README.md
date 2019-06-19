# spring
#### IOC

控制反转：所谓控制反转，就是把原先我们代码里面需要实现的对象创建、依赖的代码，反转给容器来帮忙实现。那么必然的我们需要创建一个容器，同时需要一种描述来让容器知道需要创建的对象与对象的关系。这个描述最具体表现就是我们可配置的文件。 

#### DI

依赖注入：就是指对象是被动接受依赖类而不是自己主动去找，换句话说 就是指对象不是从容器中查找它依赖的类，而是在容器实例化对象的时候主动将它依赖的类注入给它。

#### BeanFactory

Spring Bean 的创建是典型的工厂模式，这一系列的 Bean 工厂，也即 IOC 容器为开发者管理对象间的依赖关系提供了很多便利和基础服务，在 Spring 中有许多的 IOC 容器的实现供用户选择和使用

BeanFactory有三个直接的子类：

ListableBeanFactory 				这些Bean是可列表的

HierarchicalBeanFactory 			这些Bean是有继承关系的

AutowireCapableBeanFactory		定义 Bean 的自动装配规则

```java
public interface BeanFactory {
    //对 FactoryBean 的转义定义，因为如果使用 bean 的名字检索 FactoryBean 得到的对象是工厂生成的对象，
    //如果需要得到工厂本身，需要转义
    String FACTORY_BEAN_PREFIX = "&";
    //根据 bean 的名字，在 IOC 容器中获取 bean 实例
    Object getBean(String name) throws BeansException;
    //根据 bean 的名字和 Class 类型来得到 bean 实例，增加了类型安全验证机制。
    <T> T getBean(String name, @Nullable Class<T> requiredType) throws BeansException;
    //根据名字和参数 在IOC容器中获取bean的实例
    Object getBean(String name, Object... args) throws BeansException;
    <T> T getBean(Class<T> requiredType) throws BeansException;
    //根据类型和参数 在IOC容器中获取bean的实例
    <T> T getBean(Class<T> requiredType, Object... args) throws BeansException;
    //提供对 bean 的检索，看看是否在 IOC 容器有这个名字的 bean
    boolean containsBean(String name);
    //根据 bean 名字得到 bean 实例，并同时判断这个 bean 是不是单例
    boolean isSingleton(String name) throws NoSuchBeanDefinitionException;
    boolean isPrototype(String name) throws NoSuchBeanDefinitionException;
    boolean isTypeMatch(String name, ResolvableType typeToMatch) throws NoSuchBeanDefinitionException;
    boolean isTypeMatch(String name, @Nullable Class<?> typeToMatch) throws NoSuchBeanDefinitionException;
    //得到 bean 实例的 Class 类型
    @Nullable
    Class<?> getType(String name) throws NoSuchBeanDefinitionException;
    //得到 bean 的别名，如果根据别名检索，那么其原名也会被检索出来
    String[] getAliases(String name);
}

```

​	在 BeanFactory 里只对 IOC 容器的基本行为作了定义，根本不关心你的 Bean 是如何定义怎样加载的。正如我们只关心工厂里得到什么的产品对象，至于工厂是怎么生产这些对象的，这个基本的接口不关心。

​	而要知道工厂是如何产生对象的，我们需要看具体的IOC容器实现，Spring 提供了许多 IOC 容器的实现。比如XmlBeanFactory，ClasspathXmlApplicationContext 等。其中XmlBeanFactory就是针对最基本的IOC容器的实现，这个 IOC 容器可以读取 XML 文件定义的 BeanDefinition（XML 文件 
中对 bean 的描述）,如果说XmlBeanFactory是容器中的屌丝，ApplicationContext 应该算容器中的高帅富.不过在Spring5.0中XmlBeanFactory已经标志为废弃。 
​	ApplicationContext是Spring提供的一个高级的IOC容器，它除了能够提供 IOC 容器的基本功能外，还为用户提供了以下的附加服务。

从 ApplicationContext 接口的实现，我们看出其特点： 
1.支持信息源，可以实现国际化。（实现 MessageSource 接口）

2.访问资源。(实现 ResourcePatternResolver 接口) 

3.支持应用事件。(实现 ApplicationEventPublisher 接口)

#### BeanDefinition

SpringIOC 容器管理了我们定义的各种 Bean 对象及其相互的关系，Bean 对象在 Spring 实现中是以 BeanDefinition来描述的

Bean 的解析过程非常复杂，功能被分的很细，因为这里需要被扩展的地方很多，必须保证有足够的灵活性，以应对可能的变化。Bean的解析主要就是对 Spring 配置文件的解析



#### IOC 容器的初始化

IOC 容器的初始化包括 BeanDefinition 的 Resource 定位、载入和注册这三个基本的过程。

