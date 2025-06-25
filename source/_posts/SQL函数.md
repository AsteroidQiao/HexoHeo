---
title: 🍟SQL函数
date: 2025-06-25 15:19:48
categories: 开发
tags:
  - mysql
  - oracle
  - sql
cover: https://gitee.com/AsteroidQiao/library-management-system/raw/master/book-avatar/17508367405321750836740174.png
---
在公司写SQL的一个查询才发现Oracle的时间函数和SQL或者说MySQL不一致，oracle用的是extract提取，SQL可以用CURDATE()，oracle不行

##  Oracle 和 MySQL 中常用函数的详细对比

涵盖字符串、数值、日期、聚合、条件判断等多个类别，并附具体示例及差异说明：

### **一、字符串函数**

| **功能**       | **Oracle 函数**                                 | **MySQL 函数**                                | **示例与差异**                                               |
| -------------- | :---------------------------------------------- | --------------------------------------------- | ------------------------------------------------------------ |
| **拼接字符串** | `CONCAT(str1, str2)`                            | `CONCAT(str1, str2)`                          | 两者功能相同，但 Oracle 11g 后支持多参数，MySQL 需用 `CONCAT_WS(sep, str1, str2)` 带分隔符合并。 |
| **截取子串**   | `SUBSTR(str, start, length)`                    | `SUBSTRING(str, start, length)`               | 用法相似，Oracle 中 `start` 从 1 开始，MySQL 支持负数（从末尾计数）。例：`SUBSTR('hello', 2, 3)` → 'ell'。 |
| **字符串长度** | `LENGTH(str)`                                   | `LENGTH(str)` / `CHAR_LENGTH(str)`            | `LENGTH` 返回字节数，`CHAR_LENGTH` 返回字符数（处理 Unicode 更准确）。 |
| **大小写转换** | `UPPER(str)` / `LOWER(str)`                     | `UPPER(str)` / `LOWER(str)`                   | 功能一致，如 `UPPER('abc')` → 'ABC'。                        |
| **去空格**     | `TRIM([leading/trailing/both] [char] FROM str)` | `TRIM([char] FROM str)`                       | Oracle 支持指定去除方向（`leading`/`trailing`/`both`），MySQL 默认去除两端空格。 |
| **替换字符**   | `REPLACE(str, search, replace)`                 | `REPLACE(str, search, replace)`               | 功能相同，如 `REPLACE('abcabc', 'a', 'x')` → 'xbcxbc'。      |
| **填充字符串** | `LPAD(str, len, pad)` / `RPAD(str, len, pad)`   | `LPAD(str, len, pad)` / `RPAD(str, len, pad)` | 功能一致，例：`LPAD('123', 5, '0')` → '00123'。              |
| **字符串拆分** | `REGEXP_SUBSTR` / `SPLIT_TO_TABLE`（需插件）    | `SUBSTRING_INDEX` / 自定义函数                | MySQL 常用 `SUBSTRING_INDEX('a,b,c', ',', 2)` → 'a,b'；Oracle 需正则或第三方工具。 |

### **二、数值函数**

| **功能**     | **Oracle 函数**                    | **MySQL 函数**                          | **示例与差异**                                               |
| ------------ | ---------------------------------- | --------------------------------------- | ------------------------------------------------------------ |
| **四舍五入** | `ROUND(num, decimals)`             | `ROUND(num, decimals)`                  | 功能一致，`ROUND(3.1415, 2)` → 3.14。                        |
| **取整**     | `FLOOR(num)` / `CEIL(num)`         | `FLOOR(num)` / `CEIL(num)`              | 分别返回向下 / 向上取整，如 `FLOOR(3.9)` → 3，`CEIL(3.1)` → 4。 |
| **绝对值**   | `ABS(num)`                         | `ABS(num)`                              | 功能一致，`ABS(-5)` → 5。                                    |
| **幂运算**   | `POWER(num, power)` / `NUM**power` | `POWER(num, power)` / `POW(num, power)` | 功能一致，`POWER(2, 3)` → 8。                                |
| **随机数**   | `DBMS_RANDOM.VALUE(min, max)`      | `RAND()` / `RAND(min, max)`             | Oracle 需调用包，MySQL 直接生成 0-1 随机数，或用 `FLOOR(RAND()*(max-min+1)+min)` 生成指定范围。 |
| **取模运算** | `MOD(num1, num2)`                  | `MOD(num1, num2)` / `num1 % num2`       | 功能一致，`MOD(7, 3)` → 1。                                  |
| **三角函数** | `SIN()`, `COS()`, `TAN()`          | `SIN()`, `COS()`, `TAN()`               | 功能一致，但 Oracle 支持更多数学函数（如 `ASIN`, `ACOS` 等）。 |

