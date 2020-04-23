

[TOC]

# 集合（set）

## 一、什么是集合

集合可以理解成一个集合体，学习python的学生可以看成一个集合体，学习linux的学生也可以看成一个集合体。

1. 用途：==去重，乱序==。用于关系运算的集合体，由于集合体内的元素无序且集合元素不可重复，因此集合可以去重，但是集合是无序的，去重之后会打乱原来元素的顺序。

2. 定义：{}内用逗号分隔开多个元素，每个元素必须是不可变的类型。

3. 定义空集合

   ```python
   s = {}  # 空大括号是字典，不是集合，定义空集合必须得用set()
   ```

   

## 二，内置方法

   ### 2.1优先掌握

   1. 长度len

      ```python
      s = {1,2,'a'}
      print(len(s))
      #输出：
      3
      ```

   2. 成员运算in or not in

      ```python
      s = {1,2,'a'}
      print(1 in s)
      print(2 not in s)
      #输出：
      True
      False
      ```

      ![](https://img2018.cnblogs.com/blog/1739658/201907/1739658-20190728122359384-1766128333.jpg)

   3. |并集 union

      ```python
      s1 = {1,2,'a'}
      s2 = {1,2,3,4,'b'}
      print(s1|s2)
      print(s1.union(s2))
      #输出：
      {1, 2, 3, 4, 'b', 'a'}
      {1, 2, 3, 4, 'b', 'a'}
      
      ```

   4. &交集  intersection

      ```python
      s1 = {1,2,'a'}
      s2 = {1,2,3,4,'b'}
      print(s1&s2)
      print(s1.intersection(s2))
      #输出：
      {1, 2}
      {1, 2}
      ```

   5. -差集 difference

      ```python
      s1 = {1, 2, 'a'}
      s2 = {1, 2, 3, 4, 'b'}
      print(s1 - s2)
      print(s1.difference(s2))
      print(s2 - s1)
      print(s2.difference(s1))
      #输出：
      {'a'}
      {'a'}
      {3, 4, 'b'}
      {3, 4, 'b'}
      ```

   6. ^对称差集 symmetric_difference

      ```python
      ##取出除共有的外
      s1 = {1, 2, 'a'}
      s2 = {1, 2, 3, 4, 'b'}
      print(s1^s2)
      print(s1.symmetric_difference(s2))
      #输出：
      {'b', 3, 4, 'a'}
      {'b', 3, 4, 'a'}
      
      ```

   7. ==

      ```python
      #比较
      s1 = {1, 2, 'a'}
      s2 = {1, 2, 3, 4, 'b'}
      print(s1 == s2)
      #输出：
      False
      ```

   8. 父集： >  >=   issuperset

      ```python
      s1 = {1, 2, 'a'}
      s2 = {1, 2, 'a'}
      s3 = {1, 2}
      print(s1 >= s2)
      print(s1.issuperset(s2))
      print(s1 > s3)
      print(s1.issuperset(s3))
      print(s1 >= s3)
      print(s1.issuperset(s3))
      print(s3.issuperset(s1))
      #输出：
      True
      True
      True
      True
      True
      True
      False
      ```

   9. 子集：<  <=   issubset

      ```python
      s1 = {1, 2, 'a'}
      s2 = {1, 2, 'a'}
      s3 = {1, 2}
      print(s1 <= s2)
      print(s1.issubset(s2))
      print(s3 < s1)
      print(s3.issubset(s1))
      print(s3 <= s1)
      print(s3.issubset(s1))
      #输出：
      True
      True
      True
      True
      True
      True
      ```



### 2.2需要掌握

1. add()添加

   ```python
   s1 = {1, 2, 'a'}
   s1.add(5)
   print(s1)
   #输出:
   {1, 2, 5, 'a'}
   
   ```

2. remove（）移除，没有报错

   ```python
   s1 = {1, 2, 'a'}
   s1.remove(2)
   print(s1)
   #输出：
   {1, 'a'}
   
   ```

3. difference_update()

   ```python
   # 取出和另外一个集合不同的元素
   s1 = {1, 2, 'a'}
   s2={1,2,4,5,'b','c'}
   s2.difference_update(s1)
   print(s2)
   #输出：
   {4, 5, 'c', 'b'}
   ```

4. discard()删除(抛弃)，没有不报错

   ```python
   s1 = {1, 2, 'a'}
   # s1.remove(3)# 报错
   s1.discard(3)
   print(s1)
   #输出：
   {1, 2, 'a'}
   
   ```

5. isdisjoint

   ```python
   # 集合没有共同的部分返回True，否则返回False
   s1 = {1, 2, 'a'}
   s2 = {1,2,3,4,5,'b'}
   s3 = {6,7,8}
   
   
   print(s2.isdisjoint(s1))
   print(s2.isdisjoint(s3))
   #输出：
   False
   True
   ```

6. clear

7. copy

8. remove

9. pop(随机删除)

10. discard()抛弃

11. 存在一个或者多个值：多个值，不可改变

12. 有序或者无序：无序

13. 可变或者不可变：可变数据类型

