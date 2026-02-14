# &#x20;MySql

## &#x20;基础学习笔记

### 1.简单查询

语法：&#x20;

```sql
SELECT * FROM 数据源;

SELECT 列名 FROM 数据源;

```

解释：

\`SELECT \* FROM 数据源;\`

其中的 \* 表示所有信息

\`SELECT 列名 FROM 数据源; \`

其中列名可为多个列名，列如：

\`SELECT 列名1 ,列名2 ,列名3 , ... FROM 数据源; \`

### 2. 删除列中的重复信息

语法：

```sql
SELECT DISTINCT 列名 FROM 数据源 ;
```

解释：

该代码中的**DISTINCT**关键字就是代表去除重复内容的作用

### 3. 简单数学运算

可直接使用列名和数字之间进行加减乘除

例1：

已知一批商品的售价列名称为：sale\_price 批发折扣列为: cutoff，商品的数据源叫product，那么查找出该类商品的所有批发价

```sql
SELECT sale_price*cutoff FROM product;
```

例2：

在例1基础上，已知该类商品的成本价格列名为cost\_price，现在需要列出50个该商品的成本价

```sql
SELECT cost_price*50 FROM product;
```

例3：

在例2基础上，假设每个运费为1元，列出成本价

```sql
SELECT (cost_price+1)*50 FROM product;
```

### 4. 重命名操作

语法:

```sql
SELECT 列名 as 重命名 FROM 数据源 as 重命名2 ;
```

解释：

重命名操作只能对**SELECT后的列名**和**FROM后的数据源名**进行操作

例4：

在例3基础上，将成本价+运费的部分重命名为进价（cost）

```sql
SELECT (cost_price+1)*50 as costFROM product;
```

例5：

在例4基础上，将数据源重命名为p

```sql
SELECT (cost_price+1)*50 as cost FROM product as p;
```

**补充：**

重命名操作的**as**可省略，效果和原来一样

## 5. 设置显示格式

MySQL提供了一种函数用于拼接字符串

```sql
CONCAT (str1 ,str2 ,str3 ...) 
```

例1：需求:查询商品的名字和零售价。格式：xxx商品的零售价为：xxx

```sql
SELECT CONCAT(product_name ,'商品的零售价为：' ,sale_price) FROM product;
```

## 6. 过滤查询

比较运算：

*   \> 大于
*   < 小于
*   \= 等于
*   \>= 大于等于
*   <= 小于等于
*   != 不等于

语法：

```sql
SELECT */列名 FROM 数据源 WHERE 筛选条件
```

例1:

查询货品零售价为119的所有货品信息.

```sql
SELECT * FROM product WHERE sale_price = 119;
```

例2:

查询货品名为罗技G9X的所有货品信息.

```sql
SELECT * FROM product WHERE product_name = '罗技G9X';
```

例3:

查询货品名 不为 罗技G9X的所有货品信息.

```sql
SELECT * FROM product WHERE product_name != '罗技G9X';
```

例4:

查询分类编号不等于2的货品信息

```sql
SELECT * FROM product WHERE category_id != 2;
```

例5:

查询货品名称,零售价小于等于200的货品

```sql
SELECT product_name FROM product WHERE sale_price <= 200;
```

例6：

查询id，货品名称，批发价大于350的货品

```sql
SELECT id,product_name,sale_price*cutoff cost FROM product WHERE sale_price*cutoff > 300;
```

## 7. MySQL执行顺序

**FROM** -> 决定从哪一个表查询

**JOIN** -> 决定跟哪一个表关联

**ON** -> 决定关联表的共同条件

**WHERE** -> 决定从表中哪一些开始查询（条件过滤，学会善用条件过滤）

**GROUP BY** -> 决定按照什么分组进行组内查询，一般跟聚集函数联合使用

‌**SELECT**‌：从分组和筛选后的结果中选择需要的列。

**HAVING** -> 在组内条件过滤，一般使用SELECT语句中的聚集函数作为过滤条件

**ORDER BY** -> 决定显示的排序规则

**LIMIT** -> 决定每一页显示多少条件记录

## 8. 逻辑运算

### 8.1 AND--并且

选择id，货品名称，批发价在300-400之间的货品

```sql
SELECT id ,product_name ,sale_price*cutoff as cost FROM product WHERE sale_price*cutoff >= 300 AND sale_price*cutoff <= 400;
```

### 8.2 OR--或者

选择id，货品名称，分类编号为2,4的所有货品

```sql
SELECT id ,product_name ,category_id FROM product WHERE category_id = 2 OR category_id = 4;
```

选择id，货品名称，分类编号的货品零售价大于等于250或者是成本大于等于200

```sql
SELECT id ,product_name ,category_id ,sale_price ,cost_price FROM product WHERE sale_price >= 250 OR cost_price >= 200;
```

### 8.3 NOT--取反--否定

选择id，货品名称，分类编号不为2的所有商品

```sql
SELECT id ,product_name ,category_id FROM product WHERE category_id != 2;
```

优先级：

所有比较运算符 > NOT > AND > OR

## 9. 范围查询

语法：

```sql
SELECT 列名 FROM 表名 WHERE 列名 BETWEEN minvalue AND maxvalue;
```

例1：

选择id，货品名称，批发价在300-400之间的货品

```sql
SELECT id ,product_name ,sale_price*cutoff as cost FROM product WHERE sale_price*cutoff BETWEEN 300 AND 400;
```

例2:

选择id，货品名称，批发价不在300-400之间的货品

```sql
SELECT id ,product_name ,sale_price*cutoff as cost FROM product WHERE sale_price*cutoff NOT BETWEEN 300 AND 400;
```

## 10. 集合查询

使用IN运算符，判断列的值是否在指定的集合中

语法：

```sql
SELECT 列名 FROM 数据源 WHERE 列名 IN(值1 ，值2...);
```

例1:

选择id，货品名称，分类编号为2,4的所有货品

```sql
SELECT id ,product_name ,category_id FROM product WHERE category_id IN (2 ,4);
```

例2:

选择id，货品名称，分类编号不为2,4的所有货品

```sql
SELECT id ,product_name ,category_id FROM product WHERE category_id NOT IN (2 ,4);
```

## 11. 空值查询

使用 IS NULL：判断列的值是否为空

语法：

```sql
SELECT 列名 FROM 数据源 WHERE 列名 IS NULL;
```

注意：

列的值为null和空字符串不同 ，如果是空字符串则应该 = ""；

例1:

查询商品名为NULL的所有商品信息。

```sql
SELECT * FROM product WHERE product_name IS NULL;
```

例2:

查询商品名不为NULL的所有商品信息

```sql
SELECT * FROM product WHERE product_name IS NOT NULL;
```

## 12. 模糊查询

使用LIKE关键字执行通配查询，查询条件可包含文字字符或数字：

*   %:通配符：可表示零或多个任意的字符。
*   \_:通配符：可表示任意的一个字符。
*   \[]通配符：用来实现匹配部分值得特殊字符。
*   \[^]通配符：不在所列的单个字符。

例1:

查询id，货品名称，货品名称匹配'%罗技M9\_'

```sql
SELECT id ,product_name FROM product WHERE product_name LIKE '%罗技M9_';
```

例2:

查询id，货品名称，分类编号,零售价大于等于20并且货品名称匹配'%罗技M1\_\_'

```sql
SELECT id ,product_name ,category_id ,sale_price FROM product WHERE sale_price >= 20 AND product_name LIKE '%罗技M1__';
```

## 13. 正则表达式

MySQL中使用 REGEXP 操作符来进行正则表达式匹配

例1：

查找 name 字段中以 **'罗技'** 为开头的所有数据

```sql
SELECT * FROM product WHERE product_name REGEXP '^罗技';
```

^：表示匹配输入字符串的开始位置

例2:

查找 name 字段中以 5 为结尾的所有数据

```sql
SELECT * FROM product WHERE product_name REGEXP '5$';
```

\$：表示匹配输入字符串的结束位置

例3:

查找 name 字段中包含 **'罗技'** 字符串的所有数据

```sql
SELECT * FROM product WHERE product_name REGEXP '罗技';
```

\[...]：字符集合，匹配所办好的任意一个字符，例如'\[abc]'可以匹配"plain"中的'a'

.  :匹配除"\n"之外任何单个字符，要匹配包括"\n"在内的任何字符，请使用像"\[.\n]"的模式

\[^...]：负值字符集合，匹配未包含的任何字符

p1|p2|p3：匹配p1或p2或p3

## 14. 聚集函数/聚合函数

聚集函数作用于**一组数据，并对一组数据**返回一个值。

*   COUNT：统计结果记录数 如果列的值为null 不会计算在内的
*   MAX： 统计计算最大值
*   MIN： 统计计算最小值
*   SUM： 统计计算求和
*   AVG： 统计计算平均值 如果列的值为null 不会计算在内的

例1:

查询所有商品平均零售价

```sql
SELECT AVG(sale_price) 
FROM product;
```

直接这样计算只计算了非null的数据，如果要正确计算平均值，即包含null的数据条，则使用IFNULL函数

```sql
IFNULL(expr1 ,expr2);
```

解释：

如果expr1为NULL则用expr2替代

因此正确写法：

```sql
SELECT AVG(IFNULL(sale_price ,0)) 
FROM product;
```

例2:

查询商品总记录数

```sql
SELECT COUNT(*) 
FROM product;
```

例3:

查询分类为2的商品总数

```sql
SELECT COUNT(*) 
FROM product
WHERE category_id = 2;
```

例4:

查询商品的最小零售价，最高零售价，以及所有商品零售价总和

```sql
SELECT MIN(sale_price) ,MAX(sale_price) ,SUM(sale_price) 
FROM product;
```

## 15. 分组查询

可以使用GROUP BY 子句将表中的数据分成若干组，再对分组之后的数据做统计计算，一般使用聚集函数才使用GROUP BY.

语法：

    SELECT 聚集函数或者分组的列

    FROM table_name

    WHERE 条件

    GROUP BY 列名

    HAVING 分组之后的条件；

注意：

*   GROUP BY 后面的列名的值要有重复性分组才有意义;
*   不能在 WHERE 子句中使用函数（注意）;
*   可以在 HAVING 子句中使用函数;

例1：

查询每个商品分类编号和每个商品分类各自的平均零售价

```sql
SELECT category_id ,AVG(sale_price) 
FROM product
GROUP BY category_id;
```

例2：

查询每个商品分类编号和每个商品分类各自的商品总数。

```sql
SELECT category_id ,COUNT(*) 
FROM product
GROUP BY category_id;
```

例3：

查询每个商品分类编号和每个商品分类中零售价大于100的商品总数

```sql
SELECT category_id ,COUNT(sale_price)
FROM product
WHERE sale_price > 100
GROUP BY category_id;
```

例4：

查询零售价总和大于1500的商品分类编号以及总零售价和

```sql
SELECT category_id ,SUM(sale_price)
FROM product
GROUP BY category_id
HAVING SUM(sale_price)>1500;
```

这里如果写成

```sql
SELECT category_id ,SUM(sale_price)
FROM product
WHERE SUM(sale_price)>1500
GROUP BY category_id;
```

会报错，因为WHERE的执行优先级大于GROUP BY因此会先进行筛选，但是GROUP BY要从所有的数据中以category\_id来分组，拿不到全部数据，会无法执行

**WHERE后不能使用聚合函数**

## 16. 结果排序

使用ORDER BY子句将结果的记录排序

*   ASC 升序 缺省，不写则默认为升序
*   DESC 降序
*   ORDER BY 语句出现在SELECT语句的最后
   
语法：
```sql
SELECT <selectList>

