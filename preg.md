#### 正则表达式01

### ^ 和 $ 使用(行的起始符合和结束符)
**/^cat/**  ： 匹配以 c 作为一行的第一个字符，紧接着一个 a , 再紧接着 t 的文本
**/cat$/**  ：以cat 以cat 结束,后面没有任何字符
**/^cat$/** ：只包含cat，没有其他任何字符串
##### DEMO:
```php
preg_match('/^cat/','catabc',$arr);//cat
preg_match('/cat$/','cat',$arr);//cat
preg_match('/cat$/','catabc',$arr);//false
preg_match('/^cat$/','cat',$arr);//cat
preg_match('/^cat$/','catfk',$arr);//false
```

### 字符组

##### 匹配若干字符之一
如过，我们想匹配单词，如“grey”，同时又不确定它是否写作“gray”，就可以使用正则表达式结构体（construct）"[……]"。它容许使用者列出在某处期望匹配的字符
```
gr[ea]y": 先找到 g,跟着是一个r,然后是一个 a 或者 e,最后是一个y
preg_match('/ca[abc]t/','cact ',$arr);//cact
preg_match('/ca[abc]t/','cabt ',$arr);//cabt
preg_match('/ca[abc]t/','caabt ',$arr);//false
//即只能匹配结构体[]中的一个字符串，或的意思，但必须要有一个
preg_match('/c[abc]ty[aor]u/','catyou',$arr);
preg_match('/ca[0-9]t/','ca3t',$arr);//ca3t 
preg_match('/ca[\d]t/','ca6tbsdf',$arr);//ca6t
```
 在一个字符组中可以列举多个字符。如[123456],匹配 1 到 6 中的任意一个数字。在字符组内部，字符组元字符（character-class metacharacter）"-"(连字符)表示>一个范围。如[0-9],[a-z]等,表示匹 配0 到 9中的任意一个数字，a 到 z 中的任意一个小写字母。和一一列举表单的意思是一样的。“[0-9a-fA-F],[A-Fa-f0-9]”用来匹配>十六进制数字,顺序无所谓。除此外，可以随心所欲把字符范围与普通字符相结合,如“[0-9A-z_!.?]”，能够匹配一个数字，大写字母，下划线，惊叹号，点号，或者问号。
`
注：只有在字符组内部([])，“-” 连字符才是元字符，否则它只能匹配普通连字符号。实际，即使在字符组内部，若它出现在字符组的开头，它也不是元字符，只是一个普通
字符“—”，而不是表示一个范围。同样的道理，问号，和点号通常也被当做元字符，但是在字符粗内，则不是。
`
```php
preg_match('/abc[a-zA-Z0-9_]/','xxxx',$arr);
preg_match('/abc[a-z+()[]A-Z0-9_!.?*]/','xxxx',$arr);//在字符组中，这里的_!.?*是普通字符,-字符如果不是在开头，他就是元字符，如果在开头，则是普通字符，不是范围
```

##### 排除型字符组（文本匹配）
[^……]，这个字符组会匹配任何未列出的字符。如"[^1-6]"，匹配除1到6外的任何字符。字符组开头的"^"，表示排除，这里列出不希望匹配的字符。
`
注：排除型字符组，表示“匹配一个未列出的字符”，而不是 “不要匹配列出的字符”。两种说法看似一样，但略有差异。有一种简单的理解排除型字符组的办法，就是把>他们看做普通字符组，里面包含的是除了“排除型字符组中所有字符”以外的字符。
`
```php
preg_match('/abc[^d]/','abcx',$arr);//abcx
preg_match('/abc[^d]/','abcd',$arr);//false
preg_match('/abc[^0-9]/','abc234242',$arr);//false
preg_match('/abc[^dmnz]/','abcx',$arr);//abcx,即后面不能有^的某个字符
```

#####  用点号匹配任意字符
`
元字符 "." （dot 或 point) 是用来匹配任意字符的字符组的简便写法。如，‘03[-./]19[-./]76’ 匹配 '03/19/76,03-19-76,03.19.76'， 也可以简单尝试，'03.19.76'。
 `
```php
preg_match('/03.19.76/','abc num: 19 203319  7639',$arr);//203319  76
preg_match('/03[-./]19[-./]76/','abc num: 19 203319  7639',$arr);//这样写就只能匹配数字中间是-或.或/分隔的字符，更加精准，但可读性差，难写。
```

##### 匹配任意子表达式
 "|" 是一个非常简捷的元字符，它的意思是“或”。依靠它把不同的子表达式组合成一个总的表达式，而这个总的表达式，又能够匹配任意的子表达式。在这样的组合中，
子表达式称为“多选分支”。如："Bob" 和 "Robert" 是两个表达式， "Bob|Robert" 就是能头同时匹配其中任意一个正则表达式。如："gr[ae]y" 可以写成 "gr(a|e)y"。其>中括号是必须的。否则"gra|ey"，则是匹配 "gra" 或者 "ey"。多选结构可以包含多字符，但是不能跨越括号的界限。如"(fir|1)st.[Ss]treet"。

```php
preg_match('/bei(jing|abc)/','beijingchao',$arr);
preg_match('/bei[abc]/','beiabc',$arr);
//注意上面两个表达式都区别，第一个是将 jing或abc作为一个整体匹配，而第二个则是匹配单个字符a或者b或者c其中一个。

preg_match('/(Li|li)(gui|Gui)(bing|Bing)/','liguibing',$arr);//liguibing
preg_match('/(Li|li)g(ui|ui)b(ing|Ing)/','liguibing',$arr);//liguibing
//注：()内包含如 ^$.*是，是按元字符串处理，而[] 则是按普通字符串处理，(a|b|c)和[abc]表示的意思完全不一样
```
`
在一个包含多选结构的表达式中使用脱字符和美元符的时候要小心。例如 "^From|Subject|Date:." 和 "^(From|Subject|Date):.",前者由3个多选分支构成，所以能匹>配"^From"或"Subject"或"Date:.",后者匹配一行起始位置，然后匹配"From","Subject"或"Date"中的任意一个，然后匹配":."。
`

##### 忽略大小写
忽略大小写，这并非是正则表达式语言的一部分，而是许多工具语言提供的有用的特性。如egrep 的 "-i" 参数。

```
egrep -i '^(From|Subject|Date):' mailbox
preg_match('/^(From|Subject|Date)/i','str....',$arr);
preg_match('/^From:/i','From:ffstr....',$arr);
```

#####     单词分界符（位置匹配）
如果 egrep 支持“元字符序列”，如"\<","\>" 就可以用他们来匹配单词分界的位置。它相当于单词行锚点。用来匹配单词的开头和结束位置。如"\<cat\>" 匹配单词开>头位置，然后是c,a,t三个字母，然后是单词结束的位置。

    注："<"和">" 本身不是元字符，只有当它们和斜线结合的时候，整个序列才具有特殊意义。这就是称为“元字符序列”的原因。重要的是它的特殊意义，而不是字符个数>。

元字符小结

    .      点               单个任意字符
    []     字符组           列出的任意字符
    [^]    排除型字符组     为列出的任意字符
    ^      脱字符           行的起始位置
    $      美元符           行的结束位置
    \<     反斜线-小于      单词的起始位置
    \>     反斜线-大于      单词的结束位置

    |      竖线             匹配分隔两边的任意一个子表达式
    ()     括号             限制竖线的作用范围，等

#####  可选项元素

` ？      下限无,上限1 ,可以不出现，也可以只出现一次,出现0次或1次 `
```php
preg_match('/ABC?r/','ABr',$arr);//ABr
preg_match('/ABC?r/','ABCr',$arr);//ABCr
//匹配？前面的字符，前缀也可以是一个整体，如(abc)?即可以有abc，也可以没有
preg_match('/ABC(hello)?r/','ABCr',$arr);//ABCr
preg_match('/ABC(hello)?r/','ABChellor',$arr);//ABChellor
```

` *       下限无 ,上限无 ,可以出现无数次，也可以不出现（任意次数均可） ** `
```php
preg_match('/ABC*E/','ABCABCABCEF',$arr);//ABCE
preg_match('/ABC*E/','ABCABCABCEFEKL',$arr);//ABCE
```
    
` +       下限1,上限无,可以出现无数次，但至少要出现一次（至少一次） `
```php
preg_match('/AB(C)+r/','ABCCCCrrr',$arr);//ABCCCCr
preg_match('/ABC+E/','ABCABCEF',$arr);//ABCE
preg_match('/ABC+E/','ABCABCABCEF',$arr);//ABCE
preg_match('/<HR +SIZE *= *14 *>/','<HR SIZE=14>',$arr);//HR后面至少有一个空格，=左右可以有0个或多个空格，14及>左边也可以有0个或多个空格
```

#####  规定重复次数的范围：区间
`     使用元字符序列来自定义重复次数的区间"{min,max}"，称为区间量词。如 "{3,12}" 能够容许的重复次数在3到12之间。 `
```php
preg_match("/AB{0,2}C/","ABBCDEFDFd",$arr);//ABBC
preg_match("/AB{0,1}C/","ABCDEFDFd",$arr);//ABC
/**
? 区间  {0,1} 可以不出现，或者出现一次
* 区间  {0,} 可以出现无穷多次，也可以不出现
+ 区间  {1,}  可以出现无穷多次，但至少出现一次
*/
```

##### 括号的反向引用
 括号的两种用途：限制多选项的范围；将若干字符组合为一个单元，受问号或星号之类量词的作用。
 
  此外在许多流派的正则中，括号能够“记住” 它们包含的自表达式匹配的文本。“反向引用” 正则表达式特性之一，他允许我们匹配与表达式先前部分匹配的同样的文本。
如："\<([A-Za-z]+).+\1\>"，括号能够“记住”其中子表达式匹配的文本，不论文本内容是什么，元字符序列"\1"都能够记住它们。

"([a-z])([0-9])\1\2","\1" 代表"[a-z]"匹配的内容，"\2"代表"[0-9]"匹配的内容 


#####  神奇的转义
  "\." 转义的点号，这个元序列代表普通点号。这样的方法适用与所有元字符，不过在字符组内部无效。"\" 这样的反斜线称为“转义符”，它的作用是使被转义的元字符>失去特殊含义，成了普通字符。如："[a−zA−Z]+" 来匹配一个括号内的单词。若"\"后跟的不是元字符，根据语言工具版本而定。如"\<","\>","\1"被当做元字符序列对>待。

 \s 匹配所有“空白” 
     "\s" 表示所有表示“空白字符”的字符组,其中包括，空格符，制表符，换行符，和回车符。
```php
$input =~ m/^([+-]?[0-9]+(\.[0-9]*)?)\s*([CF])$/
preg_match("/abc\sef/",'abc ef',$arr);//abc ef
```

"/i" 称作“模式修饰符”，其作用是告诉perl的进行不区分大小写的匹配
```php
egrep -i '^(From|Subject|Date):' mailbox
preg_match('/^(From|Subject|Date)/i','str....',$arr);
```

"/g" 称作"全局替换"修饰符，在第一次替换完成后，继续搜索更多的匹配文本，进行更多的替换。如果需要检查的字符串包含多行需要替换的文本，每条替换规则都要>对所有行生效，就必须使用"/g";

  简记法:

    \t          制表符
    \n          换行符
    \r          回车符
    \s          任何“空白”字符（空格，制表，回车等）
    \S          除 “\s空白” 之外的任何字符
    \w          [a-zA-Z0-9] 大小写字母和数字，(\w+ 可以用来匹配一个单词)
    \W          除"\w 大小写字母和数字" 之外的任何字符([^a-zA-Z0-9])
    \d          [0-9],即数字
    \D          除"\d数字"之外的任何字符。（[^0-9]）
    \b          单词边界符


#####  常见元字符
| 元字符		|    名称  	|  功能 |
| :-------- | :-------- | :-----|
| : |
|  . | 	点	匹配单个任意字符 |  
| [...]	| 字符组	| 匹配单个字符组出现的字符| 
| [^...]| 	排除型字符组	| 匹配单个未在字符组出现的字符| 
| \char	| 转义符	| 如果char是元字符或没有特殊含义， 则匹配char。| 
| ?	| 问号	| 容许非必须的一次匹配 | 
| +	| 加号	| 匹配1次以上 | 
| *	| 星号	匹配任意次 | 
| {min,max}	| 区间量词	| 匹配min次到max次。 | 
| ^$	| 脱字符和美元符	| 匹配一行文本的开头和结束。 | 
| \b	| 边界符	| 匹配单词的边界 | 
| \| | 	\|	| 多选结构 | 
| (...)| 	括号 | 	用处有三，请看上面 | 
| \1.\2	| 反向引用	| 对括号捕捉到的分组进行引用。 | 

##### 常见正则表达式demo
	一、校验数字的表达式
	数字：^[0-9]*$
	n位的数字：^\d{n}$
	至少n位的数字：^\d{n,}$
	m-n位的数字：^\d{m,n}$
	零和非零开头的数字：^(0|[1-9][0-9]*)$
	非零开头的最多带两位小数的数字：^([1-9][0-9]*)+(.[0-9]{1,2})?$
	带1-2位小数的正数或负数：^(\-)?\d+(\.\d{1,2})?$
	正数、负数、和小数：^(\-|\+)?\d+(\.\d+)?$
	有两位小数的正实数：^[0-9]+(\.[0-9]{2})?$
	有1~3位小数的正实数：^[0-9]+(\.[0-9]{1,3})?$
	非零的正整数：^[1-9]\d*$ 或 ^([1-9][0-9]*){1,3}$ 或 ^\+?[1-9][0-9]*$
	非零的负整数：^\-[1-9][]0-9"*$ 或 ^-[1-9]\d*$
	非负整数：^\d+$ 或 ^[1-9]\d*|0$
	非正整数：^-[1-9]\d*|0$ 或 ^((-\d+)|(0+))$
	非负浮点数：^\d+(\.\d+)?$ 或 ^[1-9]\d*\.\d*|0\.\d*[1-9]\d*|0?\.0+|0$
	非正浮点数：^((-\d+(\.\d+)?)|(0+(\.0+)?))$ 或 ^(-([1-9]\d*\.\d*|0\.\d*[1-9]\d*))|0?\.0+|0$
	正浮点数：^[1-9]\d*\.\d*|0\.\d*[1-9]\d*$ 或 ^(([0-9]+\.[0-9]*[1-9][0-9]*)|([0-9]*[1-9][0-9]*\.[0-9]+)|([0-9]*[1-9][0-9]*))$
	负浮点数：^-([1-9]\d*\.\d*|0\.\d*[1-9]\d*)$ 或 ^(-(([0-9]+\.[0-9]*[1-9][0-9]*)|([0-9]*[1-9][0-9]*\.[0-9]+)|([0-9]*[1-9][0-9]*)))$
	浮点数：^(-?\d+)(\.\d+)?$ 或 ^-?([1-9]\d*\.\d*|0\.\d*[1-9]\d*|0?\.0+|0)$
	
	校验字符的表达式
	汉字：^[\u4e00-\u9fa5]{0,}$
	英文和数字：^[A-Za-z0-9]+$ 或 ^[A-Za-z0-9]{4,40}$
	长度为3-20的所有字符：^.{3,20}$
	由26个英文字母组成的字符串：^[A-Za-z]+$
	由26个大写英文字母组成的字符串：^[A-Z]+$
	由26个小写英文字母组成的字符串：^[a-z]+$
	由数字和26个英文字母组成的字符串：^[A-Za-z0-9]+$
	由数字、26个英文字母或者下划线组成的字符串：^\w+$ 或 ^\w{3,20}$
	中文、英文、数字包括下划线：^[\u4E00-\u9FA5A-Za-z0-9_]+$
	中文、英文、数字但不包括下划线等符号：^[\u4E00-\u9FA5A-Za-z0-9]+$ 或 ^[\u4E00-\u9FA5A-Za-z0-9]{2,20}$
	可以输入含有^%&',;=?$\"等字符：[^%&',;=?$\x22]+
	禁止输入含有~的字符：[^~\x22]+
	
	三、特殊需求表达式
	Email地址：^\w+([-+.]\w+)*@\w+([-.]\w+)*\.\w+([-.]\w+)*$
	域名：[a-zA-Z0-9][-a-zA-Z0-9]{0,62}(/.[a-zA-Z0-9][-a-zA-Z0-9]{0,62})+/.?
	InternetURL：[a-zA-z]+://[^\s]* 或 ^http://([\w-]+\.)+[\w-]+(/[\w-./?%&=]*)?$
	手机号码：^(13[0-9]|14[5|7]|15[0|1|2|3|5|6|7|8|9]|18[0|1|2|3|5|6|7|8|9])\d{8}$
	电话号码("XXX-XXXXXXX"、"XXXX-XXXXXXXX"、"XXX-XXXXXXX"、"XXX-XXXXXXXX"、"XXXXXXX"和"XXXXXXXX)：^(\(\d{3,4}-)|\d{3.4}-)?\d{7,8}$
	国内电话号码(0511-4405222、021-87888822)：\d{3}-\d{8}|\d{4}-\d{7}
	电话号码正则表达式（支持手机号码，3-4位区号，7-8位直播号码，1－4位分机号）: ((\d{11})|^((\d{7,8})|(\d{4}|\d{3})-(\d{7,8})|(\d{4}|\d{3})-(\d{7,8})-(\d{4}|\d{3}|\d{2}|\d{1})|(\d{7,8})-(\d{4}|\d{3}|\d{2}|\d{1}))$)
	身份证号(15位、18位数字)，最后一位是校验位，可能为数字或字符X：(^\d{15}$)|(^\d{18}$)|(^\d{17}(\d|X|x)$)
	帐号是否合法(字母开头，允许5-16字节，允许字母数字下划线)：^[a-zA-Z][a-zA-Z0-9_]{4,15}$
	密码(以字母开头，长度在6~18之间，只能包含字母、数字和下划线)：^[a-zA-Z]\w{5,17}$
	强密码(必须包含大小写字母和数字的组合，不能使用特殊字符，长度在8-10之间)：^(?=.*\d)(?=.*[a-z])(?=.*[A-Z]).{8,10}$
	日期格式：^\d{4}-\d{1,2}-\d{1,2}
	一年的12个月(01～09和1～12)：^(0?[1-9]|1[0-2])$
	一个月的31天(01～09和1～31)：^((0?[1-9])|((1|2)[0-9])|30|31)$
	钱的输入格式：
	有四种钱的表示形式我们可以接受:"10000.00" 和 "10,000.00", 和没有 "分" 的 "10000" 和 "10,000"：^[1-9][0-9]*$
	这表示任意一个不以0开头的数字,但是,这也意味着一个字符"0"不通过,所以我们采用下面的形式：^(0|[1-9][0-9]*)$
	一个0或者一个不以0开头的数字.我们还可以允许开头有一个负号：^(0|-?[1-9][0-9]*)$
	这表示一个0或者一个可能为负的开头不为0的数字.让用户以0开头好了.把负号的也去掉,因为钱总不能是负的吧。下面我们要加的是说明可能的小数部分：^[0-9]+(.[0-9]+)?$
	必须说明的是,小数点后面至少应该有1位数,所以"10."是不通过的,但是 "10" 和 "10.2" 是通过的：^[0-9]+(.[0-9]{2})?$
	这样我们规定小数点后面必须有两位,如果你认为太苛刻了,可以这样：^[0-9]+(.[0-9]{1,2})?$
	这样就允许用户只写一位小数.下面我们该考虑数字中的逗号了,我们可以这样：^[0-9]{1,3}(,[0-9]{3})*(.[0-9]{1,2})?$
	1到3个数字,后面跟着任意个 逗号+3个数字,逗号成为可选,而不是必须：^([0-9]+|[0-9]{1,3}(,[0-9]{3})*)(.[0-9]{1,2})?$
	备注：这就是最终结果了,别忘了"+"可以用"*"替代如果你觉得空字符串也可以接受的话(奇怪,为什么?)最后,别忘了在用函数时去掉去掉那个反斜杠,一般的错误都在这里
	xml文件：^([a-zA-Z]+-?)+[a-zA-Z0-9]+\\.[x|X][m|M][l|L]$
	中文字符的正则表达式：[\u4e00-\u9fa5]
	双字节字符：[^\x00-\xff] (包括汉字在内，可以用来计算字符串的长度(一个双字节字符长度计2，ASCII字符计1))
	空白行的正则表达式：\n\s*\r (可以用来删除空白行)
	HTML标记的正则表达式：<(\S*?)[^>]*>.*?|<.*? /> ( 首尾空白字符的正则表达式：^\s*|\s*$或(^\s*)|(\s*$) (可以用来删除行首行尾的空白字符(包括空格、制表符、换页符等等)，非常有用的表达式)
	腾讯QQ号：[1-9][0-9]{4,} (腾讯QQ号从10000开始)
	中国邮政编码：[1-9]\d{5}(?!\d) (中国邮政编码为6位数字)
	IP地址：((?:(?:25[0-5]|2[0-4]\\d|[01]?\\d?\\d)\\.){3}(?:25[0-5]|2[0-4]\\d|[01]?\\d?\\d))