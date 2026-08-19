---
title: "学生管理与用户认证系统"
date: 2026-08-19
slug: "javastu"
categories: ["JAVA学习"]
draft: false
---

> 本文记录了一个基于 Java 编写的控制台综合案例。项目主要分为两大模块：**学生信息管理模块**与**用户认证模块**。包含了增删改查、输入校验、随机验证码生成等经典逻辑。

---

## 一、 学生信息管理模块

该模块负责对学生数据的核心管理，包含基础的 CRUD（增删改查）操作。

### 1. Student 实体类

* **核心属性**：`id` (学号)、`name` (姓名)、`age` (年龄)、`address` (家庭住址)

<details class="code-collapse">
<summary>点击查看 Student.java 源码</summary>

```java
package me.sssnian.studentSystem;

public class Student {
    private String id;
    private String name;
    private int age;
    private String address;

    public Student() {
    }

    public Student(String id, String name, int age, String address) {
        this.id = id;
        this.name = name;
        this.age = age;
        this.address = address;
    }

    public String getId() {
        return id;
    }

    public void setId(String id) {
        this.id = id;
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

    public String getAddress() {
        return address;
    }

    public void setAddress(String address) {
        this.address = address;
    }
}
```
</details>

### 2. 系统功能描述

管理系统通过控制台菜单进行交互，主要包含以下核心功能：

* **➕ 添加学生**：录入学生信息（要求：`id` 必须唯一，若重复需重新输入）。
* **➖ 删除学生**：录入要删除的 `id`（要求：`id` 存在则删除；不存在则提示并返回菜单）。
* **📝 修改学生**：录入要修改的 `id`（要求：`id` 存在则继续录入新信息；不存在则提示并返回）。
* **🔍 查询学生**：打印所有学生信息。若当前无数据，提示添加；若有数据，按如下格式规范输出：

```text
id          姓名        年龄        家庭住址
heima001    张三        23         南京
heima002    李四        24         北京
heima003    王五        25         广州
heima004    赵六        26         深圳
```

<details class="code-collapse">
<summary>点击查看 StudentSystem.java 源码</summary>

```java
package me.sssnian.studentSystem;

import java.util.ArrayList;
import java.util.Scanner;

public class StudentSystem {
    static void startStudentSystem() {
        ArrayList<Student> list = new ArrayList<>();
        loop:
        while (true) {
            System.out.println("———————————————————————欢迎来到学生管理系统———————————————————————————");
            System.out.println("1:添加学生");
            System.out.println("2:删除学生");
            System.out.println("3:修改学生");
            System.out.println("4:查询学生");
            System.out.println("5:退出");
            System.out.println("请输入你的选择");
            Scanner sc = new Scanner(System.in);
            String choose = sc.next();
            switch (choose) {
                case "1" -> addStudent(list);
                case "2" -> deleteStudent(list);
                case "3" -> updateStudent(list);
                case "4" -> queryStudent(list);
                case "5" -> {
                    System.out.println("退出");
                    break loop;
                }
                default -> System.out.println("没有这个选项");
            }
        }


    }

    //添加学生
    static void addStudent(ArrayList<Student> list) {
        Student stu = new Student();
        Scanner sc = new Scanner(System.in);
        String id = null;
        while (true) {
            System.out.println("请输入学生的id");
            id = sc.next();
            boolean flag = contains(list, id);
            if (flag) {
                System.out.println("id已经存在,请重新录入");
            } else {
                stu.setId(id);
                break;
            }
        }


        System.out.println("请输入学生的姓名");
        String name = sc.next();
        stu.setName(name);

        System.out.println("请输入学生的年龄");
        int age = sc.nextInt();
        stu.setAge(age);

        System.out.println("请输入学生的家庭住址");
        String address = sc.next();
        stu.setAddress(address);

        list.add(stu);
        System.out.println("学生信息添加成功");
    }

    //删除学生
    static void deleteStudent(ArrayList<Student> list) {
        Scanner sc = new Scanner(System.in);
        System.out.println("请输入要删除的id");
        String id = sc.next();
        int index = getIndex(list, id);
        if(index>=0){
            list.remove(index);
            System.out.println("id为"+id+"的学生删除成功");
        }
        else{
            System.out.println("id不存在,删除失败");
        }
    }

    //修改学生
    static void updateStudent(ArrayList<Student> list) {
        Scanner sc=new Scanner(System.in);
        System.out.println("请输入要修改学生的id");
        String id = sc.next();

        int index = getIndex(list, id);
        if(index==-1){
            System.out.println("要修改的id"+id+"不存在,请重新输入");
            return;
        }

        Student stu = list.get(index);

        System.out.println("请输入的要修改的学生姓名");
        String newName = sc.next();
        stu.setName(newName);

        System.out.println("请输入要修改的学生年龄");
        int newAge = sc.nextInt();
        stu.setAge(newAge);

        System.out.println("请输入要修改的学生家庭住址");
        String newAddress = sc.next();
        stu.setAddress(newAddress);

        System.out.println("学生信息修改成功");
    }

    //查询学生
    static void queryStudent(ArrayList<Student> list) {
        if (list.size() == 0) {
            System.out.println("当前无学生信息,请添加后再查询");
            return;
        }
        System.out.println("id\t\t姓名\t年龄\t家庭住址");
        for (int i = 0; i < list.size(); i++) {
            Student student = list.get(i);
            System.out.println(student.getId() + "\t" + student.getName() + "\t" +
                    student.getAge() + "\t" + student.getAddress());
        }
    }

    //判断id是否存在
    public static boolean contains(ArrayList<Student> list, String id) {
        int index = getIndex(list, id);
        return index >= 0;
    }

    //id所在的索引
    public static int getIndex(ArrayList<Student> list, String id) {
        for (int i = 0; i < list.size(); i++) {
            Student stu = list.get(i);
            String sid = stu.getId();
            if (sid.equals(id)) return i;
        }
        return -1;
    }
}
```
</details>

