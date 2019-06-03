<center><h1>SQL 必知必会</h1></center>

- 函数  （大多数SQL实现支持以下类型的函数）
  - 用于处理文本字串（如删除或充值，转换值为大写或小写）的文本函数
    - Left() 返回串左边的字符
    - Length() 返回串的长度
    - Locate() 找出串的一个子串
    - Lower() 将串转换为小写
    - LTrim() 去掉串左边的空格
    - Right() 返回串右边的字符
    - RTrim() 去掉串右边的空格
    - Soundex() 返回串的SOUNDEX值 （SOUNDEX是一个将任何文
      本串转换为描述其语音表示的字母数字模式的算法）
    - SubString() 返回子串的字符
    - Upper() 将串转换为大写
  - 用于在数值数据上进行算数操作（如返回绝对值，进行代数运算）的数值函数
    - Abs() 返回一个数的绝对值
    - Cos() 返回一个角度的余弦
    - Exp() 返回一个数的指数值
    - Mod() 返回除操作的余数
    - Pi() 返回圆周率
    - Rand() 返回一个随机数
    - Sin() 返回一个角度的正弦
    - Sqrt() 返回一个数的平方根
    - Tan() 返回一个角度的正切
  - 用于处理日期和时间值并且从这些值中提取特定成分（例如，返回两个日期之差，检查日期有效性等）的日期和时间函数
    - AddDate() 增加一个日期（天、周等）
    - AddTime() 增加一个时间（时、分等）
    - CurDate() 返回当前日期
    - CurTime() 返回当前时间
    - Date() 返回日期时间的日期部分
    - DateDiff() 计算两个日期之差
    - Date_Add() 高度灵活的日期运算函数
    - Date_Format() 返回一个格式化的日期或时间串
    - Day() 返回一个日期的天数部分
    - DayOfWeek() 对于一个日期，返回对应的星期几
    - Hour() 返回一个时间的小时部分
    - Minute() 返回一个时间的分钟部分
    - Month() 返回一个日期的月份部分
    - Now() 返回当前日期和时间
    - Second() 返回一个时间的秒部分
    - Time() 返回一个日期时间的时间部分
    - Year() 返回一个日期的年份部分
  - 返回DBMS正使用的特殊信息（如繁花用户登录信息，检查版本细节）的系统函数

- 汇总数据

  - 聚集函数
    - AVG() 返回某列的平均值
    - COUNT() 返回某列的行数
    - MAX() 返回某列的最大值
    - MIN() 返回某列的最小值
    - SUM() 返回某列值之和

  - 组合聚集函数
    - 

  - 数据分组
    - group by 

- 组合查询

  - Union
    - union 必须由两条或两条以上的select语句组成，语句之间用关键字union分隔（因此，如果组合4条select语句，将要使用3个union关键字）
    - union中每个查询必须包含相同的列、表达式或者聚集函数（不过各个列不需要以相同的次序列出）
    - 列数据类型必须兼容：类型不必完全相同，但必须是DBMS可以隐性转换的类型（例如：不同的数值类型或者不同的日期类型）
    - 使用union会自动取消重复的行数据，如果使用union all就可以不取消重复的行数据；
  - 对组合查询结果排序
    - 在用union组合查询时，只能使用一条order by子句，他必须出现在最后一条select语句之后，对于结果集，不存在用一种方式配需一部分，而游泳另一种方式配需另一部分的情况，因此不允许使用多条order by子句；

- 视图

  - 创建视图

    - ```sql
      create view view_table as 
      select * From XXX
      ```

- 存储过程

  - 创建存储过程

    - ```sql
      CREATE PROCEDURE MailingListCount (
      ListCount OUT INTEGER
      )
      IS
      v_rows INTEGER;
      BEGIN
      SELECT COUNT(*) INTO v_rows FROM Customers
      WHERE NOT cust_email IS NULL;
      ListCount := v_rows;
      END;
      ```

    - 

