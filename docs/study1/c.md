### 十进制与二进制：

```
【问题描述】

假设有一16位的无符号整数，可以对其二进制数据进行循环右移操作，右移后仍然是无符号整数。编写程序从控制台读入要右移的整数和循环右移的位数，求得并输出循环右移后的十进制数据。

【输入形式】

从控制台输入要右移的十进制整数（大于等于0，小于等于65535）和循环右移的位数（大于等于0，小于等于16），两整数之间用一个空格分隔。

【输出形式】

向控制台输出循环右移后的十进制整数。

【输入样例1】

65532 2

【输出样例1】

16383

【样例1说明】

输入的待右移的整数为65532，该无符号整数的二进制形式为：1111111111111100，向右循环右移两位后的二进制形式为：0011111111111111，对应的十进制数据为：16383。

【输入样例2】

6 3

【输出样例2】

49152

【样例2说明】

输入的待右移的整数为6，该无符号整数的二进制形式为：0000000000000110，向右循环右移3位后的二进制形式为：1100000000000000，对应的十进制数据为：49152。
```

- ![123](c/1113.jpg)
- 接下来我用四张图来介绍一下十进制与二进制的相互转化，并且如果你看懂了接下来的四张图，那么你就可以读懂上面这个程序是如何实现的了，先简单介绍一下这个程序的整体思路：1.将二进制的16位数存储到d中，然后从左到右以此判别此位是否为1。2.利用i来判别16次位数，利用c来实现右移，实际上是改变了一开始读取的位次，到尾后又从头开始读取，然后再利用32768也就是2的15次方来实现转换为十进制。
- ![123](c/11091.jpg)
- ![123](c/11092.jpg)
- ![123](c/11093.jpg)
- ![123](c/11094.jpg)

---

## 编程练习题

```
【问题描述】

输入精度e 和实数x,用下列公式求cos x 的近似值,精确到最后一项的绝对值小于e｡要求定义和调用函数funcos(e,x)求余弦函数的近似值｡



【输入形式】

输入两个浮点数：精度e和实数x

【输入输出样例1】（下划线部分表示输入）

e: 0.001

x: 1

cos(x)=0.540

【样例说明】

输入提示符后要加一个空格。例如&ldquo;e: &rdquo;，其中&ldquo;:&rdquo;后要加一个且只能一个空格。

输出语句的&ldquo;=&rdquo;两边无空格

计算结果保留3位小数

英文字母区分大小写。必须严格按样例输入输出。
```

```
#include <stdio.h>
#include <math.h>

double factorial(int n) {
	if (n == 1)
		return 1;
	return factorial(n - 1) * n;
}

double funcos(double e, double x);

int main() {
	double e, x;
	scanf("%lf %lf", &e, &x);
	printf("e: x: cos(x)=%.3lf", funcos(e, x));
	return 0;
}

double funcos( double e, double x) {
	int a;
	double c, d;
	a = 2;

	d = 1;
	int n = 2;
	do {
		c = pow(x, a) /factorial(n);
		if (a % 4 == 0) {
			d += c;
		} else if (a % 2 == 0) {
			d -= c;
		}
		a++;
		n++;
	} while (c > e);
	return d;

}
```

这个程序一定要注意阶乘函数的类型必须为double，这是因为阶乘作为了分母，而分子是double类型，所以如果阶乘使用int等整数类型，就会出现有意想不到的错误，本题的错误在于循环中的递归运算到达峰值后，不会再进行下去，出现极大值错误，直接多出2到3位的大小，然后输出，你可以自己试一试，然后就懂我在说什么了，当然必须要看每一步循环的动态输出才可以。这给我们的启示就是，在对俩个变量进行运算时，最好要让它们为相同的类型，如果俩个数较小，那么可能不会产生什么影响，但如果像这道题一样位数十分的大，那么就要注意了！

## Learn C the hard way：

### 注意事项(目前我遇到的)：

1.在运行你需要运行的文件时，你必须先cd的文件所在目录，列入我的ex4文件

![123](c/11141.jpg)

- 首先输入命令：

```
cd ~/Documents/doc
```

- 然后再输入：

```
./ex4
```

这样进可以运行文件中的c程序了

2.要修改文件内容时，比如ex4.c，首先你会在这个文件中进行修改，可是当你修改完，然后运行这个文件输入：

```
./ex4
```

- 你发现怎么还是上次的结果，我不是已经修改了吗，这时你只需要先输入命令：

```
make ex4
```

- 它就会更新新的ex4

### 我目前学到或者见识到的东西：

### make:

1.make(在Python中，你仅仅需要输入`python`，就可以运行你想要运行的代码。Python的解释器会运行它们，并且在运行中导入它所需的库和其它东西。Make会构建源码，执行测试，设置一些选项以及为你做所有Python通常会做的事情。)

### valgrind:

2.valgrind（是一种检测错误的工具）

a.下载：

```
1|wget https://sourceware.org/pub/valgrind/valgrind-3.16.1.tar.bz2      #使用下载命令下载压缩包
```

```
2|tar-jxvf valgrind-3.16.1.tar.bz2   #解压安装包
```

```
3|cd valgrind-3.16.1    #进入目录
```

```
4|./configure    #配置valgrind,生成MakeFile文件
```

```
5|make    #编译Valgrind
```

```
6|make install   #安装Valgrind
```

在valgrind下运行文件

```
valgrind ./ex4
```

### 句法

3.1使用char来声明，以周围带有'（单引号）的单个字符来表示，使用%c来打印。

```
char initial = 'A';
printf("I have an initial %c.\n", initial);
```

- 3.2.使用char name[]来声明，以周围带有“的一些字符来表示，使用%s来打印。

