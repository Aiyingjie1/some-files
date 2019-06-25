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