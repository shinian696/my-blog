---
title: "[01]"
date: 2026-08-11
slug: "java01"
categories: ["JAVA学习"]
draft: false
---

---

## switch的一些特性

#### 1.正常形式
<details class="code-collapse">
<summary>代码</summary>

```java
switch (a) {
    case 1: {
        System.out.println("一");
        break;
    }
    case 2: {
        System.out.println("二");
        break;
    }
    case 3: {
        System.out.println("三");
        break;
    }
    default : {
        System.out.println("无");
        break;
    }
```
</details>

---

#### 2.精简(java12 新特性)
<details class="code-collapse">
<summary>代码</summary>

```java
switch (a) {
    case 1 -> System.out.println("一");
    case 2 -> System.out.println("二");
    case 3 -> System.out.println("三");
    default -> System.out.println("无");
}
```
</details>

---

#### 3.枚举
<details class="code-collapse">
<summary>代码</summary>

```java
switch (a) {
    case 1,2,3,4,5 -> System.out.println("工作日");
    case 6,7 -> System.out.println("休息日");
    default -> System.out.println("没有这个星期");
}
```
</details>

---

