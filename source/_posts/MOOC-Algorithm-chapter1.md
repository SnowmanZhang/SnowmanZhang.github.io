---
title: MOOC_Algorithm_chapter1
date: 2018-01-15 20:09:17
tags:
- 算法
- MOOC
categories:
- 编程
---

本系列文章对中国大学MOOC中的《程序设计与算法（二）算法基础》一课讲解的例题进行总结和归纳。一篇文章为一周。

第一周的主题是枚举，含有四个例题。

<!--more-->

## 第一题：完美立方

**题目描述**：形如a3= b3 + c3 + d3的等式被称为完美立方等式。例如 123= 63 + 83 + 103 。编写一个程序，对任给的正整数N (N≤100)，寻找所有的四元组(a, b, c, d)，使得a3 = b3 + c3 + d3，其中a,b,c,d 大于 1, 小于等于N，且b<=c<=d。

**输入** 一个正整数N (N≤100)。 

**输出** 每行输出一个完美立方。输出格式为： `Cube = a, Triple = (b,c,d)` 其中a,b,c,d所在位置分别用实际求出四元组值代入。

**感想**

一开始我以为形如完美立方公式的四个数字会存在一定的逻辑关系，如令a = b + d.但这只是一部分的情况。因为要枚举所有情况，故不能漏报。

```c
int sample_1(int num) {
    for (int a = 2; a <= num; ++a)
        for (int b = 2;b < a;++b)
            for (int c = b;c < a;++c)
                for (int d = c;d < a;++d)
                    if (a*a*a == b * b*b + c * c*c + d * d*d)
                        printf("Cube = %d ,Triple = (%d,%d,%d)\n", a, b, c, d);
    return 1;
}

```


在这个里面唯一需要注意的地方是，四重for循环的每个变量的起点都是上一轮变量，这是参考了`b<c<d<a`的题目要求而压缩了计算量。

## 第二题：生理周期

**题目描述** 人有体力、情商、智商的高峰日子，它们分别每隔 23天、28天和33天出现一次。对于每个人，我们想 知道何时三个高峰落在同一天。给定三个高峰出现 的日子p,e和i（不一定是第一次高峰出现的日子）, 再给定另一个指定的日子d，你的任务是输出日子d 之后，下一次三个高峰落在同一天的日子（用距离d 的天数表示）。例如：给定日子为10，下次出现三 个高峰同一天的日子是12，则输出2。

**输入** 输入四个整数：p, e, i和d。 p, e, i分别表示体力、情感和智力 高峰出现的日子。d是给定的日子，可能小于p, e或 i。所有给 定日子是非负的并且小于或等于365，所求的日子小于或等于 21252。

**输出** 从给定日子起，下一次三个高峰同一天的日子（距离给定日子 的天数）。

**感想** 这次自己的思路和答案差不多，利用同时出现的日子具有最小公倍数性质这一特点，跳着循环，而多想一点，即便是跳着循环，先确定23天和先确定33天有没有区别呢？首先是第一重循环本不必做————从上一个高潮日子直接乘以其周期即可确定。而假使一个数字的倍数列表对另一个数字的倍数关系与否成随机分布，则被查找的数字越大，找到的频率就越低，所以理论上将33天放于第一层循环(又可以说不循环，直接每次递增33天查询是否为28天的倍数，最后查找23天)，是最快的。但在时间复杂度上没有显著改善。

```c

int sample_2() {
    int p, e, i, d,caseNo = 0;
    while(cin >> p>>e>>i>>d && p!=-1){
        ++ caseNo;
        int k;
        for (k = d + 1;(k - p)%23;++k);
        for (;(k - e) % 28;k += 23);
        for (;(k - i) % 33;k += 23 * 28);
        cout << "Case" << caseNo << ": the next triple peak in "<< k-d;
        }
    return 1;
}
```

## 第三题：称硬币

**题目描述** ：有12枚硬币。其中有11枚真币和1枚假币。假币和真 币重量不同，但不知道假币比真币轻还是重。现在， 用一架天平称了这些币三次，告诉你称的结果，请你 找出假币并且确定假币是轻是重（数据保证一定能找 出来）。

**输入** ：第一行是测试数据组数。
每组数据有三行，每行表示一次称量的结果。银币标号为 A-L。每次称量的结果用三个以空格隔开的字符串表示： 天平左边放置的硬币天平右边放置的硬币平衡状态。其 中平衡状态用`up`, `down`, 或 `even`表示, 分别为右 端高、右端低和平衡。天平左右的硬币数总是相等的。

