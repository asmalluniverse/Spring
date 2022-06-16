### BeanFactory
这个其实是所有Spring Bean的容器根接口，给Spring容器定义一套规范，比如我们常用到的getBean方法等

- 该接口中定义方法：  
```java 
getBean(String name): Spring容器中获取对应Bean对象的方法，如存在，则返回该对象
containsBean(String name)：Spring容器中是否存在该对象
isSingleton(String name)：通过beanName是否为单例对象
isPrototype(String name)：判断bean对象是否为多例对象
isTypeMatch(String name, ResolvableType typeToMatch):判断name值获取出来的bean与typeToMath是否匹配
getType(String name)：获取Bean的Class类型
getAliases(String name):获取name所对应的所有的别名
```
- 主要的实现类(包括抽象类)：
```java
AbstractBeanFactory：抽象Bean工厂，绝大部分的实现类，都是继承于他
DefaultListableBeanFactory:Spring默认的工厂类
XmlBeanFactory：前期使用XML配置用的比较多的时候用的Bean工厂
AbstractXmlApplicationContext:抽象应用容器上下文对象
ClassPathXmlApplicationContext:XML解析上下文对象，用户创建Bean对象我们早期写Spring的时候用的就是他
```

### FactoryBean
该类是SpringIOC容器创建Bean的一种形式，这种方式创建Bean会有加成方式，融合了简单的工厂设计模式和装饰器模式,FactoryBean接口很简单，就提供了三个方法getObject、getObjectType、isSingleton。就是这三个方法却成为了spring中很多功能的基础，搜索整个spring的源码可以找到很多FactoryBean，除了spring自身提供的以外，在和一些框架进行集成的时候，同样有FactoryBean的影子，比如和mybatis集成的时候的SqlSessionFactoryBean，

```java
@Component
public class MyFactoryBean implements FactoryBean<Student> {

    @Override
    public Student getObject() throws Exception {
        Student student=new Student();
        return student;
    }
    @Override
    public Class<?> getObjectType() {
        return null;
    }
    @Override
    public String toString() {
        return "MyFactoryBean{}";
    }
}

public class Student {
    private String name;
    private String code;

    public Student(String name, String code) {
        this.name = name;
        this.code = code;
    }
    public Student() {
    }
    @Override
    public String toString() {
        return "Student{}";
    }
}


@SpringBootApplication(exclude={DataSourceAutoConfiguration.class, HibernateJpaAutoConfiguration.class})
public class MyDemoApplication {
    public static void main(String[] args) {
        ApplicationContext ac=SpringApplication.run(MyDemoApplication.class, args);

        //获得Student对象
        Student stu=(Student) ac.getBean("myFactoryBean");
        //获得MyFactoryBean对象
        MyFactoryBean myFactoryBean=(MyFactoryBean) ac.getBean("&myFactoryBean");
        System.out.println("stu:"+stu);
        System.out.println("myFactoryBean:"+myFactoryBean);
    }
}

打印结果：
stu:Student{}
myFactoryBean:MyFactoryBean{}
```

有些人就要问了，我直接使用Spring默认方式创建Bean不香么，为啥还要用FactoryBean做啥，在某些情况下，对于实例Bean对象比较复杂的情况下，使用传统方式创建bean会比较复杂，例如（使用xml配置），这样就出现了FactoryBean接口，可以让用户通过实现该接口来自定义该Bean接口的实例化过程。即包装一层，将复杂的初始化过程包装，让调用者无需关系具体实现细节。

### 区别
BeanFactory:负责生产和管理Bean的一个工厂接口，提供一个Spring Ioc容器规范。  
FactoryBean: 一种Bean创建的一种方式，对Bean的一种扩展。对于复杂的Bean对象初始化创建使用其可封装对象的创建细节。  