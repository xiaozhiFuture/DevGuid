### Java8-(4种方式)实现List元素的排序

>先来一个故事背景把，咱们现在在给一位农民伯伯设计一个苹果库存管理系统。他现在有这样的一个需求：想要对苹果库存里面的所有苹果，按照苹果的重量weight排序。这里咱们简单的使用List集合作为我们的仓库，快来看看如何一步一步的实现的更加简洁吧。
>
**先来一个Apple类**
```Java
public class Apple {
    private String color;
    private Double weight;

    public Apple(String color, Double weight) {
        this.color = color;
        this.weight = weight;
    }

    public String getColor() {
        return color;
    }

    public void setColor(String color) {
        this.color = color;
    }

    public Double getWeight() {
        return weight;
    }

    public void setWeight(Double weight) {
        this.weight = weight;
    }

    @Override
    public String toString() {
        return "Apple{" +
                "color='" + color + '\'' +
                ", weight=" + weight +
                '}';
    }
}
```
#### 一 传递代码

在Java8的API中已经为我们提供了一个List.sort的排序算法，这一部分已经不需要我们自己去实现它了。但是如何将排序策略传递给sort方法呢，我们可以先看看sort的方法签名
> void sort(Comparator<? super E> c)
> 
它需要一个Comparator对象来比较两个Apple。这就是Java中的策略传递方式：它们必须包裹在一个对象里面。我们说sort的行为被参数化了：传递给它们不同的排序策略不同，其行为也会不同。所以我们看看最原始的实现代码如何
**先定义一个比较器（排序策略）**
```Java
package com.zxy.basic;
import java.util.Comparator;
//这是apple的比较器
public class AppleComparator implements Comparator<Apple> {
    @Override
    public int compare(Apple apple1, Apple apple2) {
        return apple2.getWeight().compareTo(apple1.getWeight());
    }
}
```
**测试实现**
```Java
import java.util.ArrayList;
import java.util.List;
public class TransferCode {

    static List<Apple> apples = new ArrayList<>();

    static{
        apples.add(new Apple("Red",10D));
        apples.add(new Apple("Green",5D));
        apples.add(new Apple("Black",7D));
        apples.add(new Apple("Green",15D));
    }
    public static void main(String[] args) {
        System.out.println(apples);
        apples.sort(new AppleComparator());
        System.out.println(apples);
    }
}
```
#### 二 使用匿名类

**我们都知道，匿名类的出现是为了改善为了一个接口要声明好几个实现类的啰嗦问题。** 所以上面的那个例子是完全可以使用匿名类，避免创建多余的AppleComparator比较器的问题。

直接看关键部分，套路代码省略
```Java
    public static void main(String[] args) {
        System.out.println(apples);
        //在这里使用了匿名类
        apples.sort(new Comparator<Apple>() {
            @Override
            public int compare(Apple apple1, Apple apple2) {
                return apple2.getWeight().compareTo(apple1.getWeight());
            }
        });
        System.out.println(apples);
    }
```
#### 三 使用Lambda表达式

但是上面实现的匿名类的方法依然看上去还是有点啰里啰唆不够简洁的。这时候Java8为我们提供了Lambda表达式，它提供了一种轻量级的语法来实现相同的目标:**传递代码**，你看到了，在需要**函数式接口（仅仅定义了一个抽象方法的接口。抽象方法的签名称为函数描述符，描述了Lambda表达式的签名）** 的地方可以使用Lambda表达式。

直接看关键代码，套路代码省略
``` Java
   public static void main(String[] args) {
        System.out.println(apples);
        apples.sort((Apple apple1,Apple apple2)->apple2.getWeight().compareTo(apple1.getWeight()));
        System.out.println(apples);
    }
```
#### 四 使用方法引用

接下来再来看看方法引用这个语法糖是如何实现的呢？
```Java
public static void main(String[] args) {
        System.out.println(apples);
        apples.sort(comparing(Apple::getWeight).reversed());
        System.out.println(apples);
    }
```

#### 总结

可能很多小伙伴要问了，为什么一个简单的不行的集合元素的排序，你要写这么多实现的方式。我就用第一种传递代码的方式不好吗？我习惯了这种写法了，不想学习新的语法格式了，况且第一种实现的方式功能也是可以正常运行的呀。 这种问题其实笔主再以前也是这么想的，能实现需求就完事儿了我管他什么实现方式呢。但是既然Java8引入了Lambda表达式这种较大的语法改动，说明了Java它是再不断的演变的。

>**语言需要不断改进以跟进硬件的更新或满足程序员的期待（如果你还不够信服，想想COBOL还一度是商业上最重要的语言之一呢）。要坚持下去，Java必须通过增加新功能来改进，而且只有新功能被人使用，变化才有意义。所以，使用Java 8，你就是在保护你作为Java程序员的职业生涯。**
>


**最后文中有不足的地方，欢迎各位评论其留言指正交流。**