```
char first_name[] = "Zed";
printf("I have a first name %s.\n", first_name);
```

- 3.3.long用%ld占位符
- 3.4.使用%e以科学计算法的形式打印

- 3.5.特殊语法'\0'声明了一个字符，这样创建了一个“空字节”字符，实际上是数字0：

```
char nul_byte = '\0';
int care_percentage = bugs * nul_byte;
printf("Which means you should care %d%%.\n",
            care_percentage);
```

- 结果：

```
Which means you should care 0%.
```

- 3.6.用俩个%%来打印一个%

```
printf("Which means you should care %d%%.\n",
            care_percentage);
```

- 3.7.&&与||：

```
【问题描述】

   在中国数学历史上广泛流传着一个&ldquo;韩信点兵&rdquo;的故事。韩信点兵时，为了知道有多少兵，同时又能保住军事机密，便让士兵排队报数：

按从1至5报数，记录最末一个士兵的报数为1；

按从1至6报数，记录最末一个士兵的报数为5；

按从1至7报数，记录最末一个士兵的报数为4；

按从1至11报数，记录最末一个士兵的报数为10；

你知道韩信至少有多少兵？

【输入输出说明】无输入，输出至少应有的士兵数。
```

```
1  	 :  	#include <stdio.h>
2  	 :  	
3  	 :  	int main() {
4  	 :  	
5  	 :  	    int a ;
6  	 :  	    for (a = 1; a % 5 != 1 || a % 6 != 5 || a % 7 != 4 || a % 11 != 10; a++);
7  	 :  	    printf("%d", a);
8  	 :  	    return 0;
9  	 :  	}
10   :  	
 
```

- 我利用此题来介绍一下二者的区别
- 1.||表示或者，要想此题得出正确答案，那个a除以那四个数之后，得到的余数也是分别四个特定的数。只要不满足其中任何一个，那么就需要使a不断递增，知道找到那一位正确的数。此代码中，只要a满足4个条件中的任意一个，也就是有一个除完数之后余数并不是自己想要的，那么就会继续进入循环，它是一种或的关系。也就是只要有一个满足条件就会进入循环，当所有条件都不满足时，才会跳出循环。
- 2.&&表示并且，也就是说，只有当四个条件都满足时，才会进入循环，也就是说a必须做到除完数后，没有一个余数符合条件，才会进入循环，更新a的值。如果有其中一个余数符合我们想要的，而另外三个不符合，它也会跳出循环，直接输出。也就是只要有一个不满足循环条件，那么它就会跳出循环，满足所有条件时才会进入循环。

### 数组

4.数组大小：

```
#include <stdio.h>

int main(int argc, char *argv[])
{
    int areas[] = {10, 12, 13, 14, 20};
    char name[] = "Zed";
    char full_name[] = {
        'Z', 'e', 'd',
         ' ', 'A', '.', ' ',
         'S', 'h', 'a', 'w', '\0'
    };

    // WARNING: On some systems you may have to change the
    // %ld in this code to a %u since it will use unsigned ints
    printf("The size of an int: %ld\n", sizeof(int));
    printf("The size of areas (int[]): %ld\n",
            sizeof(areas));
    printf("The number of ints in areas: %ld\n",
            sizeof(areas) / sizeof(int));
    printf("The first area is %d, the 2nd %d.\n",
            areas[0], areas[1]);

    printf("The size of a char: %ld\n", sizeof(char));
    printf("The size of name (char[]): %ld\n",
            sizeof(name));
    printf("The number of chars: %ld\n",
            sizeof(name) / sizeof(char));

    printf("The size of full_name (char[]): %ld\n",
            sizeof(full_name));
    printf("The number of chars: %ld\n",
            sizeof(full_name) / sizeof(char));

    printf("name=\"%s\" and full_name=\"%s\"\n",
            name, full_name);

    return 0;
}
```

```
The size of an int: 4
The size of areas (int[]): 20
The number of ints in areas: 5
The first area is 10, the 2nd 12.
The size of a char: 1
The size of name (char[]): 4
The number of chars: 4
The size of full_name (char[]): 12
The number of chars: 12
name="Zed" and full_name="Zed A. Shaw"

```

int的大小是4，areas中含有5个整数，所以自然需要用20个字节来储存它

char的大小是1，name中含有三个字符的字符串，full__ _name中含有12个单字符，而打印出它们的字节大小却分别是4和12，full__ _name很好理解,因为它本身就含有12个单字符，但name为什么是4呢，百思不得其解下，我谷歌了一下，原来char类型的数组是以“\0”空字符结尾的。这下就明白了， full__name本身就是以“\0”空字符来结尾的，所以就是12个字节，不需要+1，而name原本应该是“zed\0”，所以它需要+1.

5.数组和字符串：

```
#include <stdio.h>

int main(int argc, char *argv[])
{
    int numbers[4] = {0};
    char name[4] = {'a'};

    // first, print them out raw
    printf("numbers: %d %d %d %d\n",
            numbers[0], numbers[1],
            numbers[2], numbers[3]);

    printf("name each: %c %c %c %c\n",
            name[0], name[1],
            name[2], name[3]);

    printf("name: %s\n", name);

    // setup the numbers
    numbers[0] = 1;
    numbers[1] = 2;
    numbers[2] = 3;
    numbers[3] = 4;

    // setup the name
    name[0] = 'Z';
    name[1] = 'e';
    name[2] = 'd';
    name[3] = '\0';

    // then print them out initialized
    printf("numbers: %d %d %d %d\n",
            numbers[0], numbers[1],
            numbers[2], numbers[3]);

    printf("name each: %c %c %c %c\n",
            name[0], name[1],
            name[2], name[3]);

    // print the name like a string
    printf("name: %s\n", name);

    // another way to use name
    char *another = "Zed";

    printf("another: %s\n", another);

    printf("another each: %c %c %c %c\n",
            another[0], another[1],
            another[2], another[3]);

    return 0;
}

```

