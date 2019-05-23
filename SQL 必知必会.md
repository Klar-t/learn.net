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

      ```
      
      ```

    - 

