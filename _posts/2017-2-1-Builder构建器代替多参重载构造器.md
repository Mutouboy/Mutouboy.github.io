---  
layout: post
title: Builder构建器
category: Java
---

当一个类拥有多个参数，特别是其中一大部分是可选参数时，其构造器的创建就变得麻烦起来。在面对这种情况时，一般有三种方式。  

### 重叠构造器  
 
构造多个有参构造器，通过构造器的参数不同进行构造器的重载，在调用时根据参数的不同选择不同的构造器。例子如下：  

```java
public class ConstructDemo {
    private  int A;
    private  int B;
    private  int C;
    
    public ConstructDemo(){}
    
    public ConstructDemo(int val){
        A=val;
    }
    public ConstructDemo(int val1,int val2){
        A=val1;
        B=val2;
    }
    public ConstructDemo(int val1,int val2,int val3){
        A=val1;
        B=val2;
        C=val3;
    }
}
```  
这种方法繁杂，当参数很多时不利于代码的维护，程序员需要仔细阅读相关构造器和参数才能理解。  

### JavaBean  

第二种方法是使用JavaBean模式，使用无参构造器构造对象，然后通过setter()方法对参数进行设置。例子如下：  

```java
public class JavaBeanDemo {
    private  int A;
    private  int B;
    private  int C;
    
    public JavaBeanDemo(){}

    public void setA(int a) {
        A = a;
    }

    public void setB(int b) {
        B = b;
    }

    public void setC(int c) {
        C = c;
    }
}
```
  
这种方法与重载构造器方法相比，在存在大量可选参数时无疑要更方便一些，但带来的坏处是对象的构建过程被打散到多个方法调用中，在这个过程中JavaBean可能处于不一致的状态，且这种方法会使得对象可变，不利于线程安全。  

### Builder构建器  

Builder构建器融合了前两种方法的优点，既有利于线程安全同时拥有姣好的可读性。**Build模式**是在类中存在一个Build内部类，不直接生成想要类的对象，而是通过这个内部类进行生成。过程如下：  
1. 使用必要的参数去调用Build的构造器得到一个Build对象  
2. 使用这个Build对象调用各个可选参数的方法，对参数赋值后再返回该对象  
3. 调用无参的build()方法返回调用想要类的构造器，构造器参数为该build对象。  

代码参考：  

```java
public class BuilderDemo {
    //必选参数A
    private final int A;
    //可选参数B，C
    private final int B;
    private final int C;

    public static class Builder{
        //必选参数A
        private final int A;
        //可选参数B，C
        private int B;
        private int C;

        public Builder(int a) {
            this.A = a;
        }

        public Builder SetB(int val){
            this.B = val;return this;
        }

        public Builder SetC(int val){
            this.C = val;return this;
        }

        public BuilderDemo build(){
            return new BuilderDemo(this);
        }
    }

    public BuilderDemo(Builder builder) {
        A = builder.A;
        B = builder.B;
        C = builder.C;
    }

    @Override
    public String toString() {
        return "BuilderDemo{" +
                "A=" + A +
                ", B=" + B +
                ", C=" + C +
                '}';
    }
}
```

测试代码：

```java
public class Text {
    public static void main(String[] args){
        BuilderDemo b=new BuilderDemo.Builder(1).SetB(2).SetC(3).build();
        System.out.print(b);
    }
}
```

结果：

```
BuilderDemo{A=1, B=2, C=3}
```

Builder构建器同样存在缺点：  

- 必须先构建build对象，在对性能要求严格的情况下会成为问题  
- 代码冗长

因此，build模式只应该在参数很多的情况下使用，在你写代码时应该考虑以后是否会添加参数的个数，从而考虑是否一开始便采用构建器方法。
