---
layout:     post
title:      备战CCC-映射表map & 元组pair
subtitle:   请心怀感激的使用Python
date:       2018-07-30
author:     Max
header-img: img/post-bg-rixi19.jpg
catalog: true
tags:
    - C++
    - 橘子树
    - 数学
    - 滑铁卢
---

## 前言
![咳.......png](https://upload-images.jianshu.io/upload_images/10219317-046a21aff0aad5fe.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
此处无言胜有言

## 0x01 why map？
映射是指两个集合之间的元素的相互对应关系。
通俗地说，就是一个元素对应另外一个元素。
比如有一个姓名的集合 {"Tom", "Jone", "Mary"}，班级集合{1,2}。姓名与班级之间可以有如下的映射关系：class("Tom") =1，class("Jone")=2，cclass("Mary") =1    我们称其中的姓名集合为 关键字集合（key），班级集合为值集合（value）。

就是字典了，行吧，~~某些个编程语言的行为已经构成了抄袭，律师函警告！~~

## 0x02 make 一个 map
这个部分和以前写过的STL结构不太一样，以前生成STL元素都是这样子写的：
```
STL_name<type> name;
```
但是map不是这样子的，定义一个map你需要写两个变量：
```
map<T1,T2> name;
```
想一想why？


以前的元素都是只包含有一种数据类型的，比如int啦，或者char啊，
但是映射表不是，因为是映射，映.射.一种数据到另外一种，

但是有人会说了，不就是像字典一样，一个key对应一个value吗？我们不是只需要定义value的值不就行了吗？

那个，key是不是int类型？
了解了吧？

## 0x03 插入映射？insert
对于C++这个语言比较蛋疼的一点就是，Python什么都帮你做好了，但是C++都要自己建造(build)，比如下面这个映射表map？
正常人应该会想：{x,x,x} —— {y,y,y}
天真咯小伙子，Python帮你把x-y 元组给写好咯，
也就是说，在C++中你要用map要先学另外一个STL结构——元组，

下次当你在虐待Python的时候请心怀感激的使用，

还好你不需要import元组的库到你的头文件里面去，算是良心发现了：
```
#include <map>
#include <string>
using namespace std;
int main() {
    map<string, int> dict;  // {}
    dict.insert(pair<string, int>("Tom", 1)); // {"Tom"->1}
    dict.insert(pair<string, int>("Jone", 2)); // {"Tom"->1, "Jone"->2}
    dict.insert(pair<string, int>("Mary", 1)); // {"Tom"->1, "Jone"->2, "Mary"->1}
    dict.insert(pair<string, int>("Tom", 2)); // {"Tom"->1, "Jone"->2, "Mary"->1}
    return 0;
}
```
通过上面的栗子就可以清楚的知道元组和insert和用法，档然咯单独拿出来写也是可以的~
pair<int, char>(1, 'a') 定义了一个整数 1 和字符a的pair。
我们向映射中加入新映射对的时候就是通过加入pair来实现的。


**如果插入的 key 之前已经有了 value，不会用插入的新的 value 替代原来的 value，也就是此次插入是无效的。**

但是其实严格意义上面是不需要写的那么复杂的，直接映射STL也会自动帮你创建好pair元素的，比如像下面这样：
```
#include <map>
#include <string>
#include <stdio.h>
using namespace std;
int main() {
    map<string, int> dict;  // {}
    dict["Tom"] = 1; // {"Tom"->1}
    dict["Jone"] = 2; // {"Tom"->1, "Jone"->2}
    dict["Mary"] = 1; // {"Tom"->1, "Jone"->2, "Mary"->1}
    printf("Mary is in class %d\n", dict["Mary"]);
    printf("Tom is in class %d\n", dict["Tom"]);
    return 0;
}
```
上面的代码里面没有出现pair之类的关键字，心情就很舒畅~
但是其实Tom第一次赋值的时候并不是赋值的1，系统将会自动为"Tom"生成一个映射，其value为对应类型的默认值。并且我们可以之后再给映射赋予新的值，比如dict["Tom"] = 3，这样为我们提供了另一种方便的插入手段。


Tom -> 默认值 -> 1 = pair(“Tom”,1)
## 0x04 访问元素
就像上面的代码所展示的，map这种元素是有下标的（你要映射的值就算下标啊），和set不一样，这是一种有序的数据结构
```
#include <map>
#include <string>
#include <stdio.h>
using namespace std;
int main() {
    map<string, int> dict;  // {}
    dict["Tom"] = 1; // {"Tom"->1}
    dict["Jone"] = 2; // {"Tom"->1, "Jone"->2}
    dict["Mary"] = 1; // {"Tom"->1, "Jone"->2, "Mary"->1}
    if (dict.count("Mary")) {
        printf("Mary is in class %d\n", dict["Mary"]);
    } else {
        printf("Mary has no class");
    }
    return 0;
}
```
有时候当你加入一个key的之后，系统会自动给你生成一个value，这时候就要用到上面的那个count函数了，在if中只要是非0的都是true，所以直接写count(要查找的名字)就可以快速对整个map进行查找，~~效率不知道比手动遍历高到哪里去了~~

在map中，只要被映射过的，count都会返回1，否则则是0。

## 0x05 手动遍历
map中的标准遍历姿势：
```
#include <map>
#include <string>
#include <iostream>
using namespace std;
int main() {
    map<string, int> dict;  // {}
    dict["Tom"] = 1; // {"Tom"->1}
    dict["Jone"] = 2; // {"Tom"->1, "Jone"->2}
    dict["Mary"] = 1; // {"Tom"->1, "Jone"->2, "Mary"->1}
    for (map<string, int>::iterator it = dict.begin(); it != dict.end(); ++it) {
        cout << it->first << " is in class " << it->second << endl;
    }
    return 0;
}
```
注意到了没有，我们在上面的循环语句里面使用begin和end了，但是输出语句里面还有一个first和second，其实这个就要说一下map的原理的，map说白了就是储存了许多的pair的一个元素，如果我们要遍历map的话我们其实是在遍历每一个的元组，
但是元组里面的元素怎么访问呢？这时候就要用->来进行访问了：
如果定义如下：


A *p则：p->play()使用; 左边是结构指针。
A p 则：p.paly()使用; 左边是结构变量。


总结：
箭头（->）：左边必须为指针；
点号（.）：左边必须为实体。


也就是说迭代器it分别指向了元组的两个项目，那么.......如果不是这么写，会不会直接输出像Python的(x,y)一样的输出呢？
![直接访问测试.PNG](https://upload-images.jianshu.io/upload_images/10219317-b242326af3b4a6b6.PNG?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

果然不行，报了一个STl类型的错误出来，
嘛，以后记得加上->啦~【这就是不加的后果！】

## 0x06 map的其他骚操作
### map的主要操作函数【没讲到的基本上就和之前的是一样的了】
方法|功能  
-- | --
insert | 插入一对映射   
count | 查找关键字  
erase | 删除关键字  
size | 获取映射对个数  
clear	 | 清空

### 大佬，如果我tm就像插入相同数据怎么办？
抱歉，这个真的没办法，但至少我可以用数组插入让你覆写（覆盖掉以前的值）

至少插入生效了嘛，嘿嘿~
```
#include <string>
#include <iostream>
#include<map>
using namespace std;
int main()
{
       map<int, string> mapStudent;
       mapStudent[1] =  "student_one";
       mapStudent[2] =  "student_two";
       mapStudent[3] =  "student_three";
       
       mapStudent[1] =  "student_change";
       
       map<int, string>::iterator  iter;
       for(iter = mapStudent.begin(); iter != mapStudent.end(); ++iter)
    {
           cout<<iter->first<<" "<<iter->second<<endl;
    }
    return 0;
}

输出：
1 student_change
2 student_two
3 student_three
```

### 如果你想要不用count查找也是可以的
这里提供了一个以前都没写~~【但是都有的】~~的方法：find

find函数来定位数据出现位置，它返回的一个迭代器，当数据出现时，它返回数据所在位置的迭代器，如果map中没有要查找的数据，它返回的迭代器等于end函数返回的迭代器【也就是最后一个元组】

```
#include <map>
#include <string>
#include <iostream>
Using namespace std;
Int main()
{
       Map<int, string> mapStudent;
       mapStudent.insert(pair<int, string>(1, “student_one”));
       mapStudent.insert(pair<int, string>(2, “student_two”));
       mapStudent.insert(pair<int, string>(3, “student_three”));
       map<int, string>::iterator iter;
       iter = mapStudent.find(1);
    if(iter != mapStudent.end())
    {
           Cout<<”Find, the value is ”<<iter->second<<endl;
    }
    Else
    {
           Cout<<”Do not Find”<<endl;
    }
    return 0;
}
```
根本连循环语句都不用写，因为find会直接返回一个迭代器【惊了】，
但是看起来好像还是没有count方便呀qwq


#### 数据的删除，除了clear，还能更有个性化嘛？
有的，首先是erase，其次还可以用迭代器批量删除：
```
#include <map>
#include <string>
#include <iostream>
using namespace std;
int main()
{
	//生成并添加数据 
    map<int, string> mapStudent;
    mapStudent.insert(pair<int, string>(1, "student_one"));
    mapStudent.insert(pair<int, string>(2, "student_two"));
    mapStudent.insert(pair<int, string>(3, "student_three"));
    
    map<int, string>::iterator iter;
    
	for(iter=mapStudent.begin();iter!=mapStudent.end();++iter)
	{
		cout<<iter->first<<" "<<iter->second<<" ";
	}
	cout<<endl;
//如果你要演示输出效果，请选择以下的一种，你看到的效果会比较好
       
	//如果要删除1,用迭代器删除
    //这里直接把iter定位到需要删除的位置了，因为find返回一个迭代器嘛 
    iter = mapStudent.find(1);
    mapStudent.erase(iter);
    for(iter=mapStudent.begin();iter!=mapStudent.end();++iter)
	{
		cout<<iter->first<<" "<<iter->second<<" ";
	}
	cout<<endl;
    //如果要删除1，用关键字删除
    int n = mapStudent.erase(1);//如果删除了会返回1，否则返回0
    cout<<"关键字删除返回:"<<n<<endl;
	for(iter=mapStudent.begin();iter!=mapStudent.end();++iter)
	{
		cout<<iter->first<<" "<<iter->second<<" ";
	}
	cout<<endl;
	   
    //用迭代器，成片的删除
    //一下代码把整个map清空
    mapStudent.erase(mapStudent.begin(), mapStudent.end());
    for(iter=mapStudent.begin();iter!=mapStudent.end();++iter)
	{
		cout<<iter->first<<" "<<iter->second<<" ";
	}
	cout<<endl;
    //成片删除要注意的是，也是STL的特性，删除区间是一个前闭后开的集合

    return 0;
}

输出：
1 student_one 2 student_two 3 student_three
2 student_two 3 student_three
关键字删除返回:0 【之前已经被删除过啦~】
2 student_two 3 student_three
```

### 关于排序的问题
这里摘抄一位大佬的吐槽：
>由于STL是一个统一的整体，map的很多用法都和STL中其它的东西结合在一起，比如在排序上，这里默认用的是小于号，即less<>，如果要从大到小排序呢，这里涉及到的东西很多，在此无法一一加以说明。
还要说明的是，map中由于它内部有序，由红黑树保证，因此很多函数执行的时间复杂度都是log2N的，如果用map函数可以实现的功能，而STL  Algorithm也可以完成该功能，建议用map自带函数，效率高一些。
下面说下，map在空间上的特性，否则，估计你用起来会有时候表现的比较郁闷，由于map的每个数据对应红黑树上的一个节点，这个节点在不保存你的数据时，是占用16个字节的，一个父节点指针，左右孩子指针，还有一个枚举值（标示红黑的，相当于平衡二叉树中的平衡因子），我想大家应该知道，这些地方很费内存了吧，不说了……

说了这么多，排序为什么不去用set呢？？？？啧啧

## 最后
参考资料：
[计蒜客的示例代码卡卡卡~](https://www.jisuanke.com)
[C++ 点(.)操作符和箭头(->)操作符- CSDN博客](https://blog.csdn.net/qq457163027/article/details/54237782)
[C++ STL map 删除一个元素erase() 操作- CSDN博客](https://blog.csdn.net/huutu/article/details/17404301)
[C++ map删除元素的三种方式- CSDN博客](https://blog.csdn.net/zvall/article/details/52267007)

~~劝大家别去官网翻垃圾了，就算是宝贝你也会当成垃圾扔掉的~~
英语母语太君，职业除外。

## 最后的最后
![哭了.PNG](https://upload-images.jianshu.io/upload_images/10219317-4d4655684328111d.PNG?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
我😭了，你们呢？呜呜呜

[球球你们给口饭次吧，不然我要偷奶奶的养老金了](https://0xc000005.github.io/)