输出：

```
numbers: 0 0 0 0
name each: a   
name: a
numbers: 1 2 3 4
name each: Z e d 
name: Zed
another: Zed
another each: Z e d 
```

- 5.1第一个numbers并没有提供全部的四个参数，第一个name也只提供了一个元素，可是为什么numbers可以将所有的元素打印出来，而name却只有一个a。那是因为numbers为int类型，未提供的剩余元素都会默认为0，而char未写出的元素默认为“\0"空字符，而空字符并不会显示。
- 5.2创建一个字符串的常用语法char name[4] = {'a'}和char *another = "name"俩中，其中后者比较常用。

- 在C语言中字符数组和字符串没有什么不同。

6.字符串数组和循环

字符串数组的写法：

```
1.char *asa = "blash" 
2.char *asa[] = {"blash1","blash2"}
3.char asa[] = {'b','l','a','s','h'}
```

然后就是比较难理解的：

```
#include <stdio.h>

int main(int argc, char *argv[])
{
    int i = 0;
    argc =10;
    
    // go through each string in argv
    // why am I skipping argv[0]?
    for(i = 0; i < argc; i++) {
        printf("arg %d: %s\n", i, argv[i]);
    }
```

例子中的`for`循环使用`argc`和`argv`，遍历了命令行参数，像这样：

- OS将每个命令行参数作为字符串传入`argv`数组，<mark>程序名称`./ex10`在下标为0的位置</mark>，剩余的参数紧随其后。
- OS将`argc`置为`argv`数组中参数的数量，所以你可以遍历它们而不会越界。要记住如果你提供了一个参数，程序名称是第一个，参数应该在第二个。

我特意将for中初始的i赋值为了0，就为了检验它所说的程序名称在下标为0的位置，也就是argv[0]这个数组中存储着程序名称，然后我也给argc赋了值，否则程序直接略过for循环。

结果展现：

![ ](c/11221.jpg)

看到它所描述的文件在下标为0处存储的结果输出时，我非常激动，因为我原本并不理解它在说什么，而且给的程序如果自己不加修改，也不会有什么东西会输出。 

 ### while循环：

在这次的练习中，我明白了下面这俩的话的含义：

- OS将每个命令行参数作为字符串传入`argv`数组，<mark>程序名称`./ex10`在下标为0的位置</mark>，剩余的参数紧随其后。
- OS将`argc`置为`argv`数组中参数的数量，所以你可以遍历它们而不会越界。要记住如果你提供了一个参数，程序名称是第一个，参数应该在第二个。

```
#include <stdio.h>

int main(int argc, char *argv[])
{
    // go through each string in argv

    int i = 0;
    while(i < argc) {
        printf("arg %d: %s\n", i, argv[i]);
        i++;
    }

    // let's make our own array of strings
    char *states[] = {
        "California", "Oregon",
        "Washington", "Texas"
    };

    int num_states = 4;
    i = 0;  // watch for this
    while(i < num_states) {
        printf("state %d: %s\n", i, states[i]);
        i++;
    }

    return 0;
}

```

![ ](c/11251.jpg)

- 原来我在for循环部分就比较困惑，为什么`argc`并没有赋值，却可以使`i<argc`的条件为真，并且可以使函数正常运行，直到我看到了while循环的俩个测试与输出，我才知道它大概是如何实现的了。

- 当我输入`./ex11 test it`这时产生了3个命令行参数，然后这些命令行参数作为字符串传入了`argv`数组，分别存储在`argv[0]`,`argv[1]`,`argv[2]`当中，然后就是解释`argc`是如何得到赋值的，为什么它并没有被输入，却又不是随机数，因为它可以使程序正常运行。原来它的值就是argv数组中所有参数的总数量，这样一来，因为i与数组一样都是从0开始的，argc却是从1开始的，所以i永远小于argc，这样就会使条件为真，并且为真的次数可以用来输出所有的数组中的字符串。

布尔表达式：

在C语言中，实际上没有真正的“布尔”类型，而是用一个整数来代替，0代表`false`，其它值代表`true`。上一个练习中表达式`i < argc`实际上值为1或者0。

在本次代码中需要注意的问题：



- 忘记初始化`int i`，使循环发生错误。
- 忘记初始化第二个循环的`i`，于是`i`还保留着第一个循环结束时的值。你的第二个循环可能执行也可能不会执行。
- 忘记在最后执行`i++`自增，你会得到一个“死循环”

<mark>最后一条给我们以启发，我们可以通过使用一个恒为真的条件，来创造一个无限循环来满足我们有时候对于程序进行不断循环计算或者运行的需要，再使用

```
if（我们要的结果result）{
printf("我们要的结果%d",result);
break;
}
```

break用来跳出无限循环，举个栗子：

```
【问题描述】输入4个整数，要求按由小到大的顺序输出。
【输入形式】输入四个整数
【输出形式】输出排好序的四个数
【样例输入】3 6 -1 -34
【样例输出】-34 -1 3 6
【样例说明】没有说明性的输出
【评分标准】5个测试样例
```

以下方法也被称为冒泡排序法：

