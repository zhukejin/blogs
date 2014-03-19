TIME_FORMATMYSQL 时间函数大全
---


- __DAYOFWEEK(date)__

>返回日期date的星期索引(1=星期天，2=星期一, ……7=星期六)。这些索引值对应于ODBC标准。
>
	mysql> select DAYOFWEEK(2007-10-31);
	-> 4 



- __WEEKDAY(date)__ 

>返回date的星期索引(0=星期一，1=星期二, ……6= 星期天)。  
>
	mysql> select WEEKDAY('2007-10-31 13:05:00');  
	-> 2
	mysql> select WEEKDAY('2007-10-31');
	-> 2 

- __DAYOFMONTH(date)__  

>返回date的月份中日期，在1到31范围内。  
>
	mysql> select DAYOFMONTH('2007-10-31');  
	-> 31

- __DAYOFYEAR(date)__  

>返回date在一年中的日数, 在1到366范围内。  
>
	mysql> select DAYOFYEAR('2007-10-31');  
	-> 304 


- __MONTH(date)__  

>返回date的月份，范围1到12。  
>
	mysql> select MONTH('2007-10-31');  
	-> 10

- __DAYNAME(date)__  

>返回date的星期名字。  
>
	mysql> select DAYNAME("2007-10-31");  
	-> 'Wednesday' 

- __MONTHNAME(date)__
  
>返回date的月份名字。 
> 
	mysql> select MONTHNAME("2007-10-31");  
	-> 'October' 

- __QUARTER(date)__ 

>返回date一年中的季度，范围1到4。  
>
	mysql> select QUARTER('2007-10-31');  
	-> 4 

- __WEEK(date)__
 
>  
	WEEK(date,first)  
>对于星期天是一周的第一天的地方，有一个单个参数，返回date的周数，范围在0到52。2个参数形式WEEK()允许你指定星期是否开始于星期天或星期一。如果第二个参数是0，星期从星期天开始，如果第二个参数是1，从星期一开始。
>  
	mysql> select WEEK('1998-02-20');  
	-> 7  
	mysql> select WEEK('1998-02-20',0);  
	-> 7  
	mysql> select WEEK('1998-02-20',1);  
	-> 8 

- __YEAR(date)__

>返回date的年份，范围在1000到9999。  
>
	mysql> select YEAR('98-02-03');  
	-> 1998 

- __HOUR(time)__

>返回time的小时，范围是0到23。  
>	
	mysql> select HOUR('10:05:03');  
	-> 10 

- __MINUTE(time)__

>返回time的分钟，范围是0到59。  
>
	mysql> select MINUTE('98-02-03 10:05:03');  
	-> 5 

- __SECOND(time)__ 

>回来time的秒数，范围是0到59。  
>
	mysql> select SECOND('10:05:03');  
	-> 3 

- __PERIOD\_ADD(P,N)__

>增加N个月到阶段P（以格式YYMM或YYYYMM)。以格式YYYYMM返回值。注意阶段参数P不是日期值。  
>
	mysql> select PERIOD_ADD(9801,2);  
	-> 199803 

- __PERIOD\_DIFF(P1,P2)__

>返回在时期P1和P2之间月数，P1和P2应该以格式YYMM或YYYYMM。注意，时期参数P1和P2不是日期值。  
>
	mysql> select PERIOD_DIFF(9802,199703);  
	-> 11 

- __DATE\_ADD(date,INTERVAL expr type)__
- __DATE\_SUB(date,INTERVAL expr type)__
- __ADDDATE(date,INTERVAL expr type)__

>
	SUBDATE(date,INTERVAL expr type)  
>这些功能执行日期运算。对于 `MySQL 3.22`，他们是新的。`ADDDATE()`和`SUBDATE()`是`DATE_ADD()`和`DATE_SUB()`的同义词.  
>在`MySQL 3.23`中，你可以使用`+`和`-`而不是`DATE_ADD()`和`DATE_SUB()`。（见例子）  
>`date`是一个指定开始日期的`DATETIME`或`DATE`值，`expr`是指定加到开始日期或从开始日期减去的间隔值一个表达式，`expr`是一个字符串；  
>它可以以 一个 `-` 开始表示负间隔。__type__是一个关键词，指明表达式应该如何被解释。
>`EXTRACT(type FROM date)`函数从日期中返回`type`间隔。下表显示了`type`和`expr`参数怎样被关联：
>
	type值 含义 期望的expr格式  
	SECOND 秒 SECONDS  
	MINUTE 分钟 MINUTES  
	HOUR 时间 HOURS  
	DAY 天 DAYS  
	MONTH 月 MONTHS  
	YEAR 年 YEARS  
	MINUTE_SECOND 分钟和秒 "MINUTES:SECONDS"  
	HOUR_MINUTE 小时和分钟 "HOURS:MINUTES"  
	DAY_HOUR 天和小时 "DAYS HOURS"  
	YEAR_MONTH 年和月 "YEARS-MONTHS"  
	HOUR_SECOND 小时, 分钟， "HOURS:MINUTES:SECONDS"  
	DAY_MINUTE 天, 小时, 分钟 "DAYS HOURS:MINUTES"  
	DAY_SECOND 天, 小时, 分钟, 秒 "DAYS HOURS:MINUTES:SECONDS" 