---

## 二、 用户认证与管理模块

为了保障学生系统的安全性，外层嵌套了用户认证体系，包含注册、登录、找回密码及各种严格的数据校验规则。

### 1. User 实体类

* **核心属性**：`username` (用户名)、`password` (密码)、`personId` (身份证号码)、`phoneNumber` (手机号码)

<details class="code-collapse">
<summary>点击查看 User.java 源码</summary>

```java
package me.sssnian.studentSystem;

public class User {
    private String username;
    private String password;
    private String personId;
    private String phoneNumber;

    public User() {
    }

    public User(String username, String password, String personId, String phoneNumber) {
        this.username = username;
        this.password = password;
        this.personId = personId;
        this.phoneNumber = phoneNumber;
    }

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }

    public String getPersonId() {
        return personId;
    }

    public void setPersonId(String personId) {
        this.personId = personId;
    }

    public String getPhoneNumber() {
        return phoneNumber;
    }

    public void setPhoneNumber(String phoneNumber) {
        this.phoneNumber = phoneNumber;
    }
}
```
</details>

### 2. 核心功能与业务逻辑

本模块对用户的输入进行了严格的合法性校验，具体规则如下：

#### 🔑 注册功能 (Register)

注册时需要键盘依次录入并验证以下信息，任何一项不符合规则都需要重新输入：

1. **用户名**
   * 必须唯一（查重）。
   * 长度必须在 `3~15` 位之间。
   * 只能由**字母+数字**组合，**不能是纯数字**。
2. **密码**
   * 需连续输入两次，两次一致方可设置成功。
3. **身份证号码**
   * 长度必须严格等于 `18` 位。
   * 不能以 `0` 开头。
   * 前 `17` 位必须全部为数字，最后一位可以是数字，也可以是大/小写字母 `X/x`。
4. **手机号码**
   * 长度必须严格等于 `11` 位。
   * 不能以 `0` 开头。
   * 必须全部为数字。

#### 🛡️ 登录功能 (Login)

1. 判断用户名是否已注册（未注册直接拦截）。
2. 输入验证码并校验（支持忽略大小写，错误需重新输入）。
3. 校验用户名与密码匹配度，**最多提供 3 次试错机会**，超过 3 次锁定账号。

> **💡 随机验证码生成规则：**
> * 总长度为 `5` 位。
> * 由 **4位大/小写英文字母** 和 **1位数字** 组成（字母可重复）。
> * **关键算法**：数字可能出现在这 5 位中的任意位置（例如：`aQa1K` 或 `1aBcD`）。

#### 🔓 忘记密码 (Forget Password)

通过校验用户的私密信息来重置密码：
1. 录入用户名（不存在则提示未注册）。
2. 录入身份证号与手机号。
3. 比对身份信息，**一致**则允许输入新密码（需二次确认），**不一致**则提示信息不匹配。