FROM table_name

WHERE 条件

ORDER BY 列名1 [ASC/DESC],列名2 [ASC/DESC]...;
```
例1：

选择id，货品名称，分类编号,零售价并且按零售价降序排序

```sql
SELECT id ,product_name ,category_id ,sale_price 
FROM product
ORDER BY sale_price DESC;
```

例2：

选择id，货品名称，分类编号,零售价先按分类编号排序,再按零售价排序

```sql
SELECT id ,product_name ,category_id ,sale_price 
FROM product
ORDER BY category_id ,sale_price;
```

例3：

查询M系列并按照批发价排序(加上别名)

```sql
SELECT id ,product_name ,sale_price*cutoff cost
FROM product
WHERE product_name LIKE '%M%'
ORDER BY cost;
```

例4：

查询分类为2并按照批发价排序(加上别名)

```sql
SELECT id ,product_name ,category_id ,sale_price*cutoff cost
FROM product
WHERE category_id = 2
ORDER BY cost;
```

## 17. 分页查询

- 假分页：把数据全部查询出来，存在于内存中，翻页的时候，直接从内存中去截取
- 真分页：每次翻页都去数据库中去查询数据
- 假分页：翻页比较快，但是第一次查询很慢，若数据过大，可能导致内存溢出
- 真分页：翻页比较慢，若数据过大，不会导致内存溢出

语法：

```sql
SELECT * FROM table_name LIMIT ?,?;

