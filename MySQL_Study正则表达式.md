## 第四章：SQL处理数据的高级方法 

## MySQL的正则表达式搜索(Regular Expression-RE)  
1. 之前是通过匹配、比较、通配符来寻找数据，但是随着过滤条件的复杂性，我们需要使用正则表达式，来**匹配文本**的特殊的串（字符集合）
_将一个模式（正则表达式）与一个文本串进行比较_   
2. 场景：1）文本中提取电话号码；2）名字中有数字的文件；3) 文本中所有重复的单词；4）替代URL为html
3. **MySQL用where子句对正则表达式提供了初步支持，允许你指定正则表达式，过滤select检索出的数据。**  

**正则表达式的语句：Table中数列的名字 REGEXP + 单引号 + 正则表达式的符号：数字、|、[  ]、 [^ ] 、 + 分号 

**正则中.则代表任何信息；如果前面使用转移符号\\ . 则是专门匹配 

### 1.基本字符串匹配 
1）找到含产品名称中含1000的字样
a. 传统方法：
```
select * from products where
prod_name like '%1000'; 

```
b.使用正则表达式：
```
select * from products where
prod_name REGEXP '1000' 

```
 
 ### 2. 匹配000相关的产品信息 
```
select * from products where
prod_name like '%000';

select * from products WHERE
prod_name REGEXP '.000'; 

```

### 3.使用or的方法进行匹配   #| 
找到含产品名称中含1000或2000的字样
```
select * from products WHERE
prod_name REGEXP '1000|2000'; 

```  

### 4.使用范围方法进行匹配  #中括号
```  
select * from products WHERE
prod_name REGEXP '[12]000';
```  
_中括号代表范围[1-4],表示括号里面是[1、2、3、4]：中括号里面代表一个字符、出现的可能性


### 5.使用范围和使用or同步进行 
```  
select * from products WHERE
prod_name REGEXP '[1|2]000';  
```  

### 6.排他符号 [^ ] 
找到含产品名称中含1的000的字样
```  
select * from products WHERE
prod_name REGEXP '[^1]000';
```  

### 7.特殊字符的匹配
```  
select * from products where
prod_name REGEXP '[1-9]000';   #将各种可能性置于此 
```  
```  
select * from products where
prod_name REGEXP '[1-9]ton';  #找出数据带ton的

```

### 8. .的使用   \\. 

``` 
select * from products where
prod_name REGEXP '.[1-9]'; 

``` 
![image](https://github.com/sophieloveforlearning/sophielearner.github.io/blob/main/%E6%88%AA%E5%B1%8F2020-12-13%20%E4%B8%8B%E5%8D%885.57.15.png) 

``` 
select * from products where
prod_name REGEXP '\\.[1-9]'; 
``` 
![image](https://github.com/sophieloveforlearning/sophielearner.github.io/blob/main/%E6%88%AA%E5%B1%8F2020-12-13%20%E4%B8%8B%E5%8D%887.23.29.png)



### 9.匹配多个字符
查找产品名字中含有stick的产品
``` 
select * from products where
prod_name REGEXP 'stick'
``` 
![image](https://github.com/sophieloveforlearning/sophielearner.github.io/blob/main/%E6%88%AA%E5%B1%8F2020-12-13%20%E4%B8%8B%E5%8D%887.34.57.png) 

``` 
select * from products where
prod_name REGEXP 'stick?\\)'    #？可以是任何一个字符，\\实现转移，更精确的匹配
``` 
![image](https://github.com/sophieloveforlearning/sophielearner.github.io/blob/main/%E6%88%AA%E5%B1%8F2020-12-13%20%E4%B8%8B%E5%8D%887.33.18.png)  


### 10.常见的正则中的字符类 
[0-9] #代表所有的数字 
[a-zA-Z]#代表所有的字符串 

1) 所有产品名字中有数字的就都出来了：
``` 
select * from products where
prod_name REGEXP '[0-9]'; 
```   

2）所有产品名字中，有2个连续数字的：
``` 
select * from products where
prod_name REGEXP '[0-9][0-9]';    #一个方框是一个数字，有2个连续的数字才匹配 
```
3）所有产品名字中，有4个连续数字的：
方法1： 
``` 
select * from products where
prod_name REGEXP '[0-9][0-9][0-9][0-9]'; 
``` 

方法2： 
``` 
select * from products where
prod_name REGEXP '[0-9]{4}'; 
``` 

