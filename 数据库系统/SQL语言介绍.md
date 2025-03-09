# Introduction to SQL

## 1 Data Definition

### 1.1 Domain Types in SQL

- `char(n).` Fixed length character string, with user-specified length n. 
   定长字符串. C 语言里字符串结尾有 `\0`, 但数据库里没有，长度由定义而得。  
- `varchar(n).` Variable length character strings, with user-specified maximum length n.
   不定长字符串。不同的数据类型比较可能有问题（比如定长和不定长的字符串）
- `int.` Integer (a finite subset of the integers that is machine-dependent).
- `smallint.` Small integer (a machine-dependent subset of the integer domain type).
- `numeric(p,d).` Fixed point number, with user-specified precision of p digits, with d digits to the right of decimal point.   
   p 表示有效数字位数, d 表示小数点后多少位。 *e.g.* `number(3,1)` allows 44.5 to be store exactly,  but neither 444.5 or 0.32
- real, double precision.  Floating point and double-precision floating point numbers, with machine-dependent precision.
- `float(n).` Floating point number, with user-specified precision of at least n digits.

### 1.2 Built-in Data Types in SQL

- **date**: Dates, containing a (4 digit) year, month and date
   *e.g.* date ‘2005-7-27’
- **time**: Time of day, in hours, minutes and seconds. *e.g.* time  ‘09:00:30’  time ‘09:00:30.75’
- **timestamp** （时间戳）: date plus time of day ***e.g.\*** timestamp  ‘2005-7-27 09:00:30.75’
- **interval**（时间段） : period of time *e.g.* interval  ‘1’ day  
  - Subtracting a date/time/timestamp value from another gives an interval value.  
  - Interval values can be added to date/time/timestamp values
  - built-in date, time functions: `current_date(), current_time(), year(x), month(x), day(x), hour(x), minute(x), second(x)`

### 1.3 Create Table Construct

  ![img](C:/Users/22094/Desktop/%E7%AC%94%E8%AE%B0/%E6%95%B0%E6%8D%AE%E5%BA%93%E7%B3%BB%E7%BB%9F/assets/202303131016449.png) 

如果引用的表中有条目被删除，可能会破坏完整性约束条件。有下面的方法：  

- *restrict*: 如果有条目是被引用的，那么不允许删除。
- *cascade*: 引用的条目被删了之后，引用者也一并删除
- *set null*: 引用者的指针设为 `null`. 
- *set default* 

如果引用的表中有更新，也有类似上面的四种方法。
 在 `create table` 中定义  

- `on delete cascade  |set null |restrict |set default`
- `on update cascade  |set null |restrict |set default`

### 1.4 Drop and Alter Table Constructs

- drop table student Deletes the table and its contents 
   把内容和表全删了，之后不能再往表里插入。
- delete from student
   Deletes all contents of table, but retains table

​	删除table中所有的数据，但是保留这个表的结构

- alter table 

  可以动态修改表的定义 

  - ```
    alter table r add A D
    ```

    - where A is the name of the attribute to be added to relation r  and D is the domain of A.
    - All tuples in the relation are assigned null as the value for the new attribute.
       ***e.g.\*** `alter table student add resume varchar(256);`
    - 还可以增加外键约束条件，也可以删掉

  - ```
    alter table r drop A
    ```

    - where A is the name of an attribute of relation r
    - *Dropping of attributes not supported by many databases* 
       可以生成一个新的表，然后把除了要删的列以外的列搬移过去。  

## 2 Basic Query Structure

* select : 投影，选择类

* where：条件筛选

* from：选择数据表

* natural join ：同名属性自然链接

* as：重命名 `old_name as new_name`

* like : 用于查找字符串 

  * **percent (%)**.  The % character matches any substring. 	多字符通配符
     ***e.g.\*** `select name from instructor where name like '%dar%';` 找名字里面含有 `dar` 的字符串。 
  * **underscore (_)**.  The _ character matches any character.     单字符通配符

* order by ：按属性排序

* limit ：限制返回的行数 `limit 3` 表示返回3行

* set operations ：集合操作

  * `union, intersect, except` 是严格的集合操作，会对结果去重.
  * `union all`, `intersect all` and `except all`. 保持多重集。

* **null** signifies *an unknown value* or that *a value does not exist*.

  - The result of any arithmetic expression involving null is null.	
     ***e.g.\*** `5 + null` returns null

    null 和代数的计算结果还是null

  - The predicate is null can be used to check for null values.           
     ***e.g.\*** Find all instructors whose salary is null. 
     `select name from instructor where salary is null`

    可以查看一个值是否是null

  - Comparisons with null values return the special truth value: **unknown**.  

     ![img](C:/Users/22094/Desktop/%E7%AC%94%E8%AE%B0/%E6%95%B0%E6%8D%AE%E5%BA%93%E7%B3%BB%E7%BB%9F/assets/202303131118989.png) 

  - Result of select predicate is treated as false if it evaluates to unknown

    如果一个值是null会返回false

* **Aggregation Fuctions** 聚合函数

  ![img](C:/Users/22094/Desktop/%E7%AC%94%E8%AE%B0/%E6%95%B0%E6%8D%AE%E5%BA%93%E7%B3%BB%E7%BB%9F/assets/202303131120904.png) 

​	注意在 `select` 里出现的属性，除了统计函数以外，一定要是分组属性里出现过的。	

* having : 对分组后的结果进行筛选
* The **`with`** clause provides a way of defining a temporary view whose definition is available only to the query in  which the with clause occurs.
   构造一个临时表  

