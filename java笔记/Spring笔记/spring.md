# Spring的bean生命周期

![1560423337681](C:\Users\mayn\AppData\Roaming\Typora\typora-user-images\1560423337681.png)

  **这Spring框架中，一旦把一个bean纳入到Spring IoC容器之中，这个bean的生命周期就会交由容器进行管理，一般担当管理者角色的是BeanFactory或ApplicationContext。认识一下Bean的生命周期活动，对更好的利用它有很大的帮助。**

​    下面以BeanFactory为例，说明一个Bean的生命周期活动：

- **Bean的建立**

​      由BeanFactory读取Bean定义文件，并生成各个实例。

- **Setter注入**

​      执行Bean的属性依赖注入。

- **BeanNameAware的setBeanName()**

​      如果Bean类实现了org.springframework.beans.factory.BeanNameAware接口，则执行其setBeanName()方法。

- **BeanFactoryAware的setBeanFactory()**

​      如果Bean类实现了org.springframework.beans.factory.BeanFactoryAware接口，则执行其setBeanFactory()方法。

- **BeanPostProcessors的processBeforeInitialization()**

​      容器中如果有实现org.springframework.beans.factory.BeanPostProcessors接口的实例，则任何Bean在初始化之前都会执行这个实例的processBeforeInitialization()方法。

- **InitializingBean的afterPropertiesSet()**

​      如果Bean类实现了org.springframework.beans.factory.InitializingBean接口，则执行其afterPropertiesSet()方法。

- **Bean定义文件中定义init-method**

​      在Bean定义文件中使用“init-method”属性设定方法名称，如下：

<bean id="demoBean" class="com.yangsq.bean.DemoBean" init-method="initMethod">   .......  </bean>

​      这时会执行initMethod()方法，注意，这个方法是不带参数的。

- **BeanPostProcessors的processAfterInitialization()**

​      容器中如果有实现org.springframework.beans.factory.BeanPostProcessors接口的实例，则任何Bean在初始化之前都会执行这个实例的processAfterInitialization()方法。

- **DisposableBean的destroy()**

​      在容器关闭时，如果Bean类实现了org.springframework.beans.factory.DisposableBean接口，则执行它的destroy()方法。

- **Bean定义文件中定义destroy-method**

​      在容器关闭时，可以在Bean定义文件中使用“destory-method”定义的方法

```xml
<bean id="demoBean" class="com.yangsq.bean.DemoBean" destory-method="destroyMethod">
  .......
</bean>
```

<bean id="demoBean" class="com.yangsq.bean.DemoBean" destory-method="destroyMethod">   ....... </bean>

​    这时会执行destroyMethod()方法，注意，这个方法是不带参数的。

   以上就是BeanFactory维护的一个Bean的生命周期。下面这个图可能更直观一些：

 **如果使用ApplicationContext来维护一个Bean的生命周期，则基本上与上边的流程相同，只不过在执行BeanNameAware的setBeanName()后，若有Bean类实现了org.springframework.context.ApplicationContextAware接口，则执行其setApplicationContext()方法，然后再进行BeanPostProcessors的processBeforeInitialization()**

   **实际上，ApplicationContext除了向BeanFactory那样维护容器外，还提供了更加丰富的框架功能，如Bean的消息，事件处理机制等。**