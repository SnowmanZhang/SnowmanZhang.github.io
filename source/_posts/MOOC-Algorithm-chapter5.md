---
title: MOOC-Algorithm-chapter5
date: 2018-01-19 20:59:41
tags:
- MOOC
- 算法
categories:
- 编程
---

这章的内容并不多，不过比较重要，其中有两种复杂度在nlogn的排序算法。归并与快排。另外附赠了两个练习题，这章虽然叫做分治，也就是“分而解决以降低复杂度”，但是以前面的递归作为基础。

## 0. 分治的概念

把一个任务，分成形式和原任务相同，但规模更小的 几个部分任务（通常是两个部分），分别完成，或只 需要选一部完成。然后再处理完成后的这一个或几个 部分的结果，实现整个任务的完成。


## 1. 归并排序

数组排序任务可以如下完成

1. 把前一半排序(Mergesort中的两个子递归函数)
2. 把后一半排序(同上)
3. 把两半归并到一个新的有序数组，然后再拷贝回原 数组，排序完成。(merge函数，复杂度为O(n))

```c

void Merge(int a[],int s,int m,int e,int tmp[])
{
    int pb = 0;
    int p1 = s,p2 = m+1;
    while(p1<=m&&p2<=e){
        if(a[p1]<a[p2])
            tmp[pb++] = a[p1++];
        else
            tmp[pb++] = a[p2++];
    }
    while(p1<=m){
        tmp[pb++] = a[p1++];
    }
    while(p2<=e){
        tmp[pb++] = a[p2++];
    }
    for(int i=0 ;i<e-s+1 ;++i)
        a[s+i] = tmp[i];
}


void MergeSort(int a[],int s ,int e,int tmp[])
{
    if(s<e){
        int m=s+(e-s)/2;
        MergeSort(a,s,m,tmp);
        MergeSort(a,m+1,e,tmp);
        Merge(a,s,m,e,tmp);
    }
    //    int size = sizeof(a)/sizeof(int);
    //  MergeSort(a,0,size-1,b);
}

```

在这里，算法中MergeSort函数并没有做实质性的排序，而是不断地把数组分成二等分，一直到子数组内只有一个，即s = e，然后使用Merge对其进行排序。

其复杂度为nlogn,推导公式详见下方图片

```
T(n) = 2*T(n/2) + a*n
    = 2*(2*T(n/4)+a*n/2)+a*n 
    = 4*T(n/4)+2a*n 
    = 4*(2*T(n/8)+a*n/4)+2*a*n 
    = 8*T(n/8)+3*a*n 
    ... 
    = 2^k *T(n/2^k)+k*a*n

```

此时k = logn，因此可得 `T(n) = n + anlogn`,故其复杂度为nlogn。

## 2. 快速排序

数组排序任务可以如下完成：

1. 设k=a\[0\], 将k挪到适当位置，使得比k小的元素都 在k左边,比k大的元素都在k右边（O（n)时间完成） 
2. 把k左边的部分快速排序 
3. 把k右边的部分快速排序

这个思维确实是非常地厉害，它将归并排序的两个步骤(分开，排序)归纳成了一个主步骤，即选中一个数，将比其小的排至左边，比其大的排在右边。再对两者分开排序。

```c


void swap(int &a,int &b)
{
    int tmp =a;
    a =b;
    b = tmp;
}

void QuickSort(int a[],int s,int e)
{
    if(s>=e)
        return ;
    int k = a[s];
    int i=s,j=e;
    while(i!=j){
        while(j>i&&a[j]>=k)
            --j;
        swap(a[i],a[j]);
        while(i<j&&a[i]<k)
            ++i;
        swap(a[i],a[j]);
    }
    QuickSort(a,s,i-1);
    QuickSort(a,i+1,e);
}

```

算法的精髓就在于，排序时设置一头一尾两个指针，若检验指向的数字是符合其要求的，则指向下一个，若不符合，则互换位置，此时再从另一头开始同样比较。

>请体察一个微妙的区别，`while(j>i&&a[j]>=k)`在前且只能在前，因为我们选择的k是数组的起点数，如果`while(i<j&&a[i]<k)`在前则会造成错误。

## 3. 输出前m大的数

**问题描述** 给定一个数组包含n个元素，统计前m大的数并且把这m个数从大到小 输出。


**输入** 第一行包含一个整数n，表示数组的大小。n < 100000。 第二行包含n个整数，表示数组的元素，整数之间以一个空格分开 。每个整数的绝对值不超过100000000。 第三行包含一个整数m。m < n。

**输出** 从大到小输出前m大的数，每个数一行。

**思路**

引入操作 arrangeRight(k): 把数组(或数组的一部分）前k大的 都弄到最右边


如何将前k大的都弄到最右边 

1. 设key=a\[0\], 将key挪到适当位置，使得比key小的元素都在 key左边，比key大的元素都在key右边（线性时间完成）
2. 选择数组的前部或后部再进行 arrangeRight操作

当然，我们选择的key是数组第一个数，假设它的右边有a个数，它不一定会是第k大，所以一定会出现以下三种情况。

1. a = k done 
2. a > k 对此a个元素再进行arrangeRigth(k) 
3. a < k 对左边b个元素再进行arrangeRight(k-a)

此时就可以进行再排序

在这里我们可以看见，实际上arrangeRight对标杆点key的选择是盲目的，如果你提前知道这个数组的大概分布，就可以设置key的大概大小，从而更快地提高排序速度。

算法复杂度见下方

```
T(n) = T(n/2) + a*n 
    = T(n/4) + a*n/2 + a*n 
    = T(n/8) + a*n/4 + a*n/2 + a*n 
    = ... 
    = T(1) + ... + a*n/8 + a*n/4 + a*n/2 + a*n < 2*a*n

```

即O(n)