### **三、日期函数**

| **功能**         | **Oracle 函数**                | **MySQL 函数**                           | **示例与差异**                                               |
| ---------------- | ------------------------------ | ---------------------------------------- | ------------------------------------------------------------ |
| **获取当前日期** | `SYSDATE`                      | `CURDATE()` / `NOW()`                    | `SYSDATE` 返回日期时间，`CURDATE()` 仅返回日期，`NOW()` 返回日期时间。例：`SELECT SYSDATE FROM dual;` → `2025-06-25 14:30:00`。 |
| **日期格式化**   | `TO_CHAR(date, '格式')`        | `DATE_FORMAT(date, '格式')`              | 格式符不同，Oracle 用 `'YYYY-MM-DD HH24:MI:SS'`，MySQL 用 `'%Y-%m-%d %H:%i:%s'`。例：`TO_CHAR(SYSDATE, 'YYYY-MM-DD')` → `2025-06-25`。 |
| **日期计算**     | `DATE + 数字`（加天数）        | `DATE_ADD(date, INTERVAL 数字 单位)`     | Oracle 直接用 `+` 加减天数，MySQL 需用 `DATE_ADD`/`DATE_SUB`。例：`SYSDATE + 7`（Oracle）等价于 `DATE_ADD(NOW(), INTERVAL 7 DAY)`（MySQL）。 |
| **提取日期部分** | `EXTRACT(part FROM date)`      | `YEAR(date)`, `MONTH(date)`, `DAY(date)` | Oracle 用 `EXTRACT(YEAR FROM date)`，MySQL 直接用函数。例：`EXTRACT(YEAR FROM SYSDATE)` → 2025。 |
| **日期差值**     | `MONTHS_BETWEEN(date1, date2)` | `DATEDIFF(date1, date2)`                 | Oracle 可计算月数差，MySQL `DATEDIFF` 计算天数差，月数差需用 `PERIOD_DIFF`（格式为 `YYYYMM`）。 |
| **日期转换**     | `TO_DATE(str, '格式')`         | `STR_TO_DATE(str, '格式')`               | 功能一致，将字符串转为日期，格式符规则不同（同格式化函数）。 |

### **四、聚合函数**

| **功能**            | **Oracle 函数**                             | **MySQL 函数**                              | **示例与差异**                                               |
| ------------------- | ------------------------------------------- | ------------------------------------------- | ------------------------------------------------------------ |
| **求和**            | `SUM(column)`                               | `SUM(column)`                               | 功能一致，忽略 `NULL` 值。                                   |
| **平均值**          | `AVG(column)`                               | `AVG(column)`                               | 功能一致，仅计算数值类型。                                   |
| **最大值 / 最小值** | `MAX(column)` / `MIN(column)`               | `MAX(column)` / `MIN(column)`               | 功能一致，可用于数值、日期、字符串类型。                     |
| **计数**            | `COUNT(column)` / `COUNT(*)`                | `COUNT(column)` / `COUNT(*)`                | 功能一致，`COUNT(*)` 统计行数，`COUNT(column)` 统计非 `NULL` 值数量。 |
| **字符串聚合**      | `LISTAGG(expr, sep) WITHIN GROUP`           | `GROUP_CONCAT(expr SEPARATOR sep)`          | Oracle 需指定排序（`WITHIN GROUP`），MySQL 用 `ORDER BY` 子句。 |
| **分组排序聚合**    | `FIRST_VALUE(expr) OVER (PARTITION BY ...)` | `FIRST_VALUE(expr) OVER (PARTITION BY ...)` | MySQL 8.0+ 支持窗口函数，与 Oracle 语法相似，低版本需用子查询。 |

### **五、条件函数**

