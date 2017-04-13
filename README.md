# replace的进阶+配合正则表达式
## 基本用法
> 一般用来简单的替换

    myString = "javascript is a good script language";
     //在此我想将字母a替换成字母A
     console.log(myString.replace("a","A"));


> 配合正则来替换str

    //将字母a替换成字母A   少了/g 就会只匹配第一个a 
    myString = "javascript is a good script language";
    console.log(myString.replace(/a/,"A"));
    //console.log(myString.replace(new RegExp('a','gm'),"A"));

    //将字母a替换成字母A  正确的写法 /g表示匹配所有
    myString = "javascript is a good script language";
    console.log(myString.replace(/a/g,"A"));

> 一般来说，会如上这些就可以完成基本工作了。

## 高阶用法
### 特殊标记 对于正则replace约定了一个特殊标记符$

1. $i (i:1-99) : 表示从左到右正则子表达式所匹配的文本。
2. $&:表示与正则表达式匹配的全文本。
3. $`(`:切换技能键)：表示匹配字符串的左边文本。
4. $'(‘:单引号)：表示匹配字符串的右边文本。
5. $$：表示$转移。

### 具体例子01
        // 匹配后替换
    var myString= "javascript is a good script language";
    console.log(myString.replace(/(javascript)\s*(is)/g,"$1 $2 fun. it $2"));
* $会匹配到(),$1就相当于第一个()。
* $1="javascript"、$2="is".
* 结果：javascript is fun.it is a good script language;

### 例子02

    //在本例中，我们将把所有的花引号""替换为直引号''：
    var myString = '"a", "b"';
    myString = myString.replace(/"([^"]*)"/g, "'$1'");//寻找所有的"abb"形式字符串，此时组合表示字符串，，然后用'$1'替换
    console.log(myString)
    
### 例子03 颠倒

        //在本例中，我们将把 "zzc,ccz" 转换为 "ccz zzc" 的形式：
    myString = "zzc , ccz";
    myString = myString.replace(/(\w+)\s*, \s*(\w+)/, "$2 $1");
    console.log(myString)
  > 结果： "ccz,zzc";
#### 类似的例子
        myString = "boy & girl";
        myString.replace(/(\w+)\s*&\s*(\w+)/g,"$2 & $1") //girl & boy
        console.log(myString)
### 例子04

        //    $&:表示与正则表达式匹配的全文本。
    myString = "boy";
    myString.replace(/\w+/g,"$&-$&") // boy-boy
    console.log("$& "+myString)

    //    $`(`:切换技能键)：表示匹配字符串的左边文本。
    myString = "javascript";
    myString.replace(/script/,"$& != $`") //javascript != java
    console.log("$` "+myString)

    //    $'(‘:单引号)：表示匹配字符串的右边文本。
    myString = "javascript";
    myString.replace(/java/,"$&$' is ") // javascript is script
    console.log("$` "+myString)
    
## 高阶用法02-replace+function

### 例子01
         //无敌的函数 - replace第二个参数可以传递函数
        //如果第二参数是一个函数的话，那么函数的参数是什么呢？
    console.log('replace功能5 - 无敌的函数 - replace第二个参数可以传递函数')
    myString = "bbabc";
    myString.replace(/(a)(b)/g, function(){
        console.log(arguments) // ["ab", "a", "b", 2, "bbabc"]
    });

-  参数将依次为：
1. 整个正则表达式匹配的字符。匹配所有的ab
2. 第一分组匹配的内容、第二分组匹配的内容…… 以此类推直到最后一个分组 a  b 。
3. 此次匹配在源自符串中的下标（位置）就是案例中a的角标。
4. 原字符串
* 所以例子的输出是 ["ab", "a", "b", 2, "bbabc"]

### 例子02

    //在本例中，我们将把字符串中所有单词的首字母都转换为大写：
    myString = 'aaa bbb ccc';
    myString=myString.replace(/\b\w+\b/g, function(word){
                return word.substring(0,1).toUpperCase()+word.substring(1);}
    );
    console.log(myString)


>  用法举例  首字母大写 -- 多个参数 - 第一个表示匹配的整个字符串，后面的表示分组中的内容

    /* 字符^
    意义：表示匹配的字符必须在最前边。
    例如：/^A/不匹配"an A,"中的'A'，但匹配"An A."中最前面的'A'。

    字符$
    意义：与^类似，匹配最末的字符。
    例如：/t$/不匹配"eater"中的't'，但匹配"eat"中的't'。*/
    function capitalize(str){
        return str.replace( /(^|\s)([a-z])/g , function(m,p1,p2){ return p1+p2.toUpperCase();
        } );

    };
    myString = "i am a boy !"
    console.log(capitalize(myString)) //I Am A Boy！
    
    
    
    
    
    
    
