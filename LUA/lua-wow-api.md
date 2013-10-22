
 LUA函数 wow-API
-
	
 
WoW API中的Lua注意在WoWAPI没有提供所有标准的Lua函数, 很显然, 操作系统以及文件I/O库是不支持的   
  
**Lua函数这些函数都是Lua编程语言的一部分：** 
 
	•assert(value) - 检查一个值是否为非nil, 若不是则(如果在wow.exe打开调试命令)显示对话框以及输出错误调试信息   
	•collectgarbage() - 垃圾收集器. (新增于1.10.1)   
	•date(format, time) - 返回当前用户机器上的时间.   
	•error("error message",level) - 发生错误时,输出一条定义的错误信息.使用pcall() (见下面)捕捉错误.   
	•gcinfo() - 返回使用中插件内存的占用量(kb)以及当前垃圾收集器的使用量(kB).   
	•getfenv(function or integer) - 返回此表已获取函数的堆栈结构或者堆栈等级   
	•getmetatable(obj, mtable) - 获取当前的元表或者用户数据对象.   
	•loadstring("Lua code") - 分析字符串中的lua代码块并且将结果作为一个函数返回   
	•next(table, index) - 返回下一个key,一对表值.允许遍历整个表   
	•pcall(func, arg1, arg2, ...) - 受保护调用. 执行函数内容，同时捕获所有的异常和错误.   
	•select(index, list) - 返回选择此列表中的商品数值.或者是此件物品在列表中的索引值   
	setfenv(function or integer, table) - 设置此表已获取函数的堆栈结构或者堆栈等级   
	•setmetatable(obj, mtable) - 设置当前表的元表或者用户数据对象   
	•time(table) - 返回从一个unix时间值   
	•type(var) - 判断当前变量的类型, "number", "string", "table", "function" 或者 "userdata".   
	•unpack(table) - 解压一个表.返回当前表中的所有值.   
	•xpcall(func, err) - 返回一个布尔值指示成功执行的函数以及调用失败的错误信息.另外运行函数或者错误的返回值   


**数学函数这些表函数同样可以参考Lua函数库(他们都是以math.开头, 详情参阅数学库了解更多信息):**


 ----三角函数是没有参考的, 包括角度->弧度的转换.暴雪的版本使用角度.而Lua的标准数学库则使用的弧度.   
	
	 •abs(value) - 返回当前数值的绝对值   
	 •acos(value) - 返回此角度值的弧余弦值.   
	 •asin(value) - 返回此角度值的弧正弦值   
	 •atan(value) - 返回此角度值的弧正切值   
	 •atan2(y, x) - 在角度中, 返回Y/X的弧正切值.   
	 •ceil(value) - 返回一个整数, 不管小数点后面是多少, 整数部分都进1   
	 •cos(degrees) - 返回角度的余弦值   
	 •deg(radians) - 弧度转换角度   
	 •exp(value) - 返回此值的指数值   
	 •floor(value) - 返回此值的整数值   
	 •frexp(num) - 返回当前数字小数点后的数值以及小数点后的位数   
	 •ldexp(value, 倍数) - 输出此数值的倍数   
	 •log(value) - 返回此值的自然对数 (以e为底数)   
	 •log10(value) - 返回以10为底数的值   
	 •max(value[, values...]) - 在一堆值中查找最大值   
	 •min(value[,values...]) - 在一堆值中查找最小值   
	 •mod(value,modulus) - 返回此值的余数   
	 •rad(degrees) - 角度转换弧度   
	 •random([ [lower,] upper]) - 返回一个随机数字 (可选界限为整数值)   
	 •randomseed(seed) - 为伪随机数字生成器设定种子   
	 •sin(degrees) - 返回当前角度的正弦值   
	 •sqrt(value) - 返回数值的平方根值(比如100的平方根为10)   
	 •tan(degrees) - 返回当前角度的正切值   
  