>`MySQL`在`expr`格式中允许任何标点分隔符。表示显示的是建议的分隔符。如果`date`参数是一个`DATE`值并且你的计算仅仅包含`YEAR`、`MONTH`和`DAY`部分(即，没有时间部分)，结果是一个`DATE`值。否则结果是一个`DATETIME`值。 
>
	mysql> SELECT "1997-12-31 23:59:59" + INTERVAL 1 SECOND;  
	-> 1998-01-01 00:00:00  
	mysql> SELECT INTERVAL 1 DAY + "1997-12-31";  
	-> 1998-01-01  
	mysql> SELECT "1998-01-01" - INTERVAL 1 SECOND;  
	-> 1997-12-31 23:59:59  
	mysql> SELECT DATE_ADD("1997-12-31 23:59:59",  
	INTERVAL 1 SECOND);  
	-> 1998-01-01 00:00:00  
	mysql> SELECT DATE_ADD("1997-12-31 23:59:59",  
	INTERVAL 1 DAY);  
	-> 1998-01-01 23:59:59  
	mysql> SELECT DATE_ADD("1997-12-31 23:59:59",  
	INTERVAL "1:1" MINUTE_SECOND);  
	-> 1998-01-01 00:01:00  
	mysql> SELECT DATE_SUB("1998-01-01 00:00:00",  
	INTERVAL "1 1:1:1" DAY_SECOND);  
	-> 1997-12-30 22:58:59  
	mysql> SELECT DATE_ADD("1998-01-01 00:00:00",  
	INTERVAL "-1 10" DAY_HOUR);  
	-> 1997-12-30 14:00:00  
	mysql> SELECT DATE_SUB("1998-01-02", INTERVAL 31 DAY);  
	-> 1997-12-02  
	mysql> SELECT EXTRACT(YEAR FROM "1999-07-02");  
	-> 1999  
	mysql> SELECT EXTRACT(YEAR_MONTH FROM "1999-07-02 01:02:03");  
	-> 199907  
	mysql> SELECT EXTRACT(DAY_MINUTE FROM "1999-07-02 01:02:03");  
	-> 20102 
>如果你指定太短的间隔值(不包括`type`关键词期望的间隔部分)，`MySQL`假设你省掉了间隔值的最左面部分。例如，如果你指定一个`type`是`DAY_SECOND`，值`expr`被希望有天、小时、分钟和秒部分。如果你象`"1:10"`这样指定值，MySQL假设日子和小时部分是丢失的并且值代表分钟和秒。换句话说，`"1:10"` `DAY_SECOND`以它等价于`"1:10"` `MINUTE_SECOND`的方式解释，这对那MySQL解释`TIME`值表示经过的时间而非作为一天的时间的方式有二义性。如果你使用确实不正确的日期，结果是`NULL`。如果你增加`MONTH`、`YEAR_MONTH`或`YEAR`并且结果日期大于新月份的最大值天数，日子在新月用最大的天调整。 
>
	mysql> select DATE_ADD('1998-01-30', Interval 1 month);  
	-> 1998-02-28 
>注意，从前面的例子中词INTERVAL和type关键词不是区分大小写的。 

- __TO\_DAYS(date)__

>给出一个日期date，返回一个天数(从0年的天数)。  
>
	mysql> select TO_DAYS(950501);  
	-> 728779  
	mysql> select TO_DAYS('1997-10-07');  
	-> 729669 

- __TO\_DAYS()__

>不打算用于使用格列高里历(1582)出现前的值。 

- __FROM\_DAYS(N)__

>给出一个天数N，返回一个DATE值。
>  
	mysql> select FROM_DAYS(729669);  
	-> '1997-10-07' 

- __DATE\_FORMAT(date,format)__