**输出**： 输出哪一个标号的银币是假币，并说明它比真币轻还是重。

**感想** 这个例题给出的示例代码，如果说进化还有空间，例如说，可以在输入后的第一步首先判断三个给出信息是否存在even的情况，如果有则将该行所有硬币排除在外。若同一枚硬币既出现在down端又出现在up端，则亦排除之。但这样的话就会排除掉绝大部分情况，只有少部分情况不会被判断出来，如问题硬币未上称，或者上称硬币过少不重复。为了体现枚举的思想，将其逐个列举也是不错的办法，况且数量级并不大。

```c
int sample_3(){
    int t;
    cin >> t;
    while(t--){
        for(int i=0;i<3;++i) cin >> Left[i] >> Right[i] >> result[i];
        for(char c='A';c<='L';c++){
            if(IsFake(c,true)){
                cout << c<<" is the counterfeit coin and it is light \n";
                break;
            }
            else if(IsFake(c,false)){
                cout << c<<" is the counterfeit coin and it is heavy.\n";
                break;

            }
        }

    return 1;
    }
}

bool IsFake(char c,bool light){
    for(int i = 0;i<3;++i){
        char *pLeft,*pRight;
        if(light){
            pLeft = Left[i];
            pRight = Right[i];
        }
        else {
            pLeft = Right[i];
            pRight = Left[i];
        }
        switch(result[i][0]){
        case 'u':
            if ( strchr(pRight,c) == NULL )
                return false;
            break;
        case 'e':
            if (strchr(pLeft,c) || strchr(pRight,c))
                return false;
            break;
        case 'd':
            if (strchr(pLeft,c)==NULL)
                return false;
            break;
        }

    }

    return true;

}

```
 

## 第四题：熄灯问题

问题描述过长，有兴趣者可以寻路至MOOC中国大学相关课程视频下。

**感想**

这个思路确乎巧妙，只枚举出第一行就可以推测出来下面n行并且从而判断是否能成功，将一个2^30数量级的问题降成2^5的问题。这里第二个巧妙的地方在于位运算，将一行数字存入一个char变量中，然后使用GetBit函数和SetBit函数存取内容。将运算速度拉升到可观的水平。

这里我提供另一个思路。既然2^30次方过高，必然不能强取，但我们可以自左上角到右下角遍历，如遍历左上角6个区块，则可以锁定左上角3个区块的值，遍历10个区块可得6个区块的值，同理可得遍历的情况，这样的话可以一路解析到最后。如果中间遇到了不可解，可再另行安排报错机制。

```c


char oriLights[5];
char lights[5];
char result[5];

int GetBit(char c,int i){   //将该行第i位的bit取出，c表示该行
    return (c>>i) & 1;
}
void SetBit(char &c,int i,int v){  //设置该行的第i位bit 的值为 v
    if(v){
        c |= (1<<i);
    }
    else
        c &= ~(i<<i);
}
void FlipBit(char &c,int i){  //设置该行第i位反转
    c ^= (1<<i);
}

void OutputResult(int t,char result[]) //输出矩阵Result
{

    cout << "PUZZLE #" << t << endl;
    for (int i = 0;i<5;++i){
    for (int j=0;j<6;++j){
        cout << GetBit(result[i],j);
        if (j<5)
            cout << " ";
    }
    cout<<endl;
}

}


int sample_4(){

    int T;
    cin>>T;
    for(int t=1;t<=T;++t){   //有T个矩阵需要判断
        for(int i=0;i<5;++i)   //131-136行用于读入原始灯光矩阵
            for(int j=0;j<6;++j){
                int s;
                cin >>s;
                SetBit(oriLights[i],j,s);
            }
        for(int n=0;n<64;++n){
            int switchs = n;
            memcpy(lights,oriLights,sizeof(oriLights));
            for(int i=0;i<5;++i){
                result[i] = swithchs;
                for(int j=0;j<6;j++){
                    if (GetBit(switchs,j)){
                        if (j>0)
                            FlipBit(lights[i],j-1);
                        FlipBit(lights[i],j);
                        if(j<5)
                            FlipBit(lights[i],j+1);
                    }
                }
                if ( i < 4 )
                    lights[i+1] ^= switchs;
                switch = lights[i];
            }
            if (lights[4]==0){
                OutputResult(t,result);
                break;
            }

        }

    }

}

```