<details class="code-collapse">
<summary>点击查看 App.java (入口与认证逻辑) 源码</summary>

```java
package me.sssnian.studentSystem;

import javax.crypto.spec.PSource;
import java.util.ArrayList;
import java.util.Random;
import java.util.Scanner;

public class App {
    static void main() {
        Scanner sc = new Scanner(System.in);
        ArrayList<User> list = new ArrayList<>();
        while (true) {
            System.out.println("欢迎来到学生管理系统");
            System.out.println("请选择操作：1登录 2注册 3忘记密码 4退出");
            String choose = sc.next();
            switch (choose) {
                case "1" -> login(list);
                case "2" -> register(list);
                case "3" -> forgetPassword(list);
                case "4" -> {
                    System.out.println("谢谢使用,再见");
                    System.exit(0);
                }
                default -> System.out.println("没有这个选项");
            }
        }
    }

    // 忘记密码
    private static void forgetPassword(ArrayList<User> list) {
        Scanner sc = new Scanner(System.in);
        System.out.println("请输入用户名");
        String username = sc.next();
        boolean flag = contains(list, username);
        if (!flag) {
            System.out.println("当前用户" + username + "未注册,请先注册");
            return;
        }
        System.out.println("请输入身份证号码");
        String personId = sc.next();
        System.out.println("请输入手机号码");
        String phoneNumber = sc.next();

        int index = findIndex(list, username);
        User user = list.get(index);
        if (!(user.getPersonId().equalsIgnoreCase(personId) && user.getPhoneNumber().equalsIgnoreCase(phoneNumber))) {
            System.out.println("身份证号码或手机号码输入有误,不能修改密码");
            return;
        }

        String password;
        while (true) {
            System.out.println("请输入新的密码");
            password = sc.next();
            System.out.println("请再次输入新的密码");
            String againPassword = sc.next();
            if(password.equals(againPassword)){
                System.out.println("两次密码输入一致");
                break;
            }
            else{
                System.out.println("两次密码输入不一致,请重新输入");
            }
        }

        user.setPassword(password);
        System.out.println("密码修改成功");
    }

    private static int findIndex(ArrayList<User> list, String username) {
        for (int i = 0; i < list.size(); i++) {
            User user = list.get(i);
            if (user.getUsername().equals(username)) {
                return i;
            }
        }
        return -1;
    }

    //注册
    private static void register(ArrayList<User> list) {
        Scanner sc = new Scanner(System.in);
        String username;
        String password;
        String personID;
        String phoneNumber;
        //用户名
        while (true) {
            System.out.println("请输入用户名");
            username = sc.next();

            boolean flag1 = checkUsername(username);
            if (!flag1) {
                System.out.println("用户名格式不满足条件,需要重新输入");
                continue;
            }
            boolean flag2 = contains(list, username);
            if (flag2) {
                System.out.println("用户名" + username + "已存在,请重新输入");
            } else {
                System.out.println("用户名" + username + "可用");
                break;
            }
        }
        //密码
        while (true) {
            System.out.println("请输入要注册的密码");
            password = sc.next();
            System.out.println("请再次输入要注册的密码");
            String againPassword = sc.next();
            if (!(password.equals(againPassword))) {
                System.out.println("两次输入的密码不一样,请重新输入");
            } else {
                System.out.println("密码一致");
                break;
            }
        }
        //身份证
        while (true) {
            System.out.println("请输入身份证号码");
            personID = sc.next();
            boolean flag3 = checkPersonID(personID);
            if (flag3) {
                System.out.println("身份证号码满足要求");
                break;
            } else {
                System.out.println("身份证号码格式有误,请重新输入");
            }
        }
        //手机号码
        while (true) {
            System.out.println("请输入手机号码");
            phoneNumber = sc.next();
            boolean flag4 = checkPhoneNumber(phoneNumber);
            if (flag4) {
                System.out.println("手机号码格式正确");
                break;
            } else {
                System.out.println("手机号码格式有误,请重新输入");
            }
        }
        User u = new User(username, password, personID, phoneNumber);
        list.add(u);
        System.out.println("注册成功");
        ;

        printList(list);
    }

    private static void printList(ArrayList<User> list) {
        for (int i = 0; i < list.size(); i++) {
            User u = list.get(i);
            System.out.println(u.getUsername() + " " + u.getPassword() + " " + u.getPersonId() + " " +
                    u.getPhoneNumber());
        }
    }

    private static boolean checkPhoneNumber(String phoneNumber) {
        if (phoneNumber.length() != 11) return false;
        if (phoneNumber.startsWith("0")) {
            return false;
        }
        for (int i = 0; i < phoneNumber.length(); i++) {
            char c = phoneNumber.charAt(i);
            if (!(c >= '0' && c <= '9')) return false;
        }
        return true;
    }

    private static boolean checkPersonID(String personID) {
        if (personID.length() != 18) {
            return false;
        }
        boolean flag = personID.startsWith("0"); // 以0开头是true
        if (flag) return false;
        for (int i = 0; i < personID.length() - 1; i++) {
            char c = personID.charAt(i);
            if (!(c >= '0' && c <= '9')) return false;
        }
        char endChar = personID.charAt(personID.length() - 1);
        if (!((endChar >= '0' && endChar <= '9') || endChar == 'x' || endChar == 'X')) return false;
        return true;
    }

    private static boolean contains(ArrayList<User> list, String username) {
        for (int i = 0; i < list.size(); i++) {
            User user = list.get(i);
            String rightUsername = user.getUsername();
            if (rightUsername.equals(username)) return true;
        }
        return false;
    }

    private static boolean checkUsername(String username) {
        int len = username.length();
        if (len < 3 || len > 15) return false;
        int count = 0;
        for (int i = 0; i < username.length(); i++) {
            char c = username.charAt(i);
            if (!((c >= 'a' && c <= 'z') || (c >= 'A' && c <= 'Z') || (c >= '0' && c <= '9'))) return false;
        }
        for (int i = 0; i < username.length(); i++) {
            char c = username.charAt(i);
            if (((c >= 'a' && c <= 'z') || (c >= 'A' && c <= 'Z'))) {
                count++;
                break;
            }
        }
        return count > 0;
    }

    //登录
    private static void login(ArrayList<User> list) {
        Scanner sc = new Scanner(System.in);
        for (int i = 0; i < 3; i++) {
            System.out.println("请输入用户名");
            String username = sc.next();
            boolean flag = contains(list, username);
            if (!flag) {
                System.out.println("用户名" + username + "未注册,请先注册再登录");
                return;
            }

            System.out.println("请输入密码");
            String password = sc.next();

            //验证码
            while (true) {
                String rightCode = getCode();
                System.out.println("当前正确的验证码为" + rightCode);
                System.out.println("请输入验证码");
                String code = sc.next();
                if (code.equalsIgnoreCase(rightCode)) {
                    System.out.println("验证码正确");
                    break;
                } else {
                    System.out.println("验证码错误");
                }
            }

            //用户名和密码是否正确
            User useInfo = new User(username, password, null, null);
            boolean result = checkUserInfo(useInfo, list);
            if (result) {
                System.out.println("登录成功,可以开始使用学生管理系统");
                StudentSystem ss = new StudentSystem();
                ss.startStudentSystem();
                break;
            } else {
                System.out.println("登录失败,用户名或密码错误");
                if (i == 2) {
                    System.out.println("当前帐号" + username + "被锁定,请联系管理员");
                    return;
                } else {
                    System.out.println("用户名或密码错误,还剩下" + (2 - i) + "次机会");
                }
            }
        }
    }

    private static boolean checkUserInfo(User useInfo, ArrayList<User> list) {
        for (int i = 0; i < list.size(); i++) {
            User user = list.get(i);
            if (user.getUsername().equals(useInfo.getUsername()) && user.getPassword().equals(useInfo.getPassword())) {
                return true;
            }
        }
        return false;
    }

    private static String getCode() {
        ArrayList<Character> list = new ArrayList<>();
        for (int i = 0; i < 26; i++) {
            list.add((char) ('a' + i));
            list.add((char) ('A' + i));
        }
        Random r = new Random();
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < 4; i++) {
            int index = r.nextInt(list.size());
            char c = list.get(index);
            sb.append(c);
        }
        int number = r.nextInt(10);
        sb.append(number);

        char[] arr = sb.toString().toCharArray();
        int randomIndex = r.nextInt(arr.length);
        char temp = arr[randomIndex];
        arr[randomIndex] = arr[arr.length - 1];
        arr[arr.length - 1] = temp;

        return new String(arr);
    }
}
```
</details>

---
*记录于 2026-08-19，继续加油！☕️*
by ssshinian