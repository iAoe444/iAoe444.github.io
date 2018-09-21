---
title: 第一次Android作业笔记
categories:
- 笔记
tags:
- 安卓
---

>这个是我在做Android时需要用到的知识点，我怕以后忘了，所以收集一下，兴许以后会用到

## BigDecimal篇

### 简单入门BigDecimal

```java
BigDecimal a = new BigDecimal("0.1000");
BigDecimal b = new BigDecimal("0.1000");
a = a.add(b);
```

[具体看这里](https://www.cnblogs.com/wangmei/p/5507807.html)



### 作除法运算时候保留小数

divide的方法其实有三个参数，第一个是BigDecimal，就是要除的数字，第二个是保留的小数位数(int)，第三个是采取怎样的形式保留

```java
BigDecimal a;
BigDecimal b;
a = new BigDecimal(3);
b = new BigDecimal(81);
System.out.print(a.divide(b, 2, RoundingMode.HALF_UP));
```

这里是保留两位小数，使用四舍五入的方式保留



### 去掉BigDecimal后无用的零

```java
BigDecimal a = new BigDecimal("0.1000");
System.out.println(a.stripTrailingZeros().toPlainString());
```



## EditText篇

### EditText只能使用数字和小数点

对于xml布局

```xml
<EditText
 android:inputType="number|numberDecimal"></EditText>
```

[具体在这里](https://blog.csdn.net/u012246458/article/details/73288487)



### 获取和设置EditText

无论是获取还是设置，都要先定义这个EditText类型的变量后来获得这个文档

```java
//java
private EditText et_fristnum;
```

```xml
//xml要设置id
<EditText 
        android:id="@+id/et_fristnum"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="请输入第一个数字"/>
```

之后最好是在oncreate方法中获取这个EditText

```java
 et_fristnum = (EditText) this.findViewById(R.id.et_fristnum);
```

**获取EditText的数值**

```java
String fristnum = et_fristnum.getText().toString();
```

**设置EditText的数值**

```java
 et_sumresult.setText(sumresult.toString());
```



## Eclipse篇幅

### 修改包名

1.在Eclipse中修改Android应用程序包名时，需要修改的几个地方(按照修改顺序)：

1）右键创建应用程序时src中自动添加的主包名，即与配置文件中包名相同的那个包，refactor->rename,注意勾选rename subpackages ,或者快捷键alt+shift+r

2）在配置文件AdroidManifest中修改直接修改package标签，或者右键项目名->Android Tools->rename application package

3）这一步很重要，修改gen文件夹下包含R文件的包名，按照第一步的方法，修改为新的包名，如果这一步没有做的话，项目中之前有import R文件的地方就不会自动修改

4）最后Project ->Clean项目，勾上Build Automatically，重新生成，即可

[具体看这里](https://blog.csdn.net/myinterface/article/details/79113181)





