---
title: "多态练习"
date: 2026-09-15
slug: "duotai test"
categories: ["JAVA学习"]
draft: false
---

> 本文记录了一个关于 Java **多态（Polymorphism）** 的经典练习案例。通过“人饲养动物”的场景，演示了父类作为方法参数接收不同子类对象、方法重写，以及使用新版 `instanceof` 模式匹配进行类型转换的技巧。

---

## 一、 基础父类：Animal 🐾

定义一个通用的动物父类，包含基本属性（年龄、颜色）和一个普通的 `eat` 方法，供子类重写。

<details class="code-collapse">
<summary>点击查看 Animal.java 源码</summary>

```java
package me.sssnian.polymorphismdemo;

public class Animal {
    private int age;
    private String color;

    public Animal() {
    }

    public Animal(int age, String color) {
        this.age = age;
        this.color = color;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public String getColor() {
        return color;
    }

    public void setColor(String color) {
        this.color = color;
    }

    public void eat(String something) {
        System.out.println("动物在吃" + something);
    }
}
```
</details>


## 二、 具体子类：Dog 与 Cat 🐶🐱

继承自 `Animal` 类，重写 `eat` 方法，体现出不同动物进食的独特行为；同时各自拥有特有的行为方法（如看家、抓老鼠）。

### 1. 狗类 (Dog)

<details class="code-collapse">
<summary>点击查看 Dog.java 源码</summary>

```java
package me.sssnian.polymorphismdemo;

public class Dog extends Animal {
    public Dog() {
    }

    public Dog(int age, String color) {
        super(age, color);
    }

    @Override
    public void eat(String something) {
        System.out.println(getAge() + "岁的" + getColor() + "颜色的狗两只前腿死死的抱住" + something + "猛吃");
    }

    public void lookhome() {
        System.out.println("狗在看家");
    }
}
```
</details>

### 2. 猫类 (Cat)

<details class="code-collapse">
<summary>点击查看 Cat.java 源码</summary>

```java
package me.sssnian.polymorphismdemo;

public class Cat extends Animal {
    public Cat() {
    }

    public Cat(int age, String color) {
        super(age, color);
    }

    @Override
    public void eat(String something) {
        System.out.println(getAge() + "岁的" + getColor() + "颜色的猫眯着眼睛侧着头吃" + something);
    }

    public void catchMouse(){
        System.out.println("猫抓老鼠");
    }
}
```
</details>


## 三、 核心逻辑类：Person 🧑

人类负责饲养宠物。这里是体现**多态**核心的地方：`keepPet` 方法接收的参数是 `Animal` 父类，而不是具体的某一种动物。

> **💡 亮点说明：**
> 代码中巧妙使用了 **JDK 16 引入的 `instanceof` 模式匹配**（Pattern Matching）。
> 过去我们需要先判断 `a instanceof Dog`，然后在内部强转 `Dog d = (Dog) a;`。现在可以直接写成 `if(a instanceof Dog d)`，让代码更加简洁优雅！

<details class="code-collapse">
<summary>点击查看 Person.java 源码</summary>

```java
package me.sssnian.polymorphismdemo;

public class Person {
    private String name;
    private int age;

    public Person() {
    }

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    /* 
    // 传统方式：需要为每一种动物重载一个方法，代码冗余
    public void keepPet(Dog dog, String something) {
        System.out.println("年龄为" + age + "岁的" + name + "养了一只" + dog.getColor() + "颜色的" + dog.getAge() + "岁的狗");
        dog.eat(something);
    }

    public void keepPet(Cat cat, String something) {
        System.out.println("年龄为" + age + "岁的" + name + "养了一只" + cat.getColor() + "颜色的" + cat.getAge() + "岁的猫");
        cat.eat(something);
    }
    */

    // 多态方式：利用父类类型接收子类对象，结合 instanceof 模式匹配实现灵活调用
    public void keepPet(Animal a, String something) {
        if(a instanceof Dog d){
            System.out.println("年龄为" + age + "岁的" + name + "养了一只" + d.getColor() + "颜色的" + d.getAge() + "岁的狗");
            d.eat(something);
        } else if (a instanceof Cat c) {
            System.out.println("年龄为" + age + "岁的" + name + "养了一只" + c.getColor() + "颜色的" + c.getAge() + "岁的猫");
            c.eat(something);
        }else{
            System.out.println("没有这种动物");
        }
    }
}
```
</details>


## 四、 测试运行：Test 🚀

创建实体对象，将具体的狗和猫传入以 `Animal` 作为参数的方法中，触发多态行为。

<details class="code-collapse">
<summary>点击查看 Test.java 源码</summary>

```java
package me.sssnian.polymorphismdemo;

public class Test {
    public static void main(String[] args) {
        Person p = new Person("zqy", 20);
        Dog d = new Dog(2, "黑");
        Cat c = new Cat(2, "黄");
        
        // 多态体现：以父类 Animal 接收具体的 Dog / Cat 子类对象
        p.keepPet(d, "骨头");
        p.keepPet(c, "鱼");
    }
}
```
</details>