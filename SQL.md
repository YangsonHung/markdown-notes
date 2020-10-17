# SQL语法学习笔记

## 介绍

全名：Structured Query Language



### 数据模型

- 层次模型

- 网状模型

- 关系模型

### 主流关系数据库

- 商用数据库，例如：Oracle，SQL Server，DB2等；
- 开源数据库，例如：MySQL，PostgreSQL等；
- 桌面数据库，以微软Access为代表，适合桌面应用程序使用；
- 嵌入式数据库，以Sqlite为代表，适合手机应用和桌面程序。

## 语法

### 查询数据

#### 基本查询

##### 语法

```sql
SELECT * FROM <表名>;
```

从某个表中查询所有列，查询结果是一个二维表

##### 例子

```sql
SELECT * FROM students;
```

查询出`students`表的所有数据



`SELECT`语句可以不用带`FROM`子句：

用作计算：`SELECT 1+2;`

不带`FROM`子句的`SELECT`语句的一个用途，用来判断当前到数据库的连接是否有效：

执行`SELECT 1;`



#### 条件查询

##### 语法

```sql
SELECT * FROM <表名> WHERE <条件表达式>;
```

通过使用`WHERE`子句来设定查询条件，查询结果是满足查询条件的记录。

##### 例子

```sql
SELECT * FROM students WHERE score >= 80;
```

查询`students`表中`score`值大于等于80的记录

###### 多种条件

1. `<条件1> AND <条件2>`，满足条件1且满足条件2

   例如：

```sql
SELECT * FROM students WHERE score >= 80 AND gender = 'M';
```

   查询`students`表中`score`大于等于80且gender等于`'M'`的记录

2. `<条件1> OR <条件2>`，满足条件1或者满足条件2

   例如：

```sql
SELECT * FROM students WHERE score >= 80 OR gender = 'M';
```


查询`students`表中`score`大于等于80或者`gender`等于`'M'`的记录

3. `NOT <条件>`，排除该条件

   例如：

```sql
SELECT * FROM students WHERE NOT class_id = 2;
```


   查询`students`表，排除`class_id = 2`的记录

   也就是查询`class_id`不等于2的记录

   `NOT class_id = 2`等价于`class_id <> 2`

###### 复杂条件

涉及三个或者更多条件的需要使用小括号：

```sql
SELECT * FROM students WHERE (score < 80 OR score > 90) AND gender = 'M';
```

括号可以改变优先级，默认优先级：`NOT` > `AND` > `OR`

###### 常用的条件表达式

| 使用=判断相等        | score = 80      | name = 'abc'     | 字符串需要用单引号括起来                          |
| -------------------- | --------------- | ---------------- | :------------------------------------------------ |
| 使用>判断大于        | score > 80      | name > 'abc'     | 字符串比较根据ASCII码，中文字符比较根据数据库设置 |
| 使用>=判断大于或相等 | score >= 80     | name >= 'abc'    |                                                   |
| 使用<判断小于        | score < 80      | name <= 'abc'    |                                                   |
| 使用<=判断小于或相等 | score <= 80     | name <= 'abc'    |                                                   |
| 使用<>判断不相等     | score <> 80     | name <> 'abc'    |                                                   |
| 使用LIKE判断相似     | name LIKE 'ab%' | name LIKE '%bc%' | %表示任意字符，例如'ab%'将匹配'ab'，'abc'，'abcd' |



#### 投影查询

##### 语法

让查询结果只包含指定的列

```sql
SELECT 列1, 列2, 列3 FROM <表名>;
```

##### 例子

查询结果中只包含`students`表的`id`，`score`，`name`列

```sql
SELECT id, score, name FROM students;
```

可以给列名设置别名：

```sql
SELECT 列1 别名1, 列2 别名2, 列3 别名3 FROM<表名>;
```

给`score`设置别名`points`，查询结果中会使用此别名来显示，其他保持不变。

```sql
SELECT id, score points, name FROM students;
```



#### 排序

##### 语法

```sql
SELECT ... FROM <表名> ORDER BY <字段名>;
```

默认查询结果集是按照`id`排序的，使用`ORDER BY`子句可以指定按照哪个字段排序

##### 例子

按照`score`字段排序，默认是从低到高排序，就是正序排序（升序），默认排序规则是`ASC`，可以省略

```sql
SELECT id, name, gender, score FROM students ORDER BY score;
```

如果要从高到低排序，倒序排序（降序），在末尾加上`DESC`关键字

```sql
SELECT id, name, gender, score FROM students ORDER BY score DESC;
```

如果指定的排序列中有相同数据的，可以进一步排序，继续添加类名：

先按照`score`列排序，如果`score`列有相同数据，再按照`gender`列排序

```sql
SELECT id, name, gender, score FROM students ORDER BY score DESC, gender;
```

有`WHERE`子句时，`ORDER BY`子句要放到 `WHERE`后面

```sql
SELECT id, name, gender, score FROM students WHERE class_id = 1 ORDER BY score DESC;
```



#### 分页查询

##### 语法

