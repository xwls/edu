# String,StringBuffer,StringBuilder

[TOC]

## 概述

### String

- Java语言中的字符串值属于String类，虽然有其它方法表示字符串（如字符数组），但Java一般使用String类作为字符串的标准格式，Java编译器把字符串值作为String对象。
- String对象一旦创建就不能改变。如果需要进行大量的字符串修改操作，应该使用StringBuffer/StringBuilder类或者字符数组，最终结果可以被转换成String对象。

### StringBuffer

- StringBuffer 线程安全的可变字符序列。
- 一个类似于String的字符串缓冲区，通过某些方法调用可以改变该序列的长度和内容。


- 每个字符串缓冲区都有一定的容量。只要字符串缓冲区所包含的字符序列的长度没有超出此容量，就无需分配新的内部缓冲区数组。


- 如果内部缓冲区溢出，则此容量自动增大。

### StringBulider

- 从JDK5开始，为StringBuffer类补充了一个单个线程使用的等价类，即StringBuilder。
- 与StringBuffer相比，通常应该优先使用StringBuilder 类，因为它支持所有相同的操作，但由于它不执行同步，所以速度更快。

## 效率

StringBuilder>StringBuffer>String

## 扩容

当前容量*2+2

## StringBuffer/StringBuilder常用方法

1. `public int length()`返回字符串长度
2. `public int capacity()`返回当前数组容量
3. `public StringBuffer append(String str)`追加字符串
4. `public int indexOf(String str)`查找指定字符串首次出现的位置
5. `public int indexOf(String str,int fromIndex)`从指定位置开始查找
6. `public int lastIndexOf(String str)`查找指定字符串最后一次出现的位置

## String常用方法

`public String trim()`去除两端空格