SELECT * FROM table_name LIMIT beginIndex,pageSize;

beginIndex = (currentPage-1) * pageSize;

第一个?: 表示本页,开始索引(从0开始).

第二个?: 每页显示的条数

```

## 18. 多表查询

### 18.1 笛卡尔积

```sql
SELECT *
FROM emp ,dept;
```

笛卡尔积就是两个集合相乘

例如：

集合A*集合B，集合A：[a ,b] ，集合B：[1 ,2 ,3]

A*B结果为：

[a ,1] ,[a ,2] ,[a ,3] ,[b ,1] ,[b ,2] ,[b ,3]

### 18.2 内连接查询

就是屏蔽笛卡尔积，将错误的数据屏蔽掉

#### 18.2.1 隐式内连接

```sql
SELECT 列名
FROM 数据源(表1 ，表2 ，表3...)
WHERE 等值条件;
```

#### 18.2.2 显示内连接

```sql
SELECT 列名
FROM 表1
INNER JOIN 表2 ON 表1和表2的等值条件
INNER JOIN 表3 ON 表1和表3的等值条件/表2和表3的等值关系
...
;
```

### 18.3 外连接查询

#### 18.3.1 左外连接查询

查询出JOIN左边表的全部数据查询出来,JOIN右边的表不匹配的数据使用NULL来填充数据

语法：

```sql
SELECT 列名
FROM 数据源A
LEFT JOIN 数据源B ON 等值条件
```

#### 18.3.2 右外连接查询

查询出JOIN右边表的全部数据查询出来,JOIN左边的表不匹配的数据使用NULL来填充数据

语法：

```sql
SELECT 列名
FROM 数据源A
RIGHT JOIN 数据源B ON 等值条件
```

### 18.4 自连接查询

本质也是多表查询，只是多张表是同一个名字

需求：查询每个商品分类的名称和父分类名称（所属分类的名称）

左外连接实现自连接

```sql
SELECT c.* ,p.category_name as pDirname
FROM product_category c LEFT JOIN product_category p ON c.parent_id = p.id;
```

内连接实现自连接

```sql
SELECT c.* ,p.category_name as pDirname
FROM product_category c JOIN product_category p ON c.parent_id = p.id;
```

注意：

自连接查询时，等值条件一般都是：子表的父ID = 父表的ID

### 18.5 子查询

也叫嵌套查询

一个查询语句嵌套在另一个查询语句当中

例1：

需求: 查询零售价比罗技MX1100更高的所有商品信息

```sql
SELECT *
FROM product
WHERE sale_price > (SELECT sale_price FROM product WHERE product_name = '罗技MX1100');
```

例2：

需求: 查询分类编号和折扣与罗技M100相同的所有商品信息

```sql
SELECT *
FROM product
WHERE (category_id ,cutoff )= (SELECT category_id ,cutoff FROM product WHERE product_name = '罗技M100');
```

子查询可以放WHERE后，也可以放在FROM后面，放在FROM后面时作为临时表使用，一般很少放在FROM后，会降低可读性

## 19. DML操作

主要是对表中的数据进行增删改操作

### 19.1 新增数据

插入语句:一次插入操作只插入一行

**INSERT INTO** table_name (column1,column2,column3\...)

**VALUES** (value1,value2,value3\...);

**INSERT INTO** table_name **VALUES** (value1,value2,value3\...);

例1：

需求：新增一条员工数据 empno = 1000 ename = '张三' job = CLERK deptno = 20

```sql
INSERT INTO emp(empno ,ename ,job ,deptno) VALUES(1000 ,'张三' ,'CLERK' ,20);
```

如果不指定列名(column)，则默认是要为全部列新增数据，新增数据中的值个数要等于列数

例2：

需求：新增一名员工数据 empno = 1002 ename = '李四' job = CLERK deptno = 20

```sql
INSERT INTO emp VALUES(1002 ,'李四' ,'CLERK' ,20);
```

批量新增： INSERT INTO 表名(字段1 ，字段2 ，字段3...) VALUES/VALUE (值1 ，值2 ，值3) ,(值1 ，值2 ，值3) ,(值1 ，值2 ，值3)...

```sql
INSERT INTO emp(empno ,ename ,deptno) VALUES (1000 ,'张三' ,20) ,(1002 ,'李四' ,20) ,(1003 ,'王五' ,20);
```

### 19.2 修改数据

UPDATE 表名 SET 字段1 = '修改后的值' ,字段2 = '修改后的值'... WHERE 过滤条件

**若不加过滤条件则是修改整张表的数据**

例1：

需求:将零售价大于300的货品零售价上调0.2倍

```sql
UPDATE product SET sale_price = sale_price*1.2 
WHERE sale_price > 300;
```

例2：

需求:将零售价大于300的有线鼠标的货品零售价上调0.1倍

```sql
UPDATE product SET sale_price = sale_price*1.1
WHERE sale_price > 300
AND category_id = (SELECT id FROM product_category WHERE category_name = '无线鼠标');
```

### 19.3 删除数据

DELETE FROM 表名 WHERE 过滤条件

**如果不加 WHERE 过滤条件则是删除整张表**

例1：

删除员工表中员工号为1000的数据

```sql
DELETE FROM emp WHERE empno = 1000;
```

## 20. 数据备份与恢复

### 20.1 数据备份(数据导出)

数据备份就是将数据库中的数据导出为一个sql文件

```bash
mysqldump -u账户 -p密码 数据库名称\>脚本文件存储地
```

### 20.2 数据恢复(数据导入)

数据恢复就是将一个sql文件执行后，将sql文件中的表结构、数据都导入到一个新的数据库中

```bash
mysql -u账户 -p密码 数据库名称\< 脚本文件存储地址
```

## 21. 函数

### 21.1 内置函数

内置函数就是sql内部已经提供好的，我们拿来用就可以了的函数

#### 21.1.1 聚集（聚合）函数:

1.  count

2.  sum

3.  avg

4.  max

5.  min
  
#### 21.1.2 CAST函数

转换数据类型：

CAST(值 AS 类型)

例如：

将sale_price int类型 转换为 DECIMAL 浮点型

```sql
SELECT sale_price ,CAST(sale_price AS DECIMAL(10,5)) 
FROM product;
```

这里的DECIMAL(10,5)意思是小数点前+小数点后的位数 = 10，小数点后的位数必须为5位

#### 21.1.3 关于DECIMAL类型

column decimal(P,D)

在上面的语法中：

P是表示有效数字数的精度(总长度)。 P范围为1〜65。

D是表示小数点后的位数。 D的范围是0~30。MySQL要求**D小于或等于(<=)P**。

DECIMAL(P，D)表示列**可以存储D位小数的P位数**。十进制列的实际范围取决于精度和刻度。

综上，可以将上述小数转换为整数：

select cast('123.4' as decimal(P,D))

#### 21.1.4 CONVERT 函数

和CAST 函数作用相同，都是转换数据类型

例如：

将sale_price int类型 转换为 DECIMAL 浮点型

```sql
SELECT sale_price ,CONVERT(sale_price ,DECIMAL(10,3))
FROM product;
```

#### 21.1.5 IFNULL 函数

IFNULL(参数1 ,参数2) 如果列中参数1为NULL，则用参数2填充

例如：

查询所有商品名称，如果商品名称为NULL，则使用'默认商品'填充

```sql
SELECT IFNULL(product_name ,'默认商品')
FROM product;
```

### 21.2 自定义函数

语法：

```sql
create function 函数名(参数名 参数类型) returns 返回类型
begin
  写函数体的内容
