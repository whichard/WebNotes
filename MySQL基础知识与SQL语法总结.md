### 主键（primary key）

一列（或一组列），其值能够唯一区分表中每个行。

表中的任何列都可以作为主键，只要它满足以下条件：
 任意两行都不具有相同的主键值；
 每个行都必须具有一个主键值（主键列不允许NULL值）

主键的最好习惯 除MySQL强制实施的规则外，应该坚持的几个普遍认可的最好习惯为：
 不更新主键列中的值；
 不重用主键列的值；
 不在主键列中使用可能会更改的值。（例如，如果使用一个
名字作为主键以标识某个供应商，当该供应商合并和更改其名字时，必须更改这个主键。）

### 行列

列（column）表中的一个字段。所有表都是由一个或多个列组成的。
理解列的最好办法是将数据库表想象为一个网格。网格中每一列存着一条特定的信息。例如，在顾客表中，一个列存储着顾客编号，另一个列存储着顾客名，而地址、城市、州以及邮政编码全都存储在各自列中。

行（row）: 表中的一个记录. 表中的数据是按行存储的，所保存的每个记录存储在自己的行内。

是记录还是行？你可能听到用户在提到行（row）时称其为数据库记录（record）。在很大程度上，这两个术语是可以互相替代的，但从技术上说，行才是正确的术语。

## [增删查改](http://www.runoob.com/mysql/mysql-create-database.html)

SHOW DATABASES; 返回可用数据库的一个列表。

use abc; 切换到数据库abc.

SHOW TABLES; 获得一个数据库内的表的列表.

SHOW COLUMNS FROM customers;  SHOW COLUMNS要求给出一个表名（这个例子中的FROMcustomers），它对每个字段返回一行，行中包含字段名、数据类型、是否允许NULL、键信息、默认值以及其他信息（如字段cust_id的auto_increment）。

	DESCRIBE语句MySQL支持用DESCRIBE作为SHOW COLUMNS FROM的一种快捷方式。换句话说，**DESCRIBE customers；是SHOW COLUMNS FROM cuStomers；的一种快捷方式。**

#### [创建数据库](http://www.runoob.com/mysql/mysql-create-database.html)

```sql
CREATE DATABASE 数据库名;
```

#### 创建表格

```sql
mysql> create table MyClass(
> id int(4) not null primary key auto_increment,
> name char(20) not null,
> sex int(4) not null default '0',
> degree double(16,2));
```

#### 插入数据insert

```sql
 mysql> insert into MyClass values(1,'Tom',96.45),(2,'Joan',82.99), (2,'Wang', 96.59);
```

字符需要用单引号包括。

#### 删除行delete

```sql
mysql> delete from MyClass where id=1;
```

#### 修改行update

```sql
mysql> update MyClass set name='Mary' where id=1;
```

#### 修改表名

命令：rename table 原表名 to 新表名;

例如：在表MyClass名字更改为YouClass

```
mysql> rename table MyClass to YouClass;
```

### 字段

##### 增加字段

在表MyClass中添加了一个字段passtest，类型为int(4)，默认值为0

```sql
mysql> alter table MyClass add passtest int(4) default '0'
```

##### 修改原字段名称及类型：

```sql
mysql> ALTER TABLE table_name CHANGE old_field_name new_field_name field_type;
```

##### 删除字段：

```sql
MySQL ALTER TABLE table_name DROP field_name;
```

### 索引

##### 加索引

   mysql> alter table 表名 add index 索引名 (字段名1[，字段名2 …]);

例子： mysql> alter table employee add index emp_name (name);

##### 加主关键字的索引

  mysql> alter table 表名 add primary key (字段名);

例子： mysql> alter table employee add primary key(id);

##### 加唯一限制条件的索引

   mysql> alter table 表名 add unique 索引名 (字段名);

例子： mysql> alter table employee add unique emp_name2(cardnumber);

##### 删除某个索引

   mysql> alter table 表名 drop index 索引名;

例子： mysql>alter table employee drop index emp_name;

#### 大小写

SQL语句和大小写请注意，SQL语句不区分大小写，因此SELECT与select是相同的。同样，写成Select也没有关系。
**习惯:** 许多SQL开发人员喜欢**对所有SQL关键字使用大写**，而对所有列和表名使用小写，这样做使代码更易于阅读和调试。

