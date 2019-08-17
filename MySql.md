# MySql#

#### Mysql的一些关键字顺序

书写sql语句时各关键字的顺序： 
select ->from ->where ->group by ->having ->order by

执行顺序： 
from ->where ->group by ->having ->select ->order by

#### limit####

　Limit子句可以被用于强制 SELECT 语句返回指定的记录数。Limit接受一个或两个数字参数。参数必须是一个整数常量。如果给定两个参数，第一个参数指定第一个返回记录行的偏移量，第二个参数指定返回记录行的最大数目。

```sql
//初始记录行的偏移量是 0(而不是 1)：

　　mysql> SELECT * FROM table LIMIT 5,10; //检索记录行6-15

　　//为了检索从某一个偏移量到记录集的结束所有的记录行，可以指定第二个参数为 -1：

　　mysql> SELECT * FROM table LIMIT 95,-1; // 检索记录行 96-last

　　//如果只给定一个参数，它表示返回最大的记录行数目。换句话说，LIMIT n 等价于 LIMIT 0,n：

　　mysql> SELECT * FROM table LIMIT 5;     //检索前 5 个记录行

```

**Limit的效率高？**

　　常说的Limit的执行效率高，是对于一种特定条件下来说的：即数据库的数量很大，但是只需要查询一部分数据的情况。
　　高效率的原理是：避免全表扫描，提高查询效率。

　　比如：每个用户的email是唯一的，如果用户使用email作为用户名登陆的话，就需要查询出email对应的一条记录。
　　SELECT * FROM t_user WHERE email=?;
　　上面的语句实现了查询email对应的一条用户信息，但是由于email这一列没有加索引，会导致全表扫描，效率会很低。
　　SELECT * FROM t_user WHERE email=? LIMIT 1;
　　加上LIMIT 1，只要找到了对应的一条记录，就不会继续向下扫描了，效率会大大提高。

 

**Limit的效率低？**

　　在一种情况下，使用limit效率低，那就是：只使用limit来查询语句，并且偏移量特别大的情况

　　做以下实验：
​      语句1：
​         　　select * from table limit 150000,1000;
　　语句2:
​         　　select * from table while id>=150000 limit 1000;
　　语句1为0.2077秒；语句2为0.0063秒
　　两条语句的时间比是：语句1/语句2＝32.968
　　比较以上的数据时，我们可以发现采用where...limit....性能基本稳定，受偏移量和行数的影响不大，而单纯采用limit的话，受偏移量的影响很大，当偏移量大到一定后性能开始大幅下降。不过在数据量不大的情况下，两者的区别不大。

　　所以应当先使用where等查询语句，配合limit使用，效率才高

　　ps：在sql语句中，limt关键字是最后才用到的。以下条件的出现顺序一般是：where->group by->having-order by->limit

　

#### ifnull####

IFNULL(expr1,expr2)

如果expr1不是NULL，IFNULL()返回expr1，否则它返回expr2。IFNULL()返回一个数字或字符串值



#### concat####

mysql CONCAT(str1,str2,…)

返回结果为连接参数产生的字符串。如有任何一个参数为NULL ，则返回值为 NULL。或许有一个或多个参数。 如果所有参数均为非二进制字符串，则结果为非二进制字符串。 如果自变量中含有任一二进制字符串，则结果为一个二进制字符串。一个数字参数被转化为与之相等的二进制字符串格式；若要避免这种情况，可使用显式类型 cast, 例如： SELECT CONCAT(CAST(int_col AS CHAR), char_col)

~~~mysql
mysql> SELECT CONCAT(’My’, ‘S’, ‘QL’);

-> ‘MySQL’

mysql> SELECT CONCAT(’My’, NULL, ‘QL’);

-> NULL

mysql> SELECT CONCAT(14.3);

-> ‘14.3′
~~~



#### 左连接，右连接，内连接

##### 内连接

关键字：inner join on

语句：select * from a_table a inner join b_table bon a.a_id = b.b_id;