**字符串库这些表函数同样可以参考Lua字符串库(他们都是以string.开头, 参考字符串库了解更多信息)**

	 •format(formatstring[, value[, ...]]) - 格式化字符串   
	 •gsub(string,pattern,replacement[, limitCount]) - 全局替换   
	 •strbyte(string[, index]) - 转换字符串为整数值(可以指定某个字符).   
	 •strchar(asciiCode[, ...]) - 转换整数为相对应的字符   
	 •strfind(string, pattern[, initpos[, plain]]) - 在一个指定的目标字符串中搜索指定的内容(第三个参数为索引),返回其具体位置.   
	 •strlen(string) - 返回当前字符串的字符数   
	 •strlower(string) - 将字符串的字母转换为小写格式   
	 •strmatch(string, pattern[, initpos]) - 与strfind不同的是,macth返回的是具体值,而find返回的是此值的位置   
	 •strrep(seed,count) - 返回一个字符串种子副本的个数   
	 •strsub(string, index[, endIndex]) - 返回字符串指定位置的值.   
	 •strupper(string) - 将字符串的字母转为大写格式   
	 •tonumber(arg[, base]) - 若参数能转为数字则返回一个数值.可以指定转换的类型.默认为十进制整数   
	 •tostring(arg) - 转换参数为字符串   

**下面的字符串函数是wow独有的**

	•strtrim(string) - 去除字符串前后空格   
	•strsplit(delimiter, string) - 分割字符串   
	•strjoin(delimiter, string, string[, ...]) - 根据定界符将字符串连接起来   


**表函数:**
这些表函数同样可以参考Lua表库 (他们都是以"table.开头", 查阅Table库 f了解更多信息).   
也知道, 很多表都被设计为只有数字索引, 开始于1并且没有结尾(如like {[1] = "foo", [3] = "bar"} --- [2]是不存在的所以为nil值).工作时，与任何其他表并没有差别，但很可能会产生意想不到的结果. 在编写lua中, 这个也就是通常所说的bug
  
	 •foreach(table,function) - 为表中的每个元素执行函数   
	 •foreachi(table,function) - 为表中的每个元素执行函数, 按顺序访问(不推荐, 可以使用ipairs代替)   
	 •getn(table) - 当作为一个聊表时, 返回当前表的大小. 现在已经废弃了, 你可以直接使用#符号.使用#(table)代替table.getn(table)   
	 •ipairs(table) - 返回一个迭代型整数遍历表。   
	 •pairs(table) - 返回一个迭代遍历表   
	 •sort(table[, comp]) - 对一个数组进行排序, 还可以指定一个可选的次序函数   
	 •tinsert(table[, pos], value) - 将一个元素插入指定的位置(默认在表的最后)   
	 •tremove(table[, pos]) - 删除指定位置的元素(默认表最后一个)   
   
 Bit Functions魔兽世界包含了Lua BitLib库(他们都是以 "bit."开头). 与C的位运算相同.位运算库并非Lua标准函数库的一部分, 但   是在WoW 1.9中得到了应用   


	1. •bit.bnot(a) - 返回一个a的补充   
	2. •bit.band(w1,...) - 返回w的位与   
	3. •bit.bor(w1,...) - 返回w的位或   
	4. •bit.bxor(w1,...) - 返回w的位异或   
	5. •bit.lshift(a,b) - 返回a向左偏移到b位   
	6. •bit.rshift(a,b) - 返回a逻辑右偏移到b位   
	7. •bit.arshift(a,b) - 返回a算术偏移到b位   
	8. •bit.mod(a,b) - 返回a除以b的整数余数   

Lua是一个脚本语言, 使用这些函数来对你的数据结构进行比对是十分慢的. 除非你有一个非常大的数据库以及需要保护的RAM使用, 保存你的信息到几个或个别的变量中.   
  
### 笔记: ###
所有的字符都有他们的元表设置到全局字符串表, 因此一些字符串函数可以直接写在冒号前面进行调用:   
	
	local s = string.format(input, arg1, arg2, ...)  
	  
	 -- 可以写成这样  
	local s = input:format(arg1, arg2, ...)  -- input将作为第一个参数, 复制到代码上, 这就是冒号语法 
	 
这同样可以使用在字符串上, `"%d":format(arg1)` 不是有效的lua代码,你可以写成   

	("%d"):format(arg1)  

自从使用字符串表, 一些函数放在字符串表里也是同样可以工作的, 下面的例子是有效的:   
	  
	function string.print(a)  
		return print(a)  
	end  
		("test"):print()  