end;
```

 如何调用自定义函数?

 ```sql
 select 函数名();
 ```
 
如果自定义函数创建失败，需要开启函数创建的日志

```sql
show variables like 'log_bin_trust_function_creators';

set global log_bin_trust_function_creators = 1;
```

两个数相加

```sql
CREATE FUNCTION test_add(num1 INT ,num2 INT) RETURNS INT
BEGIN
  RETURN num1 + num2;
END;
```

## 22. 存储过程

可以把代码中复杂业务逻辑(要执行100sql),封装为一个存储过程.编译好了以后放在数据库中，如果要用的话，就直接调用该存储过程就可以了，它就相当于执行了100行sql

**函数和存储过程的区别:**

- 函数一般是将多个都要用的地方封装起来
- 存储过程是将一个复杂操作封装起来

```sql
-- 1 定义存储过程  
create procedure yaosangsum(in a int ,in b int,out sum int)--in表输入 out表输出
begin
    set a = a + 1;
	set sum = a+b;
end;
drop procedure yaosangsum;
-- 2 调用存储过程
call yaosangsum(1,2,@xxx);
select @xxx;
```

例如：

```sql
CREATE PROCEDURE ProductByID(in ID1 INT)
BEGIN
  SELECT * 
  FROM product
  WHERE id = ID1;
END;

-- 调用
CALL ProductByID(2);
```

## 23. 索引

索引使用注意事项:

1. 如果查询少，增删数据多，不适合使用索引，增删数据时会导致底层产生的索引也会跟着改变
2. 如果某个字段不会作为查询条件，那么该字段也不适合加索引
3. 如果一个字段的值分布很集中，不适合加索引，例如性别：只有男和女两个节点
4. 如果一个表中有主键，则会自动为主键添加索引