使用空格在处理SQL语句时，其中所有空格都被忽略。SQL语句可以在一行上给出，也可以分成许多行。多数SQL开发人员认为将SQL语句分成多行更容易阅读和调试。

---

其他常见操作通过此教程看也许更快：http://www.runoob.com/mysql/mysql-regexp.html

---

## 检索

检索单个列: select a from products; SELECT a FROM products;

检索多个列: select a, b, c from products;

#### 使用通配符*

检索所有列: SELECT * FROM products; 

 : 一般，除非你确实需要表中的每个列，否则最好别使用*通配符。虽然使用通配符可能会使你自己省事，不用明确列出所需列，但检索不需要的列通常会降低检索和应用程序的性能。

#### limit关键字

limit int a, int b: select * from category_ limit 0,5; limit语法, LIMIT5，5指示MySQL返回从行5开始的5行。第一个数为开始位置，第二个数为要检索的行数。

#### 完全限定名

SELECT products.prod_name FROM database.products;同时使用表名和列字来引用列

## 排序检索数据ORDER BY

子句（clause）SQL语句由子句构成，有些子句是必需的，而有的是可选的。一个子句通常由一个关键字和所提供的数据组成。子句的例子有SELECT语句的FROM子句，我们在前一章看到过这个子句。

`SELECT prod_name FROM products ORDER BY prod_name; `

这条语句除了指示MySQL对prod_name列以字母顺序排序数据的ORDERBY子句外，与前面的语句相同。

#### 按多个列排序

按多个列排序，只要指定列名，列名之间用逗号分开即可

下面的代码检索3个列，并按其中两个列对结果进行排序一—首先按价格，然后再按名称排序。
SELECT prod_id，prod_price，prod_name FROM products ORDER BY prod_price，prod_name；

> 换句话说，对于上述例子中的输出，仅在多个行具有相同的prod_price值时才对产品按prod_name进行排序。如果prod_price列中所有的值都是唯一的，则不会按prod_name排序。

#### 降序排列DESC (Descending Order)

数据排序不限于升序排序（从A到z）。这只是默认的排序顺序，还可以使用ORDER BY子句以降序（从z到A）顺序排序。为了进行降序排序，必须指定DESC关键字。(Descending Order)
下面的例子按价格以降序排序产品（最贵的排在最前面）：
SELECT prod_id，prod_price，prod_nameFROM products ORDER BY prod_price DESC；

但是，如果打算用多个列排序怎么办？下面的例子以降序排序产品（最贵的在最前面），然后再对产品名排序：
SELECT prod_id，prod_price，prod_name FROM products ORDER BY prod_price DESC，prod_name；

**DESC关键字只应用到直接位于其前面的列名**。在上例中，只对prod_price列指定DESC，对prod_name列不指定。因此，prod_price列以降序排序，而prod_name列（在每个价格内）仍然按标准的升序排序。在多个列上降序排序如果想在多个列上进行降序排序，必须对每个列关键字指定DESC关键字.

#### 使用ORDER BY和LIMIT的组合，能够找出一个列中最高或最低的值。

下面的例子演示如何找出最昂贵物品的值：
SELECT prod_price FROM products ORDER BY prod_price DESC LIMIT 1；

在给出ORDERBY子句时，应该保证它位于FROM子句之后。如果使用LIMIT，它必须位于ORDERBY之后。使用子句的次序不对将产生错误消息。

## 过滤数据 WHERE

本章将讲授如何使用 SELECT 语句的 WHERE 子句指定搜索条件。

在SELECT语句中，数据根据WHERE子句中指定的搜索条件进行过滤。
WHERE子句在表名（FROM子句）之后给出，如下所示：

SELECT prod_name，prod_price FROM products WHERE prod_price=2.50；

这条语句从products表中检索两个列，但不返回所有行，只返回prod_price值为2.50的行