>根据format字符串格式化date值。下列修饰符可以被用在format字符串中：
>
	%M 月名字(January……December)  
	%W 星期名字(Sunday……Saturday)  
	%D 有英语前缀的月份的日期(1st, 2nd, 3rd, 等等。）  
	%Y 年, 数字, 4 位  
	%y 年, 数字, 2 位  
	%a 缩写的星期名字(Sun……Sat)  
	%d 月份中的天数, 数字(00……31)  
	%e 月份中的天数, 数字(0……31)  
	%m 月, 数字(01……12)  
	%c 月, 数字(1……12)  
	%b 缩写的月份名字(Jan……Dec)  
	%j 一年中的天数(001……366)  
	%H 小时(00……23)  
	%k 小时(0……23)  
	%h 小时(01……12)  
	%I 小时(01……12)  
	%l 小时(1……12)  
	%i 分钟, 数字(00……59)  
	%r 时间,12 小时(hh:mm:ss [AP]M)  
	%T 时间,24 小时(hh:mm:ss)  
	%S 秒(00……59)  
	%s 秒(00……59)  
	%p AM或PM  
	%w 一个星期中的天数(0=Sunday ……6=Saturday ）  
	%U 星期(0……52), 这里星期天是星期的第一天  
	%u 星期(0……52), 这里星期一是星期的第一天  
	%% 一个文字“%”。 

>所有的其他字符不做解释被复制到结果中。 
>
	mysql> select DATE_FORMAT('1997-10-04 22:23:00', '%W %M %Y');  
	-> 'Saturday October 1997'  
	mysql> select DATE_FORMAT('1997-10-04 22:23:00', '%H:%i:%s');  
	-> '22:23:00'  
	mysql> select DATE_FORMAT('1997-10-04 22:23:00',  
	'%D %y %a %d %m %b %j');  
	-> '4th 97 Sat 04 10 Oct 277'  
	mysql> select DATE_FORMAT('1997-10-04 22:23:00',  
	'%H %k %I %r %T %S %w');  
	-> '22 22 10 10:23:00 PM 22:23:00 00 6'
>MySQL3.23中，在格式修饰符字符前需要%。在MySQL更早的版本中，%是可选的。 

- __TIME\_FORMAT(time,format)__

>这象上面的DATE_FORMAT()函数一样使用，但是format字符串只能包含处理小时、分钟和秒的那些格式修饰符。其他修饰符产生一个NULL值或0。 

- __CURDATE()__
- __CURRENT\_DATE__

>以`'YYYY-MM-DD'`或`YYYYMMDD`格式返回今天日期值，取决于函数是在一个字符串还是数字上下文被使用。 
> 
	mysql> select CURDATE();  
	-> '1997-12-15'  
	mysql> select CURDATE() + 0;  
	-> 19971215 

- __CURTIME()__  
- __CURRENT\_TIME__

>以`'HH:MM:SS'`或`HHMMSS`格式返回当前时间值，取决于函数是在一个字符串还是在数字的上下文被使用。  
>
	mysql> select CURTIME();  
	-> '23:50:26'  
	mysql> select CURTIME() + 0;  
	-> 235026 

- __NOW()__
- __SYSDATE()__
- __CURRENT\_TIMESTAMP__

>以`'YYYY-MM-DD HH:MM:SS'`或`YYYYMMDDHHMMSS`格式返回当前的日期和时间，取决于函数是在一个字符串还是在数字的上下文被使用.
>
	mysql> select NOW();  
	-> '1997-12-15 23:50:26'  
	mysql> select NOW() + 0;  
	-> 19971215235026 

- __UNIX\_TIMESTAMP()__
- __UNIX\_TIMESTAMP(date)__

>如果没有参数调用，返回一个`Unix`时间戳记(从`1970-01-01 00:00:00'GMT`开始的秒数)。如果`UNIX_TIMESTAMP()`用一个`date`参数被调用，它返回从`'1970-01-01 00:00:00' GMT`开始的秒数值。`date`可以是一个`DATE`字符串、一个`DATETIME`字符串、一个`TIMESTAMP`或以`YYMMDD`或`YYYYMMDD`格式的本地时间的一个数字。  
>
	mysql> select UNIX_TIMESTAMP();  
	-> 882226357  
	mysql> select UNIX_TIMESTAMP('1997-10-04 22:23:00');  
	-> 875996580  
>当`UNIX_TIMESTAMP`被用于一个`TIMESTAMP`列，函数将直接接受值，没有隐含的`“string-to-unix-timestamp”`变换。 

- __FROM\_UNIXTIME(unix\_timestamp)__

>以`'YYYY-MM-DD HH:MM:SS'`或`YYYYMMDDHHMMSS`格式返回`unix_timestamp`参数所表示的值，取决于函数是在一个字符串还是或数字上下文中被使用。
>  
	mysql> select FROM_UNIXTIME(875996580);  
	-> '1997-10-04 22:23:00'  
	mysql> select FROM_UNIXTIME(875996580) + 0;  
	-> 19971004222300 

- __FROM\_UNIXTIME(unix\_timestamp,format)__

>返回表示 Unix 时间标记的一个字符串，根据`format`字符串格式化。`format`可以包含与`DATE_FORMAT()`函数列出的条目同样的修饰符。
>  
	mysql> select FROM_UNIXTIME(UNIX_TIMESTAMP(),  
	'%Y %D %M %h:%i:%s %x');  
	-> '1997 23rd December 03:43:30 x' 

- __SEC\_TO\_TIME(seconds)__

>返回`seconds`参数，变换成小时、分钟和秒，值以`'HH:MM:SS'`或`HHMMSS`格式化，取决于函数是在一个字符串还是在数字上下文中被使用。  
>
	mysql> select SEC_TO_TIME(2378);  
	-> '00:39:38'  
	mysql> select SEC_TO_TIME(2378) + 0;  
	-> 3938 

- __TIME\_TO\_SEC(time)__

>返回time参数，转换成秒。  
>
	mysql> select TIME_TO_SEC('22:23:00');  
	-> 80580  
	mysql> select TIME_TO_SEC('00:39:38');  
	-> 2378
