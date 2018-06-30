

<font size=5>　　假期结束了，小学期的时候想重新学习一遍算法，想把之前学过的一些简单的东西还有考研的东西都重新具体的写一下，方便理解，也给很多刚学习算法同学一点思路。涉及的算法都以《算法导论》的内容为准。<br>
##快速排序介绍（QuickSort）
&emsp;&emsp;快速排序是对冒泡排序的一种改进，运用的是分治策略的思想。<br>
* 基本思想　<br>
&emsp;&emsp;给定一个序列进行排序，先找到一个基准数，以此为标准，比它小的放在左边，比它小的放在右边。然后进行递归，知道每个部分为空或者只有一个元素的时候，快速排序结束。<br>
* 举例<br>
  方法：左右指针法<br>
　　在一段排序内我们设置一个标准，从左进行遍历，找到一个比它大的就停下，从右边遍历，找到一个比它小的就停下，将两个值进行交换就可以达到左边数比标准小，右边数比标准大的目的。<br>
　　当左指针和右指针相遇的时候表示的时候，说明左右标准已经建立完毕，递归实现左半部分和右半部分即可。<br>
  ![示例图片](https://img-blog.csdn.net/20170308204345872?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvUGF5c2hlbnQ=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)<br>
* 代码演示<br>
    <pre>#include <iostream>
using namespace std;
void quickSort(int a[],int beg,int end){
   //partition非递归实现，官方版
   if(beg >= end) return;
    int i = beg, j = end, x = a[(i + j)>>1],tmp =0;//这里基准值选了中间的值
    while(i <= j){//取等号，确保分成两个不相交区间
        while( a[i] < x) i++;
        while(a[j] > x) j--;
        if(i <= j ){
            tmp = a[i];
            a[i] = a[j];
            a[j] = tmp;
            i++;
            j--;
        }     
    }
    quickSort(a,beg,j);
    quickSort(a,i,end);
};
int main()
{
    int test[] = {34,6,4,5,1,2,6,90,7};
    quickSort(test,0,8);
    for(int i=0;i<9;i++){ 
        cout<<test[i]<<"  ";
    }
    cout<<endl;
    return 0;}
   </pre>