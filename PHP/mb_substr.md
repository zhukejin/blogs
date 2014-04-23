PHP截取中文字符串时候出现乱码问题
--
因为英文字符串占用字符是1个，而中文UTF8 字符串占用的字符是 3 个，GB2312 是两个，所以当一个字符串中同时拥有英文字符+中文汉字的时候，截取就容易正好截到汉字一半，就出现了不完整的汉字，显示出来就是乱码。
以下这个方法可以解决。

PS：也可以用于Javascript （修改下里面使用的PHP内置函数）


	public static function chinesesubstr($str, $start, $len) { // $str指字符串,$start指字符串的起始位置，$len指字符串长度
        $strlen = $start + $len; // 用$strlen存储字符串的总长度，即从字符串的起始位置到字符串的总长度
        for($i = $start; $i < $strlen;) {
            if (ord ( substr ( $str, $i, 1 ) ) > 0xa0) { // 如果字符串中首个字节的ASCII序数值大于0xa0,则表示汉字
                $tmpstr .= substr ( $str, $i, 3 ); // 每次取出三位字符赋给变量$tmpstr，即等于一个汉字
                $i=$i+3; // 变量自加3
            } else{
                $tmpstr .= substr ( $str, $i, 1 ); // 如果不是汉字，则每次取出一位字符赋给变量$tmpstr
                $i++;
            }
        }
        return $tmpstr; // 返回字符串
    }

代码是从CSDN某一个博客里面挖过来的，忘记地址了，正好今天用到就顺便整理一下以前的笔记，放到线上。

