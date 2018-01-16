---
title: MOOC-Algorithm-chapter2-3
date: 2018-01-16 17:39:15
tags:
- 算法
- MOOC
categories:
- 编程
---

由于MOOC课程上的递归由两周组成，所以本篇文章直接介绍第2周和第3周的内容。均为递归相关内容。

<!--more-->

## 1.阶乘

递归的定义与基本用法，相信凡是学习过一点程序设计的同学都大略明白，即在一个函数体内依照一定参数规律调用自身，以达到分化问题的解决方案。在MOOC课程中，老师也提到了递归与普通函数调用一样是通过栈实现的，具体而说，在主函数内调用基础的递归函数，这个递归函数就相当于在主函数上堆了一层栈，它会留有给下一层递归函数return过来的空间，然后进入下一层递归函数再执行，一直到不再调用，需要返回时，再一层一层返回，最后返回主函数。

```c
int Factorial_1(int n)
{
    if (n==0)
        return 1;
    return n*Factorial_1(n-1);
}

```

## 2.波兰表达式

这个题目中所说的逆波兰表达式事实上是波兰表达式，即运算符前置，而逆波兰表达式是运算符后置

**题目描述** 逆波兰表达式是一种把运算符前置的算术表达式(其实一般教科书上称这种表
达式为波兰表达式），例如普通的表达式2 + 3的逆波兰表示法为+ 2 3。逆波兰 表达式的优点是运算符之间不必有优先级关系，也不必用括号改变运算次序，例如 (2 + 3) * 4的逆波兰表示法为* + 2 3 4。本题求解逆波兰表达式的值，其中运算符 包括+ - * /四个。

**感想** 这个题目其实实用性有点堪忧，虽然exp()设计得很简洁易懂，但在输入的时候需要将表达式不断地输入cin>>才能进行运算，因此不如以下面的四则运算式为借鉴模板(虽然确实有点繁复)，这个函数的主要思想是，因为波兰表达式存在这样一个特性“波兰表达式 = 运算符 波兰表达式  波兰表达式”，因此每当输入一个字符就进行是否为运算符的判定，然后准备好case 等return。


```c
double exp(){
    char s[20];
    cin >> s;
    switch(s[0]){
        case '+': return exp()+exp();
        case '-': return exp()-exp();
        case '*': return exp()*exp();
        case '/': return exp()/exp();
        default: return atof(s);
        break;
    }
}
```


## 3. 四则运算

四则运算是比较好的一个示例用以解释递归，程序将递归函数分为三级，即expression_value、term_value、factor_value，分别对应求取表达式值，项值，因子值，程序设计认为，项值通过加减构成表达式，因子通过乘除构成项，而括弧内的值应视作表达式值。由此推出递归关系。在expression_value中获取项值，探查操作符号op是+/-。然后返回对应的表达式值。在term_value中获取因子值，探查操作符号op是否为乘除。然后返回对应的表达式值。

```c


int expression_value()
{
    int result = term_value();
    bool more = true;
    while(more){
        char op = cin.peek(); //返回输入流里的第一个字符而不取走
        if(op=='+'||op=='-'){
            cin.get();
            int value = term_value();
            if(op=='+') result += value;
            else result -= value;
        }
        else more=false;
    }
    return result;
}

int term_value()
{
    int result = factor_value();
    while(true){
        char op = cin.peek();
        if(op=='*'||op == '/'){
            cin.get();
            int value = factor_value();
            if(op == '*') result *= value;
            else result /= value;
        }
        else
            break;
    }
    return result;
}

int factor_value()
{
    int result = 0;
    char c = cin.peek();
    if (c=='('){
            cin.get();
            result = expression_value();
            cin.get();
    }
    else{
        while(isdigit(c)){
            result = 10 * result + c - '0';
            cin.get();
            c = cin.peek();
        }
    }
    return result;
}


```


## 4.爬楼梯问题

**问题描述** 树老师爬楼梯，他可以每次走1级或者2级，输入楼梯的级数， 求不同的走法数
例如：楼梯一共有3级，他可以每次都走一级，或者第一次走一 级，第二次走两级，也可以第一次走两级，第二次走一级，一 共3种方法。

**输入** 输入包含若干行，每行包含一个正整数N，代表楼梯级数，1 <= N <= 30输出不同的走法数，每一行输入对应一行

这个问题的思路很简单，既然求不同走法，那么第一步走一阶和两阶，它们后面的走法必然是不一样的走法，因此对其进行递归遍历即可。程序非常简单

```c
 int stairs(int n)
 {
    if(n<0)
        return 0;
    if(n==0)
        return 1;
    return stairs(n-1) + stairs(n-2);
 }

```

## 5.放苹果问题

**问题描述** 把M个同样的苹果放在N个同样的盘子里，允许有的盘子空着不放， 问共有多少种不同的分法？5，1，1和1，5，1 是同一种分法。

**感想** 实话说这个解法意外到我，它能够递归的亮点就在于，当苹果数大于盘子数，且需要每盘都有苹果时，该情况等同于苹果数 - 盘子数摆放于盘子内而不要求每盘均摆满。再结合若苹果数小于盘子数，则等同于相同苹果数与相同盘子数摆放的情况。怎么说呢，设计得实在巧妙，叹为观止。寥寥几笔就解决了问题，并且复杂度很低，这大概就是算法魅力吧。


```c

 int f(int m,int n){
    if(n>m)
        return f(m,m);
    if(m==0)
        return 1;
    if(n==0)
        return 0;
    return f(m,n-1)+f(m-n,n);
 }

```


## 6.求24点


**问题描述** 给出4个小于10个正整数，你可以使用加减乘除4种运算以及括 号把这4个数连接起来得到一个表达式。现在的问题是，是否存 在一种方式使得得到的表达式的结果等于24。


>这里加减乘除以及括号的运算结果和运算的优先级跟我们平常 的定义一致（这里的除法定义是实数除法）。
比如，对于5，5，5，1，我们知道5 * (5 – 1 / 5) = 24，因此 可以得到24。又比如，对于1，1，4，2，我们怎么都不能得到 24。

**输入** 输入数据包括多行，每行给出一组测试数据，包括4个小于10个正整 数。最后一组测试数据中包括4个0，表示输入的结束，这组数据不用 处理。

**输出** 对于每一组测试数据，输出一行，如果可以得到24，输出“YES”； 否则，输出“NO”。


注意：浮点数比较是否相等，不能用 ==


**感想** 这个题目虽然繁复，但确实有点“凑题”之嫌，于是我们可以看到，程序主体还是大量使用了枚举(当然，不用枚举这题也没法做，毕竟给出的样本数的大小在此)只是在思想上由a,b,...n 求24点转化为先计算两个数得到一个结果，再与其它相比较的递归情况。可能也是因为自己在看答案前就是这样想的，所以看到解法后没有上一题带来的惊喜之感。


```c

 double a[5];
 #define EPS 1e-6
 bool isZero(double x){
    return fabs(x) <= EPS;
 }

 bool count24(double a[],int n)
 {
     if (n==1){
            if(isZero(a[0]-24))
                return true;
            else
                return false;
    }
    double b[5];
    for(int i=0;i<n-1;++i)
        for(int j=i+1;j<n;++j){
            int m=0;
            for(int k=0;k<n;++k)
                if( k!=i && k!=j )
                    b[m++] = a[k];
            b[m] = a[i] + a[j];
            if(count24(b,m+1))
                return true;
            b[m] = a[i] - a[j];
            if(count24(b,m+1))
                return true;
            b[m] = a[j] - a[i];
            if(count24(b,m+1))
                return true;
            b[m] = a[i] * a[j];
            if(count24(b,m+1))
                return true;
            if(!isZero(a[j])){
                b[m] = a[i] / a[j];
                if(count24(b,m+1))
                    return true;
            }
            if(!isZero(a[i])){
                b[m] = a[j] / a[i];
                if(count24(b,m+1))
                    return true;
            }
        }
    return false;
 }


```