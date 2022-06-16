### BeanFactory
�����ʵ������Spring Bean���������ӿڣ���Spring��������һ�׹淶���������ǳ��õ���getBean������

- �ýӿ��ж��巽����  
```java 
getBean(String name): Spring�����л�ȡ��ӦBean����ķ���������ڣ��򷵻ظö���
containsBean(String name)��Spring�������Ƿ���ڸö���
isSingleton(String name)��ͨ��beanName�Ƿ�Ϊ��������
isPrototype(String name)���ж�bean�����Ƿ�Ϊ��������
isTypeMatch(String name, ResolvableType typeToMatch):�ж�nameֵ��ȡ������bean��typeToMath�Ƿ�ƥ��
getType(String name)����ȡBean��Class����
getAliases(String name):��ȡname����Ӧ�����еı���
```
- ��Ҫ��ʵ����(����������)��
```java
AbstractBeanFactory������Bean���������󲿷ֵ�ʵ���࣬���Ǽ̳�����
DefaultListableBeanFactory:SpringĬ�ϵĹ�����
XmlBeanFactory��ǰ��ʹ��XML�����õıȽ϶��ʱ���õ�Bean����
AbstractXmlApplicationContext:����Ӧ�����������Ķ���
ClassPathXmlApplicationContext:XML���������Ķ����û�����Bean������������дSpring��ʱ���õľ�����
```

### FactoryBean
������SpringIOC��������Bean��һ����ʽ�����ַ�ʽ����Bean���мӳɷ�ʽ���ں��˼򵥵Ĺ������ģʽ��װ����ģʽ,FactoryBean�ӿںܼ򵥣����ṩ����������getObject��getObjectType��isSingleton����������������ȴ��Ϊ��spring�кܶ๦�ܵĻ�������������spring��Դ������ҵ��ܶ�FactoryBean������spring�����ṩ�����⣬�ں�һЩ��ܽ��м��ɵ�ʱ��ͬ����FactoryBean��Ӱ�ӣ������mybatis���ɵ�ʱ���SqlSessionFactoryBean��

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

        //���Student����
        Student stu=(Student) ac.getBean("myFactoryBean");
        //���MyFactoryBean����
        MyFactoryBean myFactoryBean=(MyFactoryBean) ac.getBean("&myFactoryBean");
        System.out.println("stu:"+stu);
        System.out.println("myFactoryBean:"+myFactoryBean);
    }
}

��ӡ�����
stu:Student{}
myFactoryBean:MyFactoryBean{}
```

��Щ�˾�Ҫ���ˣ���ֱ��ʹ��SpringĬ�Ϸ�ʽ����Bean����ô��Ϊɶ��Ҫ��FactoryBean��ɶ����ĳЩ����£�����ʵ��Bean����Ƚϸ��ӵ�����£�ʹ�ô�ͳ��ʽ����bean��Ƚϸ��ӣ����磨ʹ��xml���ã��������ͳ�����FactoryBean�ӿڣ��������û�ͨ��ʵ�ָýӿ����Զ����Bean�ӿڵ�ʵ�������̡�����װһ�㣬�����ӵĳ�ʼ�����̰�װ���õ����������ϵ����ʵ��ϸ�ڡ�

### ����
BeanFactory:���������͹���Bean��һ�������ӿڣ��ṩһ��Spring Ioc�����淶��  
FactoryBean: һ��Bean������һ�ַ�ʽ����Bean��һ����չ�����ڸ��ӵ�Bean�����ʼ������ʹ����ɷ�װ����Ĵ���ϸ�ڡ�  