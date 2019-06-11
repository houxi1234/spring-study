# spring
IOC

控制反转：所谓控制反转，就是把原先我们代码里面需要实现的对象创建、依赖的代码，反转给容器来帮忙实现。那么必然的我们需要创建一个容器，同时需要一种描述来让容器知道需要创建的对象与对象的关系。这个描述最具体表现就是我们可配置的文件。 

DI

依赖注入：就是指对象是被动接受依赖类而不是自己主动去找，换句话说 就是指对象不是从容器中查找它依赖的类，而是在容器实例化对象的时候主动将它依赖的类注入给它。

BeanFactory

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