```
#include <stdio.h>
int main() {
    int a,b,c,d,e,f,g=0;
    scanf("%d%d%d%d",&a,&b,&c,&d);
    int sz[4]={a,b,c,d};
    while(1){
     for(e=0;e<3;e++){
      if(sz[e]>sz[e+1]){
      f=sz[e];
      sz[e]=sz[e+1];
      sz[e+1]=f; }}
      if(sz[0]<=sz[1]&&sz[1]<=sz[2]&&sz[2]<=sz[3])
      break;}
    while(g<=3){
     printf("%d ",sz[g]);
     g++;}
    return 0; 
}
```

### if,else if,else

我对这个语法有着特别的感觉，因为它也是那么的清新先介绍一下练习中的它，再举一个题例。

```
#include <stdio.h>

int main(int argc, char *argv[])
{
    int i = 0;

    if(argc == 1) {
        printf("You only have one argument. You suck.\n");
    } else if(argc > 1 && argc < 4) {
        printf("Here's your arguments:\n");

        for(i = 0; i < argc; i++) {
            printf("%s ", argv[i]);
        }
        printf("\n");
    } else {
        printf("You have too many arguments. You suck.\n");
    }

    return 0;
}
```

首先的if(argc == 1)就给了一个小范围，然后的else if(argc > 1 && argc < 4)就比较灵活了，其实它完全可以写成else if( **argc** < 4），它们最终会使程序运行的结果完全一致，else if( **argc** < 4）实际代表的范围是（-00，1）U(1,4)，然后再使用else代表范围[4,+00)

程序结果展示：

![ ](c/11252.jpg)

例题展示：

```
【问题描述】当前小学生的成绩单由以前的百分制改为A、B、C、D四个等级，编写程序完成分数的自动转换工作。转换规则如下：60分一下为D；60-69分为C；70-89分为B；90分以上为A。
【输入形式】70
【输出形式】B
【样例输入】95
【样例输出】A
```

```
#include <stdio.h>

int main() {
	int num;
	scanf("%d", &num);
	if (num < 60) {
		printf("D");
	} else if (num < 70) {
		printf("C");
	} else if (num < 90) {
		printf("B");
	} else {
		printf("A");
	}
	return 0;

}
```

### Switch语句:

```
#include <stdio.h>

int main(int argc, char *argv[])
{
    if(argc != 2) {
        printf("ERROR: You need one argument.\n");
        // this is how you abort a program
        return 1;
    }

    int i = 0;
    for(i = 0; argv[1][i] != '\0'; i++) {
        char letter = argv[1][i];

        switch(letter) {
            case 'a':
            case 'A':
                printf("%d: 'A'\n", i);
                break;

            case 'e':
            case 'E':
                printf("%d: 'E'\n", i);
                break;
         
                if(i > 2) {
                    // it's only sometimes Y
                    printf("%d: 'Y'\n", i);
                }
                break;

            default:
                printf("%d: %c is not a vowel\n", i, letter);
        }
    }

    return 0;
}
```

![ ](c/11271.jpg)

我将程序走一遍：

- 此程序从上往下走，当输入的命令行参数的个数不等于而时，就会输出一段话，让你来输入出来`./ex*`外，再输入一个命令行参数，所以输入` ./ex13 Zed Shaw`会被警告，然后就是读取`argv[1][i]`代表了从下标为1的数组中依次取出带个字母，然后再来判断是否属于元音字母(大小写都会进入），每判断完一个字母，它就会跳出Switch语句重新进入。如果不是元音会进入default，输出它不是一个元音。
- 这里利用for循环来实现每次letter的变换，有点像putchar函数。

```
char ch;
ch = putchar;
```

作者要点：

- switch实际上是一个<mark>跳转表</mark>，只能够放置结果为整数的表达式，而不是一些随机的布尔表达式，这些整数用于计算从switch顶部到匹配部分的跳转。
- switch的工作原理：

1.编译器会标记`swicth`语句的顶端，我们先把它记为地址Y。

2.接着对`switch`中的表达式求值，产生一个数字。在上面的例子中，数字为`argv[1]`中字母的原始的ASCLL码。

3.编译器也会把每个类似`case 'A'`的`case`代码块翻译成这个程序中距离语句顶端的地址，所以`case 'A'`就在`Y + 'A'`处。

4.接着计算是否`Y+letter`位于`switch`语句中，如果距离太远则会将其调整为`Y+Default`。

5.一旦计算出了地址，程序就会“跳”到代码的那个位置并继续执行。这就是一些`case`代码块中有`break`而另外一些没有的原因。

6.如果输出了`'a'`，那它就会跳到`case 'a'`，它里面没有`break`语句，所以它会贯穿执行底下带有代码和`break`的`case 'A'`。

7.最后它执行这段代码，执行`break`完全跳出`switch`语句块。

原则：

1.总是要包含一个`default:`分支，可以让你接住被忽略的输入。

2..不要允许“贯穿”执行，除非你真的想这么做，这种情况下最好添加一个`//fallthrough`的注释。

3.一定要先编写`case`和`break`，再编写其中的代码。

4.如果能够简化的话，用`if`语句代替。

### 编写并使用函数：

```
#include <stdio.h>
#include <ctype.h>

// forward declarations
int can_print_it(char ch);
void print_letters(char arg[]);

void print_arguments(int argc, char *argv[])
{
    int i = 0;

    for(i = 0; i < argc; i++) {
        print_letters(argv[i]);
    }
}

void print_letters(char arg[])
{
    int i = 0;

    for(i = 0; arg[i] != '\0'; i++) {
        char ch = arg[i];

        if(can_print_it(ch)) {
            printf("'%c' == %d ", ch, ch);
        }
    }

    printf("\n");
}

int can_print_it(char ch)
{
    return isalpha(ch) || isblank(ch);
}


int main(int argc, char *argv[])
{
    print_arguments(argc, argv);
    return 0;
}
```

![ ](c/11272.jpg)

自己写一个函数大概的<mark>步骤</mark>：

类型一

1.先声明函数

2.再定义函数（本例就是如此）

3最后在主函数中进行调用

类型二

1.先声明函数

2.在主函数中进行调用

3.最后定义函数

此程序要点：

1.判断条件为函数的返回值

```
if(can_print_it(ch))    #if当中的式子为非零时，执行if当中的内容，当if当中的值为0时，不执行|也就是看can_print_it(char ch)函数的返回值是否为0
```

```
int can_print_it(char ch)
{
    return isalpha(ch) || isblank(ch);
}
```

此函数返回值为字母与空格的或运算，一真为正（返回1），全假为假（返回0）

2.测试时输入的命令行参数

```
1|./ex14 hi this is cool
```

```
2|./ex14 "I go 3 spaces"
```

为什么这俩个测试命令的结果一个有输出空格，而另一个没有呢？
因为在读取时，第一个用空格隔开共读取了5个命令行参数，而第二个共读取了2个命令行参数，双引号整体算一个命令行参数。所以第二个会逐字读取。

### 指针（哈哈，看了好几遍才理解）

```
#include <stdio.h>
 
int main(int argc, char *argv[])
{
    // create two arrays we care about
    int ages[] = {23, 43, 12, 89, 2};
    char *names[] = {
        "Alan", "Frank",
        "Mary", "John", "Lisa"
    };

    // safely get the size of ages
    int count = sizeof(ages) / sizeof(int);
    int i = 0;

    // first way using indexing
    for(i = 0; i < count; i++) {
        printf("%s has %d years alive.\n",
                names[i], ages[i]);
    }

    printf("---\n");
    // setup the pointers to the start of the arrays
    int *cur_age = ages;
    char *cur_name* = names;

    // second way using pointers
    for(i = 0; i < count; i++) {
        printf("%s is %d years old.\n",
                *(cur_name+i), *(cur_age+i));
    }
    //names[i],ages[i]

    printf("---\n");

    // third way, pointers are just arrays
    for(i = 0; i < count; i++) {
        printf("%s is %d years old again.\n",
                cur_name[i], cur_age[i]);
    }

    printf("---\n");

    // fourth way with pointers in a stupid complex way
    for(cur_name = names, cur_age = ages;
            (cur_age - ages) < count;
            cur_name++, cur_age++)
    {
        printf("%s lived %d years so far.\n",
                *cur_name, *cur_age);
    }

    return 0;
}
```

此程序以四种方式打印出了相同的输出：

![ ](c/11281.jpg)

在他说“<mark>这个看似简单的程序却包含了大量的信息，其目的是在我向你讲解前尝试让你自己弄清楚指针。直到你写下你认为指针做了什么之前，不要往下阅读。</mark>”我先从头到尾看走了一遍程序，将每一步都理解了一下，尽可能地知道它们都是如何实现的了，一些比较新的地方，我也是意会了一下（比如比较抽象的数组++ ：“cur_name++”）。做完这一步后，我也是对这个程序不再陌生了，越看越熟悉（因为这里的指针与数组有着微妙的关系，指针在用着数组的语法，有时甚至只是换了一种形式来表达相同的含义）。

- sizeof(int)在64位中值为8

sizeof可以用类型做参数，strlen只能用char*做参数，且必须是以“\0”结尾的。

- `*(cur_name+i)`和`name[i]`是一样的，你应该把它读作“‘`cur_name`指针加`i`’的值”。这样就可以理解第一种与第二种本质是一样的。
- 第三种甚至可以说只是给`name`和`age`数组换了个名字一样
- 第四中就比较复杂了全部都使用指针（我有时再Frank的影响下，认为是快捷方式）来作为变量实现for中的初始化和条件判断和递增，这显然要比前三种看起来更复杂一些，但是你如果把它单纯的看作是一个变量那就非常好理解了，实际上并不是。
- 无论怎么样，你都不应该把指针和数组混为一谈。它们并不是相同的东西，即使C让你以一些相同的方法来使用它们。例如，如果你访问上面代码中的`sizeof(cur_age)`，你会得到指针的大小，而不是它指向数组的大小。如果你想得到整个数组的大小，你应该使用数组的名称`age`，就像第12行那样
- 创建了指向`names`的指针。`char *`已经是“指向`char`的指针”了，所以它只是个字符串。你需要两个层级，因为`names`是二维的，也就是说你需要`char **`作为“指向‘指向字符的指针’的指针”。

```
for(cur_age = ages;
(cur_age - ages) < count;
cur_age++;)
#cur_age为基址，然后对基址不断的实现偏移，差值分别为0 、1、2、3、4、5，因为count为5所以加到5后不再进入循环。for循环的增加部分增加了cur_name和cur_age的值，这样它们可以只想names和ages的下一个元素。
```

指针原理：

C把你的计算机看成一个庞大的字节数组。

- 在你的计算机中开辟一块内存。
- 将`ages`这个名字“指向”它的起始位置。
- 通过选取`ages`作为<mark>基址</mark>，并且获取位置为`i`的元素，来对内存块进行索引。
- 将`ages+i`处的元素转换成大小正确的有效的`int`，这样就返回了你想要的结果：下标`i`处的`int`。

现在访问数组和指针的语法都会翻译成相同的机器码，并且表现一致。由此，你应该每次尽可能使用数组，并且按需将指针用作提升性能的手段。

# 结构体

```
#include <stdio.h>
#include <assert.h>
#include <stdlib.h>
#include <string.h>

// 定义一个 Person 结构体，表示一个人的信息
struct Person {
    char *name;  // 人的姓名
    int age;     // 人的年龄
    int height;  // 人的身高
    int weight;  // 人的体重
};

// 函数声明，用于创建一个新的 Person 结构体
struct Person *Person_create(char *name, int age, int height, int weight);

// 函数声明，用于销毁一个 Person 结构体
void Person_destroy(struct Person *who);

// 函数声明，用于打印一个 Person 结构体的信息
void Person_print(struct Person *who);

int main(int argc, char *argv[])
{
    // 创建两个 Person 结构体实例
    struct Person *joe = Person_create("Joe Alex", 32, 64, 140);
    struct Person *frank = Person_create("Frank Blank", 20, 72, 180);

    // 打印它们的信息以及内存中的位置
    printf("Joe is at memory location %p:\n", joe);
    Person_print(joe);

    printf("Frank is at memory location %p:\n", frank);
    Person_print(frank);

    // 使两人都年龄增加20岁，然后再次打印它们的信息
    joe->age += 20;
    joe->height -= 2;
    joe->weight += 40;
    Person_print(joe);

    frank->age += 20;
    frank->weight += 20;
    Person_print(frank);

    // 销毁两个结构体实例，进行内存清理
    Person_destroy(joe);
    Person_destroy(frank);

    return 0;
}

// 函数定义，用于创建一个新的 Person 结构体
struct Person *Person_create(char *name, int age, int height, int weight)
{
    // 使用 malloc 分配内存来存储一个 Person 结构体的空间
    struct Person *who = malloc(sizeof(struct Person));
    assert(who != NULL);  // 检查内存分配是否成功

    // 使用 strdup 复制输入的姓名字符串，并将其分配给 Person 结构体的 name 成员
    who->name = strdup(name);

    // 将传入的年龄、身高和体重分配给相应的成员变量
    who->age = age;
    who->height = height;
    who->weight = weight;

    return who;
}

// 函数定义，用于销毁一个 Person 结构体
void Person_destroy(struct Person *who)
{
    assert(who != NULL);  // 检查结构体是否存在

    // 释放姓名字符串的内存
    free(who->name);
    
    // 释放结构体的内存
    free(who);
}

// 函数定义，用于打印一个 Person 结构体的信息
void Person_print(struct Person *who)
{
    printf("Name: %s\n", who->name);
    printf("\tAge: %d\n", who->age);
    printf("\tHeight: %d\n", who->height);
    printf("\tWeight: %d\n", who->weight);
}

```

![ ](c/1251.jpg)

- malloc用于内存分配
- assert确保从malloc得到一块有效的内存
- NULL表示未设置或无效的指针
- asset大致检查了malloc会不会返回NULL
- x->y即（*x）.y语法来初始化struct Person的每个成员

### 12月7号补充内容：

![ ](c/1271.jpg)

- <mark>who->name也可以用*who.name来代替</mark>（12.7）
- <mark>12月7日对终于加深了指针的原理的理解，struct Person *who = malloc(sizeof(struct Person)); #这里的*号其实就可以看作是指针pointer类型，后面是用malloc来请求的一块地址，让这个指针来指向这块内存，这块内存的大小就是`struct Person`里面包含的各个数据类型的大小的总和，一个char加上三个int.</mark>

- ```c
  struct Person *Person_create(char *name, int age, int height, int weight)   
  ```

​     这块代码其实创造了一个函数。

```c
who->name = strdup(name);
```

这块访问了name并使用strduo来使name获得name数组的内容，之所以它不想age,height,weight那样，是因为它是一个数组，本质是指针，不存在string这样字符串的数据类型，数组其实只存储了第一个字符的地址，然后通过不断的递增访问来获取剩下的内容，直到读取NULL也就是'/0'

free用来释放内存，防止内存泄漏。



结构体给我一种先创造出一个模版，然后再创造函数来使用这个模版，再然后直接调用这个函数，只需要简单输入一些参数就可以。

使用了`free`函数来交还通过`malloc`和`strdup`得到的内存。如果你不这么做则会出现“内存泄露”。

内存溢出与泄漏：

- 内存溢出 out of memory**，是指程序在申请内存时，没有足够的内存空间供其使用，出现out of memory；比如申请了一个integer,但给它存了long才能存下的数，那就是内存溢出。内存溢出就是你要求分配的内存超出了系统能给你的，系统不能满足需求，于是产生溢出。

- 内存泄漏是指你向系统申请分配内存进行使用(new)，可是使用完了以后却不归还(delete)，结果你申请到的那块内存你自己也不能再访问（也许你把它的地址给弄丢了），而系统也不能再次将它分配给需要的程序。一个盘子用尽各种方法只能装4个果子，你装了5个，结果掉倒地上不能吃了。这就是溢出！比方说栈，栈满时再做进栈必定产生空间溢出，叫上溢，栈空时再做退栈也产生空间溢出，称为下溢。就是分配的内存不足以放下数据项序列,称为内存溢出.

![ ](c/1261.jpg)

1.<mark>其实结构体就相当于一个含有多种类型的变量的变量类型，它可以声明一个变量。并且结构体具有很便利的用途，比如介绍几个学生的情况，就可以创造一个`struct student`的结构体来提供基本的介绍框架，里面包括了姓名，年龄，成绩。之后你就可以直接来使用这个模版，以上图片中的例子是声明了一个a数组，将俩个人的信息存储在这个数组当中，它的便利就体现出来了，只需要在大括号里填入对于的参数即可，如果是大于一个人只需要用逗号将俩个大括号隔开。</mark>

2.图中的typedef是用来重命名的，只需要在你需要重命名的结构体前面写下typedef，然后在结构体大括号的最后写下重命名后的名字即可。

![ ](c/1162.jpg)

<mark>在一个结构体中甚至还可以嵌套另一个结构体，然后输入参数时只需要再加一个大括号即可!</mark>

## 堆和栈的内存分配

```
#include <stdio.h>
#include <assert.h>
#include <stdlib.h>
#include <errno.h>
#include <string.h>

#define MAX_DATA 512
#define MAX_ROWS 100

// 定义 Address 结构体，表示数据库中的一条记录
struct Address {
    int id;
    int set;
    char name[MAX_DATA];
    char email[MAX_DATA];
};

// 定义 Database 结构体，表示整个数据库
struct Database {
    struct Address rows[MAX_ROWS];
};

// 定义 Connection 结构体，表示与数据库的连接
struct Connection {
    FILE *file;
    struct Database *db;
};

// 函数声明，用于打印错误信息并退出程序
void die(const char *message);

// 函数声明，用于打印一条记录
void Address_print(struct Address *addr);

// 函数声明，用于加载数据库内容
void Database_load(struct Connection *conn);

// 函数声明，用于打开数据库连接
struct Connection *Database_open(const char *filename, char mode);

// 函数声明，用于关闭数据库连接
void Database_close(struct Connection *conn);

// 函数声明，用于写入数据库内容
void Database_write(struct Connection *conn);

// 函数声明，用于创建数据库
void Database_create(struct Connection *conn);

// 函数声明，用于设置数据库中的一条记录
void Database_set(struct Connection *conn, int id, const char *name, const char *email);

// 函数声明，用于获取数据库中的一条记录
void Database_get(struct Connection *conn, int id);

// 函数声明，用于删除数据库中的一条记录
void Database_delete(struct Connection *conn, int id);

// 函数声明，用于列出数据库中所有的记录
void Database_list(struct Connection *conn);

int main(int argc, char *argv[])
{
    // 检查命令行参数是否足够
    if (argc < 3) die("USAGE: ex17 <dbfile> <action> [action params]");

    // 从命令行参数获取数据库文件名和操作类型
    char *filename = argv[1];
    char action = argv[2][0];
    struct Connection *conn = Database_open(filename, action);
    int id = 0;

    // 如果有额外的参数，将其解析为记录的 ID
    if (argc > 3) id = atoi(argv[3]);
    if (id >= MAX_ROWS) die("There's not that many records.");

    // 根据操作类型执行相应的操作
    switch (action) {
        case 'c':
            Database_create(conn);
            Database_write(conn);
            break;

        case 'g':
            if (argc != 4) die("Need an id to get");

            Database_get(conn, id);
            break;

        case 's':
            if (argc != 6) die("Need id, name, email to set");

            Database_set(conn, id, argv[4], argv[5]);
            Database_write(conn);
            break;

        case 'd':
            if (argc != 4) die("Need id to delete");

            Database_delete(conn, id);
            Database_write(conn);
            break;

        case 'l':
            Database_list(conn);
            break;
        default:
            die("Invalid action, only: c=create, g=get, s=set, d=del, l=list");
    }

    // 关闭数据库连接
    Database_close(conn);

    return 0;
}

// 函数定义，用于打印错误信息并退出程序
void die(const char *message)
{
    // 如果 errno 设置，使用 perror 打印详细错误信息，否则只打印错误信息
    if (errno) {
        perror(message);
    } else {
        printf("ERROR: %s\n", message);
    }

    // 退出程序
    exit(1);
}

// 函数定义，用于打印一条记录
void Address_print(struct Address *addr)
{
    printf("%d %s %s\n", addr->id, addr->name, addr->email);
}

// 函数定义，用于加载数据库内容
void Database_load(struct Connection *conn)
{
    int rc = fread(conn->db, sizeof(struct Database), 1, conn->file);
    if (rc != 1) die("Failed to load database.");
}

// 函数定义，用于打开数据库连接
struct Connection *Database_open(const char *filename, char mode)
{
    // 分配 Connection 结构体的内存
    struct Connection *conn = malloc(sizeof(struct Connection));
    if (!conn) die("Memory error");

    // 分配 Database 结构体的内存
    conn->db = malloc(sizeof(struct Database));
    if (!conn->db) die("Memory error");

    // 根据打开模式选择打开文件
    if (mode == 'c') {
        conn->file = fopen(filename, "w");
    } else {
        conn->file = fopen(filename, "r+");

        if (conn->file) {
            // 如果是读写模式，加载数据库内容
            Database_load(conn);
        }
    }

    // 检查文件是否成功打开
    if (!conn->file) die("Failed to open the file");

    return conn;
}

// 函数定义，用于关闭数据库连接
void Database_close(struct Connection *conn)
{
    // 检查 Connection 结构体是否存在
    if (conn) {
        // 如果文件存在，关闭文件
        if (conn->file) fclose(conn->file);
        // 如果数据库结构体存在，释放内存
        if (conn->db) free(conn->db);
        // 释放 Connection 结构体的内存
        free(conn);
    }
}

// 函数定义，用于写入数据库内容
void Database_write(struct Connection *conn)
{
    // 将文件指针定位到文件开头
    rewind(conn->file);

    // 将整个数据库结构体写入文件
    int rc = fwrite(conn->db, sizeof(struct Database), 1, conn->file);
    if (rc != 1) die("Failed to write database.");

    // 刷新文件缓冲区
    rc = fflush(conn->file);
    if (rc == -1) die("Cannot flush database.");
}

// 函数定义，用于创建数据库
void Database_create(struct Connection *conn)
{
    int i = 0;

    // 遍历数据库中的所有记录，初始化为未设置状态
    for (i = 0; i < MAX_ROWS; i++) {
        // 创建 Address 结构体的原型以进行初始化
        struct Address addr = {.id = i, .set = 0};
        // 直接赋值
        conn->db->rows[i] = addr;
    }

```

![ ](c/1262.jpg)

1.调用了`malloc`就是堆，其它的是静态分配，或者栈。

2.当你将一个结构体传入到函数中，如果你不使用指针，直接传入完整的结构体，结构体就可以取代一部分函数调用栈，这意味着它会使用更多空间，而且需要被拷贝到栈中。

3.在创建Database时使用堆传递需要再各处共享的东西。

4.数据库里边不是一个指针，里边其实有一个完整的地址元素数组`struct Database {structure Address rows[MAX_ROWS]}`MAX_ROWS是100，就会有100个地址，每个地址有512大小的字符和512大小的email,也就是100*512，再加上俩个int，这是很大的一块内存数据。

5.嵌套结构体指针

```
db->conn->row + i
```

它读作“读取`db`中的`conn`中的`rows`的第`i`个元素

6.die函数

```
void die(const char *message)
{
    // 检查全局变量 errno 是否被设置
    if(errno) {
        // 如果 errno 被设置，使用 perror 打印错误消息和描述
        perror(message);
    } else {
        // 如果 errno 没有被设置，直接打印错误消息
        printf("ERROR: %s\n", message);
    }

    // 退出程序，状态码为1，表示发生了错误
    exit(1);
}
```

`const char *message`：这是一个指向字符串的指针，表示要打印的错误消息。

`if(errno)`：检查全局变量` errno` 是否被设置。`errno` 是一个在标准库中定义的全局变量，它在发生错误时被设置为一个特定的错误码。如果` errno` 被设置，说明发生了错误。

`perror(message)：`如果 `errno` 被设置，`perror `函数会打印一个描述错误的消息，然后打印 message 参数指向的字符串。这样可以提供更详细的错误信息。`perror` 函数根据` errno `的值查找相应的错误描述，并将其打印到标准错误流`(stderr)`。

else 块：如果 `errno` 没有被设置，表示错误消息不需要更详细的描述，直接使用 printf 打印传入的 message 字符串。

exit(1)：无论是通过 `perror `还是` printf `打印错误消息，都会导致程序退出。exit(1) 用于终止程序的执行，状态码为1，表示发生了错误。

这个 die 函数的设计使得在程序遇到错误时能够方便地打印错误消息并退出程序，同时提供了可选的详细错误描述。

## 函数指针

```
typedef int (*compare_cb)(int a, int b);

int *bubble_sort(int *numbers, int count, compare_cb cmp)
{
    // ...
    if(cmp(target[j], target[j+1]) > 0) {
        // ...
    }
    // ...
}

void test_sorting(int *numbers, int count, compare_cb cmp)
{
    // ...
    int *sorted = bubble_sort(numbers, count, cmp);
    // ...
}

```

- 在这里，compare_cb 是一个函数指针类型，它可以指向一个以两个整数为参数并返回整数的比较函数。bubble_sort 函数接受一个比较函数作为参数，并在排序过程中调用这个比较函数。test_sorting 函数也接受一个比较函数，并在测试排序时调用。

  通过这种方式，你可以灵活地指定排序过程中使用的比较方式，而不必修改排序算法的实现。这样的设计使得代码更具可复用性和可扩展性。

- `compare_cb cmp` 是在函数参数列表中定义的一个参数，其中 compare_cb 是一个类型，而 cmp 是该类型的一个实例（函数指针）。这里使用 cmp 的目的是通过这个函数指针来调用比较函数。

  具体来说：

  compare_cb 是一个函数指针类型的别名，它可以指向以两个整数为参数并返回整数的函数。
  cmp 是作为参数传递进来的具体的比较函数，可以是 sorted_order、reverse_order 或 strange_order 中的任何一个，因为它们都符合 compare_cb 类型的定义。
  这种方式允许你在调用 bubble_sort 函数时灵活地指定不同的比较函数，而不需要修改 bubble_sort 的实现。这是 C 语言中使用函数指针来实现回调的一种常见用法。

```
if(cmp(target[j], target[j+1]) > 0) {
    // ...
}
```

- cmp 是一个函数指针，指向一个用于比较两个整数的比较函数。这个函数接受两个整数参数，然后返回一个整数值，表示它们的相对顺序。

  target[j] 和 target[j+1] 是数组中相邻的两个元素。

  cmp(target[j], target[j+1]) 调用了比较函数，将两个相邻元素的值传递给比较函数进行比较。

  if(cmp(target[j], target[j+1]) > 0) 检查比较函数的返回值是否大于0。如果返回值大于0，表示 target[j] 大于 target[j+1]，那么需要进行交换，以实现升序排序。

  整个 if 语句表示：如果比较函数返回的结果表明 target[j] 大于 target[j+1]，那么执行相关的交换操作，以便将较大的元素移动到数组的右侧。这就是冒泡排序算法的核心部分，通过多次迭代，不断比较和交换相邻的元素，实现整个数组的排序。

### “回调” :

向其他函数传递“回调”是一种常见的编程模式，它允许你将一个函数作为参数传递给另一个函数，使得另一个函数能够调用这个传递进来的函数。这种做法通常用于实现灵活的、可定制的行为，尤其是在涉及到事件处理、排序、过滤或其他需要在运行时决定的情境中。

在C语言中，回调通常通过函数指针来实现。函数指针是指向函数的指针，允许你在运行时动态地选择要调用的函数。
