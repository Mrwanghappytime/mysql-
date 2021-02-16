**1.注解是什么**
    不是程序本身，可以对程序做出解释，但是可以被编译器识别，可以通过反射的方式进行访问注解
    注解可以使用@注解在程序中体现，还可以添加一些变量
    注解可以用于修饰方法，包，类，变量等信息
    TYPE, FIELD , METHOD , PARAMETER , CONSTRUCT , LOCAL_VARIABLE , ANNOTATION_TYPE
    PACKAGE
    
**2.注解的作用时间**
    1.编译阶段（例如Override注解）SOURCE
    2.运行阶段（）    CLASS
    2.两者皆可以（）
**3.注解的定义**
    public @interface 注解名称{
    }
    此时会默认继承Annotation注解，该注解
**4.元注解**
    jdk自带的注解，用于解释其他注解
    1.@Target注解，用于标识注解使用的地方
    2.@Retention注解，用于标识注解的使用时间，生命周期，其中source只在源码上进行保留进行使用，例如Override注解
        CLASS 注解旨在编译时注解，RUNTIME 表示运行时注解，会被加载至JVM
    3.@Document 用于注解类，将元素包含到javadoc中
    4.@Inherited 用于注解类，表示继承的时候，注解也被继承
    5.@Repeatable 后续进行理解
**5.注解的属性**
    注解的属性也叫成员变量，注解只有成员变量，没有方法，以无参的方式进行使用
    成员变量可以有默认值，如 int msg() default -1;
    当注解只有只有一个成员变量时，可以直接进行赋值使用
**6. jdk自带的一些注解**
    @Deprecated
    @Override
    @SafeVarargs
    @FunctionalInterface函数式编程注解
**7.注解的作用原理（反射）**
    