- 事务处理

  - 事务（transaction）指一组SQL语句
  - 回退（rollback）指撤销指定SQL语句的过程
  - 提交（commit）指将未存储的SQL语句结果写入数据库表
  - 保留点（savepoint）指事务处理中设置的临时占位符（placeholder），可以对它发布回退（与回退整个事务处理不同）

  - 使用rollback

    - ```sql
      delete from orders
      rollback;
      ```

  - 使用commit

    - ```sql
      begin transaction
      delete orderitems where order_num=1234
      delete orders where order_num=1234
      commit transaction
      --一般的SQL语句都是针对数据库表直接执行和编写的，这就是所谓的隐式提交（implicit commit），即提交（写或保存）操作室自动进行的。在事务处理快中，体积哦啊不会隐式进行。不过，不同的DBMS的做法有所不同。有点DBMS按隐式提交处理事务端，有的则不这样。进行明确的提交，使用commit语句。
      ```

  - 使用保留点

    - 使用简单的rollback和commit语句，就可以写入或撤销整个事务。但是，只对简单的事务才能这样，复杂的事务需要部分提交或回退

      ``` sql
      --使用savepoint语句创建保留点
      davepoint delete1;--oracle
      save transaction delete1 --SQL server
      
      --使用rollback回退到保留点
      rollback to delete1 --oracle
      rollback transaction delete1 --SQL server
      
      --一条完整的SQL Server例子
      BEGIN TRANSACTION
      INSERT INTO Customers(cust_id, cust_name)
      VALUES('1000000010', 'Toys Emporium');
      SAVE TRANSACTION StartOrder;
      INSERT INTO Orders(order_num, order_date, cust_id)
      VALUES(20100,'2001/12/1','1000000010');
      IF @@ERROR <> 0 ROLLBACK TRANSACTION StartOrder;
      INSERT INTO OrderItems(order_num, order_item, prod_id, quantity, item_price)
      VALUES(20100, 1, 'BR01', 100, 5.49);
      IF @@ERROR <> 0 ROLLBACK TRANSACTION StartOrder;
      INSERT INTO OrderItems(order_num, order_item, prod_id, quantity, item_price)
      VALUES(20100, 2, 'BR03', 100, 10.99);
      IF @@ERROR <> 0 ROLLBACK TRANSACTION StartOrder;
      COMMIT TRANSACTION
      
      这里的事务处理块中包含了4条insert语句。在第一条insert语句之后定义了一个保留点，因此如果后面的任何一个 insert操作失败，事务处理最近回退到这里。在SQL Server中，可检测一个名为@@error的变量，看操作是否成功。（其他DBMS使用不同的函数或变量返回此信息）如果@@error返回一个非0值，表示有错误发生，事务处理回退到保留点。如果整个事务处理成功，发布commit以保留数据。
      --保留点越多越好  如果在SQL代码中设置任意多的保留点，就可以灵活的进行回退
      ```

- 使用游标

  - 

- 约束

  - 主键 一种唯一的约束 保证一列（或一组列）中的值是唯一的，而且永不改动。

  - 主键满足条件

    - 任意两行的主键值都不相同。

    - 每行都具有一个主键值（即劣种不允许null值）

    - 包含主键值的列从不修改或更新（大部分DBMS不允许这么做）

    - 主键值不能重用。如果从表中删除莫一行，器煮简直不分牌非新行

    - ```sql
      CREATE TABLE Vendors (
      vend_id CHAR(10) NOT NULL PRIMARY KEY,
      vend_name CHAR(50) NOT NULL, 
      );
      
      --
      ALTER TABLE Vendors ADD CONSTRAINT PRIMARY KEY (vend_id);
      ```

    - 

- 检查约束

  - 检查约束用来保证一列（或一组列）中的数据满足一组指定的条件。

  - 检查约束的常见用途

    - 检查最小或最大值。例如：防止0 个物品的订单（即使0是合法的数）

    - 指定范围。例如：保证发货日期大于今天的日期，但超过今天起一年后的日期

    - 只允许特定的值。例如：在性别字段中只允许M或F。

    - ```SQL
      CREATE TABLE OrderItems ( order_num INTEGER NOT NULL, order_item INTEGER NOT NULL,
      prod_id CHAR(10) NOT NULL, quantity INTEGER NOT NULL CHECK (quantity > 0), item_price MONEY NOT NULL
      );
      --利用这个约束，任何插入（或更新）的行都会被检查，保证quantity大于0
      
      --检查名为gender的列只包含M或F，可编写如下的alter table语句
      add constranint check（gender like'[MF]'）
      ```

- 索引

  - 索引改善检索操作的性能，但降低了数据插入、修改和删除的性能。在执行这些操作室，DBMS必须动态的更新索引

  - 索引数据可能要占用大量的存储空间。

  - 并非所有数据都适合索引。取值不多的数据（如州）不如具有更多可能值的数据（如 姓，名），能通过索引得到那么多的好处

  - 索引用于数据过滤和数据排序。如果你经常以魔种特定的顺序排序数据，则该数据可能适合做索引。

  - 可以再索引中定义多个列（例如，州加上城市）。这样的索引仅在以州加城市的顺序排序时有用。如果按城市排序，则这种索引没有用处

  - ```sql
    create index prod_name_ind
    on products(paod_name)
    --索引必须唯一命名。这里的索引名prod_name_ind 在关键字create index之后定义。on用来指定被索引的表，而索引中包含的列在表名后的括号中给出
    ```

- 触发器

  - 触发器是特殊的存储过程，他在特定的数据库活动发生时自动执行。夫发起可以再特定表上insert、update和delete操作（或组合）相关联。
  - 触发器常见用途
    - 保证数据一致。例如，在INSERT或UPDATE操作中将所有州名转换为大写。
    - 基于某个表的变动在其他表上执行活动。例如，每当更新或删除一行时将审计跟踪记录写入某个日志表。
    - 进行额外的验证并根据需要回退数据。例如，保证某个顾客的可用资金不超限定，如果已经超出，则阻塞插入。
    - 计算计算列的值或更新时间戳。

  ```sql
  CREATE TRIGGER customer_state
  ON Customers
  FOR INSERT, UPDATE
  AS
  UPDATE Customers
  SET cust_state = Upper(cust_state)
  WHERE Customers.cust_id = inserted.cust_id;
  --约束比触发器更快
  ```

- 数据库安全

  - 安全性使用SQL的GRANT和REVOKE语句来管理，不过，大多数DBMS提供了交互式的管理实用程序，这些实用程序在内部使
    用GRANT和REVOKE语句。

[参考](<https://www.cnblogs.com/sunshinewang/p/6789419.html>)