```sql
`SELECT ... FROM <表名> LIMIT <M> OFFSET <N>
```

对结果集分页，每页显示M条记录，从N号记录开始。`LIMIT M`表示最多显示M条记录，SQL记录集的索引从0开始。

##### 例子

```sql
SELECT id, name, gender, score FROM students ORDER BY score DESC LIMIT 3 OFFSET 0;
```

对结果集分页显示记录，从结果集的第0条记录开始，每页显示3条记录，获取的是第一页的记录。

获取第二页使用`LIMIT 3 OFFSET 3`

分页查询需要确定2个点

- 每页显示的记录数`pageSize`，`LIMIT`后面的数字，这个数字固定不变

- 偏移值，`OFFSET`后面的数字，表示从结果集第几条记录开始显示，根据`pageIndex`(页面的索引，从1开始)和`pageSize`确定

  `OFFSET = pageSize * (pageIndex - 1)`

当`N`的值超过结果集的索引时，查询会得到一个空的结果集。

注意点：

- `OFFSET` 是可选值，默认`OFFSET 0`
- 在MySQL中，`LIMIT M OFFSET N`可以简写为`LIMIT M, N`
- `N`值越大，查询效率越低



#### 聚合查询

##### 语法

使用`SQL`提供的聚合函数进行查询，称为聚合查询。

聚合函数

| 函数  | 说明                                                 |
| :---: | :--------------------------------------------------- |
| COUNT | 计算某一列行数，COUNT(*)表示计算所有列的行数         |
|  SUM  | 计算某一列的合计值，该列必须为数值类型               |
|  AVG  | 计算某一列的平均值，该列必须为数值类型               |
|  MAX  | 计算某一列的最大值，若是字符类型，返回排序最后的字符 |
|  MIN  | 计算某一列的最小值，若是字符类型，返回排序最前的字符 |

##### 例子

```sql
SELECT COUNT(*) FROM students;
```

查询`students`表一共有多少条记录

查询结果是二维表，表头显示`COUNT(*)`，可以设置别：

```sql
SELECT COUNT(*) num FROM students;
```

聚合查询可以使用`WHERE`条件：

```sql
SELECT COUNT(*) boys FROM students WHERE gender = 'M';
```

计算`gender = 'M'`时，`score`列的平均数：

```sql
SELECT AVG(score) average FROM students WHERE gender = 'M';
```



分组聚合查询

按照`class_id`将查询结果集分组：

```sql
SELECT COUNT(*) num FROM students GROUP BY class_id;
```

多个列分组，按照`class_id和gender`分组

```sql
SELECT class_id, gender, COUNT(*) num FROM students GROUP BY class_id, gender;
```



注意点：

- 如果聚合查询的WHERE条件没有匹配到任何行，COUNT()会返回0，而SUM()、AVG()、MAX()和MIN()会返回NULL，例如：

```sql
SELECT AVG(score) average FROM students WHERE gender = 'X';
```

每页3条记录，使用聚合查询获得总页数，向上取整：

```sql
SELECT CEILING(COUNT(*) / 3) FROM students;
```





#### 多表查询（笛卡尔查询）

##### 语法

```sql
SELECT * FROM <表1> <表2>
```

查询结果集行数是`表1`和`表2`的行数乘积，列数是`表1`和`表2`的列数和



```sql
SELECT * FROM students, classes;
```

查询`students` 表和`classes`表的结果集



##### 例子

多表查询时，对于相同名字的列可以取别名

```sql
SELECT
    students.id sid,
    students.name,
    students.gender,
    students.score,
    classes.id cid,
    classes.name cname
FROM students, classes;
```

给表名取别名，再通过表名的别名引用列和设置别名

```sql
SELECT
    s.id sid,
    s.name,
    s.gender,
    s.score,
    c.id cid,
    c.name cname
FROM students s, classes c;
```

上述语句给`students`和`classes`分别取别名`s`，`c`，再通过`s.id sid`，`c.id cid`的方式设置别名



可以添加`WHERE`子句

```sql
SELECT
    s.id sid,
    s.name,
    s.gender,
    s.score,
    c.id cid,
    c.name cname
FROM students s, classes c
WHERE s.gender = 'M' AND c.id = 1;
```



#### 连接查询

##### 语法

```sql
SELECT <列名> FROM <表1> INNER JOIN <表2> ON <条件...>
```

先确定主表：<表1>，再确定连接表：<表2>，使用`INNER JOIN`，然后加上条件，使用`ON`。后面可接`WHERE`、`ORDER BY`子句



`INNER JOIN`表示内连接，只返回同时存在于两张表的行数据，此外还有：

`OUTER JOIN`：外连接

`RIGHT OUTER JOIN`：右外连接，返回右表都存在的行，某行仅存在右表，结果集就会以NULL填充剩下的字段

`LEFT OUTER JOIN`：左外连接，返回左表都存在的行，某行仅存在左表，结果集就会以NULL填充剩下的字段

`FULL OUTER JOIN`：全外连接，把两张表的所有记录全部选择出来，并且，自动把对方不存在的列填充为NULL



##### 例子

### 修改数据

#### INSERT

#### UPDATE

#### DELETE