说明：组合两个表中的记录，返回关联字段相符的记录，也就是返回两个表的交集（阴影）部分。![img](https://img-blog.csdn.net/20171209135846780?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcGxnMTc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

##### 左连接

关键字：left join on / left outer join on

语句：select * from a_table a left join b_table bon a.a_id = b.b_id;

说明：left join 是left outer join的简写，它的全称是左外连接，是外连接中的一种。左(外)连接，左表(a_table)的记录将会全部表示出来，而右表(b_table)只会显示符合搜索条件的记录。右表记录不足的地方均为NULL。

![img](https://img-blog.csdn.net/20171209142610819?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcGxnMTc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

##### 右链接

关键字：right join on / right outer join on

语句：select * from a_table a right outer join b_table b on a.a_id = b.b_id;

说明：right join是right outer join的简写，它的全称是右外连接，是外连接中的一种。与左(外)连接相反，右(外)连接，左表(a_table)只会显示符合搜索条件的记录，而右表(b_table)的记录将会全部表示出来。左表记录不足的地方均为NULL。

![img](https://img-blog.csdn.net/20171209144056668?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcGxnMTc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

####编码

```sql
/* 字符集编码 */ ------------------
-- MySQL、数据库、表、字段均可设置编码
-- 数据编码与客户端编码不需一致
SHOW VARIABLES LIKE 'character_set_%'    -- 查看所有字符集编码项
    character_set_client        客户端向服务器发送数据时使用的编码
    character_set_results        服务器端将结果返回给客户端所使用的编码
    character_set_connection    连接层编码
SET 变量名 = 变量值
    set character_set_client = gbk;
    set character_set_results = gbk;
    set character_set_connection = gbk;
SET NAMES GBK;    -- 相当于完成以上三个设置
```

#### 函数

```sql
--// 内置函数 ----------
-- 数值函数
abs(x)            -- 绝对值 abs(-10.9) = 10
format(x, d)    -- 格式化千分位数值 format(1234567.456, 2) = 1,234,567.46
ceil(x)            -- 向上取整 ceil(10.1) = 11
floor(x)        -- 向下取整 floor (10.1) = 10
round(x)        -- 四舍五入去整
mod(m, n)        -- m%n m mod n 求余 10%3=1
pi()            -- 获得圆周率
pow(m, n)        -- m^n
sqrt(x)            -- 算术平方根
rand()            -- 随机数
truncate(x, d)    -- 截取d位小数

-- 时间日期函数
now(), current_timestamp();     -- 当前日期时间
current_date();                    -- 当前日期
current_time();                    -- 当前时间
date('yyyy-mm-dd hh:ii:ss');    -- 获取日期部分
time('yyyy-mm-dd hh:ii:ss');    -- 获取时间部分
date_format('yyyy-mm-dd hh:ii:ss', '%d %y %a %d %m %b %j');    -- 格式化时间
unix_timestamp();                -- 获得unix时间戳
from_unixtime();                -- 从时间戳获得时间

-- 字符串函数
length(string)            -- string长度，字节
char_length(string)        -- string的字符个数
substring(str, position [,length])        -- 从str的position开始,取length个字符
replace(str ,search_str ,replace_str)    -- 在str中用replace_str替换search_str
instr(string ,substring)    -- 返回substring首次在string中出现的位置
concat(string [,...])    -- 连接字串
charset(str)            -- 返回字串字符集
lcase(string)            -- 转换成小写
left(string, length)    -- 从string2中的左边起取length个字符
load_file(file_name)    -- 从文件读取内容
locate(substring, string [,start_position])    -- 同instr,但可指定开始位置
lpad(string, length, pad)    -- 重复用pad加在string开头,直到字串长度为length
ltrim(string)            -- 去除前端空格
repeat(string, count)    -- 重复count次
rpad(string, length, pad)    --在str后用pad补充,直到长度为length
rtrim(string)            -- 去除后端空格
strcmp(string1 ,string2)    -- 逐字符比较两字串大小
```


### EXPLAN详解

~~~sql
mysql> explain update users set name='test' where userid=1;
+----+-------------+-------+------------+-------+---------------+---------+---------+-------+---
|id|select_type|table|partitions|type|possible_keys|key|key_len|ref|rows|filtered|Extra|
+----+-------------+-------+------------+-------+---------------+---------+---------+-------+---
|1 | UPDATE    |users| NULL     |range| PRIMARY    |PRIMARY|8  |const|1 |100.00  |Using where|
+----+-------------+-------+------------+-------+---------------+---------+---------+-------+---
~~~

输出列	                  含义
id	                          查询标识
select_type	          查询类型
table	                  查询涉及的表
partitions	          匹配到的分区信息
type	                  连接类型
possible_keys	  可能选择的索引
key	                          实际使用的索引
key_len	                  实际使用的索引的长度
ref	                          和索引进行比较的列
rows	                  需要被检索的大致行数
filtered	                  按表条件过滤的行百分比
Extra	                  额外信息

1.**id**    id相同，执行顺序**由上至下**；如果是**子查询**，id的序号会**递增**，id值越大优先级**越高**，越**先**被执行；id如果相同，可以认为是一组，从上往下顺序执行；在所有组中，id值越大，优先级越高，越先执行；当该行数据引用了其它查询的**UNION结果**时，**id列显示为NULL**，此时，**table列显示类似这样：< unionM,N>**，表示引用了查询标识为M、N的两行的UNION结果。

2.**selectType**  selectType                                    含义

​    			 SIMPLE	                                        简单查询（不包含子查询或UNION)
   		 	 PRIMARY	                                最外层查询
   			 UNION	                                        UNION语句中第二或更后面的查询
   			 DEPENDENT UNION	                依赖外部查询的UNION中第二或更后面的查询
​    			UNION RESULT	                        UNION语句的结果集
​    			SUBQUERY	                                子查询中的第一个查询
   		        DEPENDENT SUBQUERY	        依赖外部查询的子查询中的第一个查询
​    			DERIVED	                                查询的派生表(在FROM从句中的子查询）
  		        MATERIALIZED	                        物化子查询
​    			UNCACHEABLE SUBQUERY	无法缓存结果的子查询，并且必须为外部查询的每一行重新计算
​    			UNCACHEABLE UNION	        属于无法缓存的子查询的UNION的第二或更后面的查询
3.**table**

输出行引用的表的名称。这也可以是下列值之一:

​								< unionM,N>:输出行引用了id值为M和N的行的UNION结果。 
​								< derivedN>:该行引用了一个id值为n的行的派生表结果。 
​								例如，派生表可以从from子句的子查询中得到结果。 
​								< subqueryN>:输出行引用了id值为N的行的物化子查询的结果。

4.**partitions**         由查询匹配记录的分区。对于非分区表，值为NULL。

5.**type**  表示MySQL在表中找到所需行的方式，又称“访问类型”，常见类型从最好到最差依次如下:

system:表只有一行记录，这是const类型的一种特殊情况。

const:常量查询.在整个查询过程中，该表最多有一个匹配行，在查询开始时读取。 因为只有一行，所以这个行中的列值可以被其他优化器视为常量。const表非常快，因为它们只读一次。 当你将**主键或唯一索引**的所有部分与常量值进行比较时，将使用const。 

eq_ref:对前面的表中每一行记录的组合，都从当前表中读取一行。 除了system和const类型之外，这是最好的连接类型。 主键或非空唯一索引在索引的所有部分都被join使用时采用。 **eq_ref可用于与使用=操作符**进行比较的索引列。 比较值可以是一个常量，也可以是一个在该表之前读取的表的列的表达式。**类似ref，区别就在使用的索引是唯一索引**，对于每个**索引键值，表中只有一条记录**匹配，简单来说，就是**多表连接中使用primary key或者 unique key作为关联条件**。

 ref:对前面的表中每一行记录的组合，都从当前表中读取所有匹配索引值的行。 如果join仅使用key的最左前缀，或者key不是主键或唯一索引(换句话说，**如果连接不能基于key值选择单个行**)，**则使用ref**。 如果使用的key只匹配几行，这是一个很好的连接类型。简单来说，就是使用**非唯一索引扫描或者唯一索引的前缀扫描**，返回匹配某个单独值的记录行

fulltext:连接是使用全文索引执行的。

ref_or_null:这个连接类型就像ref，但是加上MySQL对包含空值的行进行了额外的搜索。 这种连接类型优化通常用于解决子查询。 

index_merge:此连接类型指示使用索引合并优化。 在此情形下，输出行中的key列包含所使用的索引列表，key_len列包含用于所使用索引的最长key部分的列表。

unique_subquery:该类型在某些如下格式的IN子查询中替代eq_ref

​			value IN (SELECT primary_key FROM single_table WHERE some_expr) ;unique_subquery只是一个索引查找函数，它完全替代了子查询以提高效率。

index_subquery:该类型与unique_subquery很相似。它也是替代IN子查询，但它适用如下格式的子查询中的非唯一索引： value IN (SELECT key_column FROM single_table WHERE some_expr)

range:只有在给定范围内的行被检索，才使用索引来选择行。 输出行中的key列指示使用哪个索引。 key_len包含所使用的最长key部分。 此类型的ref列为NULL。 range用于当一个key列与一个常量使用=，<>，>，>=，<，<=，<=>，BETWEEN，或IN()操作符进行比较时.

 index：索引连接类型与ALL相同，只是扫描索引树。有两种情况:

​            如果索引是查询的覆盖索引，并且可以用来满足表所需的所有数据，那么只扫描索引树。此种情况下，Extra列显示Using index。一个索引扫描通常比ALL都快，因为索引的大小通常小于表数据。
​           一个全表扫描以索引顺序读取索引来查找数据行。Extra列不显示Using index。按索引次序扫描，先读索引，再读实际的行，结果还是全表扫描，主要优点是避免了排序。因为索引是排好的

 ALL：全表扫描。 
6.**possible_keys**  该列指示了MySQL查找表中的行时可以选择的索引。 

​		注意，这个列完全独立于来自EXPLAIN的输出中显示的表的顺序。 这意味着，可能在使用生成的表顺序时，一些可能的键可能无法使用。如果该列为空，则没有相关的索引。在这种情况下，您可以通过检查WHERE子句来检查是否引用了适合于索引的某些列或列，从而提高查询的性能。如果是这样，创建一个适当的索引并再次检查查询。

7.**key**    显示MySQL在查询中实际决定使用的索引，若没有使用索引，显示为NULL

8.**key_len**     key_len列表示MySQL决定使用的键的长度（字节数）。key_len的值使您能够确定MySQL实际使用了一个多列索引的哪些部分。 key_len显示的值为索引字段的最大可能长度，并非实际使用长度，即key_len是根据表定义计算而得，不是通过表内检索出的。

9.**ref**    ref列显示哪些列或常量与键列中命名的索引相比较，以从表中选择行。

10.**rows**   表示MySQL认为执行该查询必须检查的行数。这是根据表统计信息及索引选用情况得出地结果。

11.**filtered**  该列表示将被表条件过滤的表行的估计百分比。 

12.**Extra**   

​	1.Using index 该值表示相应的select操作中使用了覆盖索引（Covering Index）。MySQL可以利用索引返回select列表中的字段，而不必根据索引再次读取数据文件 ，包含所有满足查询需要的数据的索引称为覆盖索引（Covering Index）。

​	2.Using where 表示mysql服务器将在存储引擎检索行后再进行过滤。许多where条件里涉及索引中的列，当（并且如果）它读取索引时，就能被存储引擎检验，因此不是所有带where字句的查询都会显示”Using where”。有时”Using where”的出现就是一个暗示：查询可受益于不同的索引。

​	3.Using temporary  表示MySQL需要使用临时表来存储结果集，常见于排序和分组查询。

​	4.Using filesort  MySQL中无法利用索引完成的排序操作称为“文件排序”。

​	5.Using join buffer  该值强调了在获取连接条件时没有使用索引，并且需要连接缓冲区来存储中间结果。如果出现了这个值，那应该注意，根据查询的具体情况可能需要添加索引来改进能。

​	6.Impossible where  这个值强调了where语句会导致没有符合条件的行。

​	7.Select tables optimized away  这个值意味着仅通过使用索引，优化器可能仅从聚合函数结果中返回一行。

​	8.Index merges  当MySQL 决定要在一个给定的表上使用超过一个索引的时候，就会出现以下格式中的一个，详细说明使用的索引以及合并的类型。 

​	