| **功能**       | **Oracle 函数**                                         | **MySQL 函数**                                               | **示例与差异**                                               |
| -------------- | ------------------------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| **三目运算**   | `CASE WHEN condition THEN result ELSE else_result END`  | `CASE WHEN condition THEN result ELSE else_result END` / `IF(condition, result, else_result)` | 两者 `CASE` 语法一致，MySQL 额外支持 `IF` 函数（更简洁）。例：`IF(score >= 60, '及格', '不及格')`。 |
| **NULL 处理**  | `NVL(expr1, expr2)`                                     | `IFNULL(expr1, expr2)`                                       | 功能一致，`expr1` 为 `NULL` 时返回 `expr2`。                 |
| **NULL 转换**  | `NULLIF(expr1, expr2)`                                  | `NULLIF(expr1, expr2)`                                       | 功能一致，`expr1=expr2` 时返回 `NULL`，否则返回 `expr1`。    |
| **多条件判断** | `DECODE(expr, search1, result1, search2, result2, ...)` | 无直接等价，可用 `CASE` 替代                                 | Oracle 特有的简化 `CASE` 语法，例：`DECODE(grade, 'A', '优秀', 'B', '良好', '其他')`。 |

### **六、系统与流程函数**

| **功能**                 | **Oracle 函数**                | **MySQL 函数**                             | **示例与差异**                                               |
| ------------------------ | ------------------------------ | ------------------------------------------ | ------------------------------------------------------------ |
| **获取用户**             | `USER`                         | `USER()` / `CURRENT_USER()`                | 功能一致，返回当前登录用户。                                 |
| **执行动态 SQL**         | `EXECUTE IMMEDIATE sql_str`    | `PREPARE stmt FROM sql_str; EXECUTE stmt;` | Oracle 用 `EXECUTE IMMEDIATE`，MySQL 需预处理语句，语法差异较大。 |
| **流程控制（存储过程）** | `IF-THEN-ELSE`, `CASE`, `LOOP` | `IF-THEN-ELSE`, `CASE`, `LOOP`             | 语法类似，但细节（如循环条件）有差异，需单独学习。           |
| **序列生成**             | `SEQUENCE.NEXTVAL`             | `AUTO_INCREMENT` 字段                      | Oracle 用序列对象，MySQL 用表字段属性，例：`CREATE TABLE t(id INT AUTO_INCREMENT PRIMARY KEY);`。 |

### **七、正则表达式函数**

| **功能** | **Oracle 函数**                                  | **MySQL 函数**                                   | **示例与差异**                                               |
| -------- | ------------------------------------------------ | ------------------------------------------------ | ------------------------------------------------------------ |
| **匹配** | `REGEXP_LIKE(str, pattern)`                      | `REGEXP_LIKE(str, pattern)` / `REGEXP_MATCHES`   | MySQL 8.0+ 支持 `REGEXP_LIKE`，低版本用 `REGEXP`（等价于 `RLIKE`）。例：`REGEXP_LIKE('abc123', '^[a-z]+')` → TRUE。 |
| **替换** | `REGEXP_REPLACE(str, pattern, replace)`          | `REGEXP_REPLACE(str, pattern, replace)`          | 功能一致，例：`REGEXP_REPLACE('a1b2c3', '[0-9]', '#')` → 'a#b#c#'。 |
| **提取** | `REGEXP_SUBSTR(str, pattern, start, occurrence)` | `REGEXP_SUBSTR(str, pattern, start, occurrence)` | MySQL 8.0+ 支持，低版本需用复杂子查询。例：`REGEXP_SUBSTR('a1b2c3', '[0-9]', 1, 2)` → '2'。 |

### **八、特殊函数**

| 功能           | Oracle 函数                                                  | MySQL 函数                                            | 说明                                                         |
| -------------- | ------------------------------------------------------------ | ----------------------------------------------------- | ------------------------------------------------------------ |
| 分组合并字符串 | `LISTAGG(expr, delimiter) WITHIN GROUP (ORDER BY ...)`       | `GROUP_CONCAT(expr ORDER BY ... SEPARATOR delimiter)` | Oracle 需用 `WITHIN GROUP` 子句，MySQL 用 `SEPARATOR` 指定分隔符，语法更简洁。 |
| 排名函数       | `ROW_NUMBER()`, `RANK()`, `DENSE_RANK()`                     | `ROW_NUMBER()`, `RANK()`, `DENSE_RANK()`              | MySQL 8.0+ 支持窗口函数，功能与 Oracle 一致（早期版本不支持）。 |
| 分组排序       | `RANK() OVER (PARTITION BY col ORDER BY col)`                | `RANK() OVER (PARTITION BY col ORDER BY col)`         | 窗口函数语法一致，但 MySQL 需 8.0+ 版本。                    |
| 累计计算       | `SUM(expr) OVER (ORDER BY col ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW)` | 同上                                                  | 累计求和、平均等窗口函数，语法一致，MySQL 8.0+ 支持。        |


