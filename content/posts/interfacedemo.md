---
title: "接口和抽象类"
date: 2026-09-16
slug: "interfacedemo"
categories: ["JAVA学习"]
draft: false
---

> 本文记录了一个关于 Java **抽象类（Abstract Class）与接口（Interface）** 的经典练习案例。通过“运动员与教练”的场景，演示了如何使用抽象类提取事物的**共性**（如姓名、年龄、教/学），以及如何使用接口来扩展特定角色的**额外技能**（如部分人员需要学英语）。

---

## 一、 顶层抽象父类：Person 🧑

定义所有人的通用属性。因为直接实例化一个“人”没有具体的业务意义，所以将其定义为 `abstract` 抽象类，防止被直接创建对象。

<details class="code-collapse">
<summary>点击查看 Person.java 源码</summary>

```java
package me.sssnian.interfacedemo;

//因为创建person对象没有任何意义,所以把person变成抽象类不让被创建
public abstract class Person {
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
}
```
</details>

---

## 二、 角色抽象类：Coach 与 Sporter 🏃‍♂️

继承自 `Person`，并根据角色的不同，定义了各自特有的抽象行为（教练负责教 `teach`，运动员负责学 `study`）。

### 1. 教练类 (Coach)

<details class="code-collapse">
<summary>点击查看 Coach.java 源码</summary>

```java
package me.sssnian.interfacedemo;

public abstract class Coach extends Person {
    public Coach() {
    }

    public Coach(String name, int age) {
        super(name, age);
    }

    public abstract void teach();
}
```
</details>

### 2. 运动员类 (Sporter)

<details class="code-collapse">
<summary>点击查看 Sporter.java 源码</summary>

```java
package me.sssnian.interfacedemo;

public abstract class Sporter extends Person{
    public Sporter() {
    }

    public Sporter(String name, int age) {
        super(name, age);
    }

    public abstract void study();
}
```
</details>

---

## 三、 技能扩展接口：English 🗣️

在 Java 中，类是单继承的，但可以通过实现接口来扩展额外的能力。并非所有教练和运动员都需要说英语，因此将“说英语”定义为接口，供需要的人实现。

<details class="code-collapse">
<summary>点击查看 English.java 源码</summary>

```java
package me.sssnian.interfacedemo;

public interface English {
    public abstract void speakEnglish();
}
```
</details>

---

## 四、 具体实现类 (Concrete Classes) 🏀🏓

### 1. 篮球系列（无英语需求）

篮球教练和篮球运动员只需继承对应的抽象父类，重写核心方法即可。

<details class="code-collapse">
<summary>点击查看 篮球教练与运动员 源码</summary>

```java
// 篮球教练类
package me.sssnian.interfacedemo;

public class BasketballCoach extends Coach{
    public BasketballCoach() {
    }

    public BasketballCoach(String name, int age) {
        super(name, age);
    }

    @Override
    public void teach() {
        System.out.println("篮球教练正在教如何打篮球");
    }
}

// 篮球运动员类
package me.sssnian.interfacedemo;

public class BasketballSporter extends Sporter{
    public BasketballSporter() {
    }

    public BasketballSporter(String name, int age) {
        super(name, age);
    }

    @Override
    public void study() {
        System.out.println("篮球运动员在学习如何打篮球");
    }
}
```
</details>

### 2. 乒乓球系列（有英语需求）

乒乓球教练和运动员不仅要继承父类，还需要 `implements English` 接口，并重写 `speakEnglish` 方法。

<details class="code-collapse">
<summary>点击查看 乒乓球教练与运动员 源码</summary>

```java
// 乒乓球教练类
package me.sssnian.interfacedemo;

public class PingPongCoach extends Coach implements English{
    public PingPongCoach() {
    }

    public PingPongCoach(String name, int age) {
        super(name, age);
    }

    @Override
    public void teach() {
        System.out.println("乒乓球教练正在教如何打乒乓球");
    }

    @Override
    public void speakEnglish() {
        System.out.println("乒乓球教练在学习说英语");
    }
}

// 乒乓球运动员类
package me.sssnian.interfacedemo;

public class PingPongSporter extends Sporter implements English{
    public PingPongSporter() {
    }

    public PingPongSporter(String name, int age) {
        super(name, age);
    }

    @Override
    public void speakEnglish() {
        System.out.println("乒乓球运动员在说英语");
    }

    @Override
    public void study() {
        System.out.println("乒乓球运动员在学习如何打乒乓球");
    }
}
```
</details>

---

## 五、 测试运行：Test 🚀

实例化具体的子类对象，测试抽象方法重写和接口实现的效果。

<details class="code-collapse">
<summary>点击查看 Test.java 源码</summary>

```java
package me.sssnian.interfacedemo;

public class Test {
    static void main() {
        PingPongSporter pps = new PingPongSporter("王楚钦",23);
        System.out.println(pps.getName()+", "+pps.getAge());
        pps.study();
        pps.speakEnglish();

        BasketballCoach bbc= new BasketballCoach("神秘人",50);
        System.out.println(bbc.getName()+", "+bbc.getAge());
        bbc.teach();
    }
}
```
</details>