![](https://imgconvert.csdnimg.cn/aHR0cDovL3d3MS5zaW5haW1nLmNuL2xhcmdlL2UwNjlmNjBlbHkxZndoYmJjdmdyeWoyMG03MGI3NzR0LmpwZw)

下面的例子说明如何使用BETWEEN操作符，它检索价格在5美元和10美元之间的所有产品：(包含5和10)
输入SELECT prod_name，prod_price FROM products WHERE prod_price BETWEEN 5 AND 10；
使用 BETWEEN 时，必须指定两个值, 包含所指定的开始和结束值.

#### 空值检查IS NULL

SELECT语句有一个特殊的WHERE子句，可用来检查具有NULL值的列。
这个WHERE子句就是IS NULL子句。其语法如下：
SELECT prod_name FROM products WHERE prod_price IS NULL；
这条语句返回没有价格（空prod_price字段，不是价格为0）的所有产品，由于表中没有这样的行，所以没有返回数据。

#### AND操作符

为了通过不止一个列进行过滤，可使用AND操作符给WHERE子句附加条件。下面的代码给出了一个例子：
SELECT prod_id，prod_price，prod_name FROM products WHERE vend_id=1003 AND prod_price<=10；

此SQL语句检索由供应商1803制造且价格小于等于10美元的所有产品的名称和价格。这条SELECT语句中的WHERE子句包含两个条件，并且用AND关键字联结它们。AND指示DBMS只返回满足所有给定条件的行。如果某个产品由供应商1003制造，但它的价格高于10美元，则不检索它。类似，如果产品价格小于10美元，但不是由指定供应商制造的也不被检索。

可以添加多个过滤条件，每添加一条就要使用一个 AND.

AND的优先级高于OR. 可以使用圆括号将操作条件隔开.

#### OR操作符

检索符合任一条件的所有行。
请看如下的SELECT语句：
SELECT prod_name，prod_price FROM products WHERE vend_id=1002 OR vend_id=1003；

此SQL语句检索由任一个指定供应商制造的所有产品的产品名和价格。OR操作符告诉DBMS匹配任一条件而不是同时匹配两个条件。如果这里使用的是AND操作符，则没有数据返回（此时创建的WHERE子句不会检索到匹配的产品）。

#### AND的优先级高于OR. 

`SELECT prod_name,prod_price FROM products WHERE vend_id=1002 OR vend_id=1003 AND prod_price>=10;` 会先操作合并后面两个 

##### 可以使用圆括号将操作条件隔开.

`SELECT prod_name,prod_price FROM products WHERE(vend_id=1002 OR vend_id=1003)AND prod_price>=10;`

分析这条SELECT语句与前一条的唯一差别是，这条语句中，前两个条件用圆括号括了起来。因为圆括号具有较AND或OR操作符高的计算次序，DBMS首先过滤圆括号内的0R条件。这时，SQL语句变成了选择由供应商1002或1003制造的且价格都在10美元（含）以上的任何产品，这正是我们所希望的。

#### IN操作符

提供一个集合, 匹配集合中的所有条件都列出(**相当于OR**)

SELECT prod_name,prod_price FROM products WHERE vend_id IN(1002,1003, 1005)
ORDER BY prod_name;

此SELECT语句检索供应商1002, 1003和1005制造的所有产品。IN操作符后跟由逗号分隔的合法值清单，整个清单必须括在圆括号

#### NOT操作符

SELECT prod_name,prod_price FROM products WHERE vend_id NOT IN(1002,1003)
ORDER BY prod_name;

分析这里的NOT否定跟在它之后的条件，因此，MySQL不是匹配1002和1003的vend_id，而是匹配1002和1003之外供应商的vend_id。
为什么使用NoT？对于简单的WHERE子句，使用NOT确实没有什么优势。但在更复杂的子句中，NOT是非常有用的。例如，在与IN操作符联合使用时，NOT使找出与条件列表不匹配的行非常简单。

MySQL**支持使用NOT对IN、BETWEEN和EXISTS子句取反**，这与多数其他DBMS允许使用NOT对各种条件取反有很大的差别。

## 通配符过滤

通配符是一种极重要和有用的搜索工具, 通配符搜索的处理一般要比前面讨论的其他搜索所花时间更长。使用注意:

- 不要过度使用通配符。如果其他操作符能达到相同的目的，应该使用其他操作符。
- 在确实需要使用通配符时，除非绝对有必要，否则不要把它们用在搜索模式的开始处。把通配符置于搜索模式的开始处，搜索起来是最慢的。
- 仔细注意通配符的 位置。如果放错地方，可能不会返回想要的数据.

### LIKE操作符

怎样搜索产品名中包含文本anvil的所有产品？用简单的比较操作符肯定不行，必须使用通配符. 为在搜索子句中使用通配符，必须使用 LIKE 操作符。从技术上说， **LIKE 是谓词**而不是操作符。

**通配符**（wildcard）用来匹配值的一部分的特殊字符。
**搜索模式**（search pattern）由字面值、通配符或两者组合构成的搜索条件。
通配符本身实际是SQL的WHERE子句中有特殊含义的字符，SQL支持几种通配符。

#### 百分号通配符 %

最常使用的通配符是百分号（%）。在搜索串中，**%表示任何字符出现任意次数**。例如，为了找出所有以词jet起头的产品，可使用以下SELECT语句：

SELECT prod_id，prod_name FROM products WHERE prod_name LIKE 'jet%'；

分析此例子使用了搜索模式’jet%’。在执行这条子句时，将检索任意以jet起头的词。%告诉MySQL接受jet之后的任意字符，不管它有多少字符。搜索可以是区分大小写的。如果区分大小写， 'jet%' 与 JetPack 1000 将不匹配.

##### 在搜索模式中任意位置使用

通配符可在搜索模式中任意位置使用，并且可以使用多个通配符。

SELECT prod_id,prod_ name FROM products WHERE prod_name LIKE '%anvil%';

搜索模式%anvi1%’表示匹配任何位置包含文本anvil的值，而不论它之前或之后出现什么字符。

 通配符也可以出现在搜索模式的中间，虽然这样做不太有用。下面的例子找出以s起头以e结尾的所有产品：
SELECT prod_name FROM products WHERE prod_name LIKE 's%e'；

重要的是要注意到，除了一个或多个字符外，**%还能匹配0个字符。**%代表搜索模式中给定位置的0个、1个或多个字符。

##### 注意尾空格尾空格

注意尾空格尾空格可能会干扰通配符匹配。例如，在保存词anvil时，如果它后面有一个或多个空格，则子句WHERE prod_name LIKE‘%anvi1'将不会匹配它们，因为在最后的1后有多余的字符。解决这个问题的一个简单的办法是在搜索模式最后附加一个%。一个更好的办法是使用函数（第11章将会介绍）去掉首尾空格。

注意NULL 虽然似乎 % 通配符可以匹配任何东西，但有一个例外，即 NULL 。即使是 WHERE prod_name LIKE '%' 也不能匹配用值 NULL 作为产品名的行。

#### 下划线_通配符

另一个有用的通配符是下划线（_）。下划线的用途与%一样，但下划线只匹配单个字符而不是多个字符。

SELECT prod_id，prod_name FROM products WHERE prod_name LIKE '_ton anvil'；

只能匹配一个, 不能多也不能少(不能匹配0个).

## 正则表达式[REGEXP](http://www.runoob.com/mysql/mysql-regexp.html)

```sql
SELECT prod_name FROM products WHERE prod_name REGEXP '\\（[O-9]sticks？\\）'
ORDER BY prod_name；
```

正则表达式和Like的区别: like必须是全匹配(like ''中的内容必须和某字段完全匹配), 而regexp可以部分匹配. regexp灵活度更高, like性能更好. 

正则表达式(Regular expression)是用来匹配文本的特殊的串（字符集合）

不区分大小写（即，大写和小写都匹配）。为区分大小写，可使用 BINARY 关键字 

#### . 操作符

 . 是正则表达式语言中一个特殊的字符。它表示匹配任意一个字符.

#### OR操作符|[]

 | 为正则表达式的 OR 操作符

 [] 是另一种形式的 OR 语句. 	[123] Ton 。 [123] 定义一组字符，它的意思是匹配 1 或 2 或 3 ，因此， 1 ton 和 2 ton 都匹配且返回. 正则表达式 [123]Ton为 [1|2|3]Ton 的缩写, 使用后者也合法. 

#### 范围匹配 -

范围不限于完整的集合， [1-3] 和 [6-9] 也是合法的范围。此外，范围不一定只是数值的， [a-z] 匹配任意字母字符。

#### 操作符本身匹配

为了匹配特殊字符，必须用 \ \ 为前导。 \ \ - 表示查找 - ， \ \ . 表示查找 . 。![](https://imgconvert.csdnimg.cn/aHR0cDovL3d3MS5zaW5haW1nLmNuL2xhcmdlL2UwNjlmNjBlbHkxZndoZDl2bDkxdWoyMGV5MDc0bXhlLmpwZw)

![](https://imgconvert.csdnimg.cn/aHR0cDovL3d3MS5zaW5haW1nLmNuL2xhcmdlL2UwNjlmNjBlbHkxZndoZGI1ejZvZmoyMGtxMGNmZGhxLmpwZw)

#### 重复匹配

![](https://imgconvert.csdnimg.cn/aHR0cDovL3d3MS5zaW5haW1nLmNuL2xhcmdlL2UwNjlmNjBlbHkxZndoZGM5NDRhbGoyMGkzMDcxYWFyLmpwZw)

##### 例子: 使用?使得字母s变得可选

```sql
SELECT prod_name FROM products WHERE prod_name REGEXP '\\（[O-9]sticks？\\）'
ORDER BY prod_name；

+-------—-+
|TNT（1 stick）|
I TNT（5 sticks）|
+-----—-+
分析正则表达式\\（[0-9]sticks？\\）需要解说一下。\\（匹配），
[0-9]匹配任意数字（这个例子中为1和5），sticks？匹配stick和sticks（s后的?使s可选，因为?匹配它前面的任何字符的0次或1次出现），\\）匹配）。没有？，匹配stick和sticks会非常困难。
```

#### 定位符

前面的匹配都是匹配任意位置的文本, 如果需要**只**匹配特定位置的文本，需要使用表9-4列出的定位符![](https://imgconvert.csdnimg.cn/aHR0cDovL3d3MS5zaW5haW1nLmNuL2xhcmdlL2UwNjlmNjBlbHkxZndpZDd6bzR2NWoyMGo2MDcxZGc1LmpwZw)

## 计算字段

直接检索拿不到, 而需要从数据库中检索出转换、计算或格式化过的数据 (如所需查找的内容包含在多个字段中)

**字段**基本上与列（column）的意思相同. 数据库列一般称为列，而术语字段通常用在计算字段的连接上

### 拼接字段Concat

将多个列值联结到一起构成单个值 可以按照指定的格式来输出.

在MySQL的 SELECT 语句中，可使用Concat() 函数来拼接两个列。

`SELECT Concat(vend_name,'(',vend_country,')') FROM vendors ORDER BY vend_name;`

Concat（）拼接串，即把多个串连接起来形成一个较长的串。
Concat（）需要一个或多个指定的串，各个串之间用逗号分隔。
上面的SELECT语句连接以下4个元素：口存储在vend_name列中的名字；口包含一个空格和一个左圆括号的串；口存储在vend_country列中的国家；口包含一个右圆括号的串。
从上述输出中可以看到，SELECT语句返回包含上述4个元素的单个列（计算字段）。

#### Trim()函数

MySQL除了支持 RTrim() （正如刚才所见，它去掉串右边的空格），还支持 LTrim() （去掉串左边的空格）以及Trim() （去掉串左右两边的空格）。

## 函数

函数一般是在数据上执行的，它给数据的转换和处理提供了方便

函数没有SQL的可移植性强 ; 使用函数，应该保证做好代码注释

SQL函数常见类型: 字符串处理, 算术操作, 时间日期, 系统信息

### 文本处理函数

![](https://imgconvert.csdnimg.cn/aHR0cDovL3d3MS5zaW5haW1nLmNuL2xhcmdlL2UwNjlmNjBlbHkxZndpZHlhcDZjc2oyMGV1MGFtM3pyLmpwZw)

#### Soundex()函数

表11-1中的SOUNDEX需要做进一步的解释。SOUNDEX是一个将任何文本串转换为描述其语音表示的字母数字模式的算法。SOUNDEX考虑了类似的发音字符和音节，使得能对串进行发音比较而不是字母比较。虽然SOUNDEX不是SQL概念，但MySQL（就像多数DBMS一样）都提供对SOUNDEX的支持。
下面给出一个使用Soundex（）函数 的例子。customers表中有一个顾客Coyote Inc.，其联系名为V.Lee。但如果这是输入错误，此联系名实际应该是Y.Lie，怎么办？显然，按正确的联系名搜索不会返回数据，如

`SELECT cust_name,cust_contact FROM customers WHERE Soundex(cust_contact)=Soundex('Y Lie');`

分析在这个例子中，WHERE子句使用Soundex（）函数来转换cust contact列值和搜索串为它们的SOUNDEX值。因为Y.Lee和Y.Lie发音相似，所以它们的SOUNDEX值匹配，因此WHERE子句正确地过滤出了所需的数据。

#### 日期时间处理函数

较为重要, 使用详见<MySQL必知必会>11章.

![](https://imgconvert.csdnimg.cn/aHR0cDovL3d3MS5zaW5haW1nLmNuL2xhcmdlL2UwNjlmNjBlbHkxZndpZTMzYngwa2oyMGkwMGhwZGlmLmpwZw)

![](https://imgconvert.csdnimg.cn/aHR0cDovL3d3MS5zaW5haW1nLmNuL2xhcmdlL2UwNjlmNjBlbHkxZndpZTRnYTBla2oyMGk3MGI2MHR5LmpwZw)

## 聚集函数

汇总数据而不用把它们实际检索出来

![](https://imgconvert.csdnimg.cn/aHR0cDovL3d3MS5zaW5haW1nLmNuL2xhcmdlL2UwNjlmNjBlbHkxZndpZWV3cDFrbmoyMGdwMDgycTNrLmpwZw)

```sql
SELECT COUNT(*) AS num_items,MIN(prod_price) AS price_min,MAX(prod_price) AS price_max, AVG(prod_price) AS price_avg FROM products;
```

这里用单条 SELECT 语句执行了4个聚集计算，返回4个值（ products 表中物品的数目，产品价格的最高、最低以及平均值）。

## 分组数据

分组数据，以便能汇总表内容的子集

#### 建立分组

分组是在 SELECT 语句的 GROUP BY 子句中建立的

![](https://imgconvert.csdnimg.cn/aHR0cDovL3d3MS5zaW5haW1nLmNuL2xhcmdlL2UwNjlmNjBlbHkxZndpZWpub2lpbWoyMGxyMGQ1NDB4LmpwZw)

在具体使用GROUPBY子句前，需要知道一些重要的规定。

- GROUPBY子句可以包含任意数目的列。这使得能对分组进行嵌套，为数据分组提供更细致的控制。
- 如果在GROUPBY子句中嵌套了分组，数据将在最后规定的分组上进行汇总。换句话说，在建立分组时，指定的所有列都一起计算（所以不能从个别的列取回数据）。
- GROUPBY子句中列出的每个列都必须是检索列或有效的表达式（但不能是聚集函数）。如果在SELECT中使用表达式，则必须在GROUPBY子句中指定相同的表达式。不能使用别名。
- 除聚集计算语句外，SELECT语句中的每个列都必须在GROUP BY子句中给出。
- 如果分组列中具有NULL值，则NULL将作为一个分组返回。如果列中有多行NULL值，它们将分为一组。
- GROUP BY子句必须出现在WHERE子句之后，ORDERBY子句之前。

#### 过滤分组

 WHERE 过滤指定的是行而不是分组。事实上， WHERE 没有分组的概念。

**WHERE 过滤行，而 HAVING 过滤分组** 所有类型的 WHERE 子句都可以用 HAVING 来替代

`SELECT cuSt_id,COUNT(*)AS orders FROM orders GROUP BY cust_id HAVING COUNT(*)>=2;`

分析这条SELECT语句的前3行类似于上面的语句。最后一行增加了HAVING子句，它过滤COUNT（*）>=2（两个以上的订单）的那些分组。

正如所见，这里 WHERE 子句不起作用，因为过滤是基于分组聚集值而不是特定行值的

![](https://imgconvert.csdnimg.cn/aHR0cDovL3d3MS5zaW5haW1nLmNuL2xhcmdlL2UwNjlmNjBlbHkxZndpZjFybGRwcWoyMGtwMDVxZ21pLmpwZw)

