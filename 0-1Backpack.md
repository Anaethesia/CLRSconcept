### 0-1背包问题
#### 问题介绍<br>
&emsp;&emsp;有N件物品和一个容量为V的背包，第i件物品的费用是w[i],价值为p[i]。求解将那些物品装到背包可使这些物品的费用总和不超过别抱容量，且总价值最大。这是一个经典的动态规划问题。<br>
&emsp;&emsp;首先设集合S={$$a_1,a_2,...a_n$$}表示n个物品的集合，A={$$a_i1,a_i2,...a_ik$$}表示问题的最优解集合。<br>
### 1、由动态规划问题可以证明原问题最优解一定包含子问题的最优解，证明了有最优子结构的性质。<br>
### 2、递归的去探索最优解的值<br>
&emsp;&emsp;对每个物品有两个选择，放入背包或者不放入背包。定义m[i][j]表示做出第i次选择后，已经装入容量j的最大价值。<br>
1.若放入物品i，m[i][j]=m[i-1][j-w[i]]+p[i]<br>
2.若不放入物品i，则m[i][j]=max{m[i-1][j],m[i-1][j-w[i]]+p[i]}<br>
### 实现代码<br>
```
   #include <iostream>
   #include <cstdio>
   #include <cstdlib>
using namespace std;
 
const int MAXN = 100;
 
int main()
{
    int n, V;
    int f[MAXN][MAXN];
    int w[MAXN], p[MAXN];
 
    while(cin>>n>>V)
    {
        if(n == 0 && V == 0)
            break;
        for(int i = 0; i <= n; ++i)
        {
            w[i] = 0, p[i] = 0;
            for(int j = 0; j <= n; ++j)
            {
                f[i][j] = 0;
            }
        }
        for(int i = 1; i <= n; ++i)
            cin>>w[i]>>p[i];
        for(int i = 1; i <= n; ++i)
        {
            for(int v = w[i]; v <= V; ++v)
            {
                f[i][v] = max(f[i-1][v], f[i-1][v-w[i]]+p[i]);
            }
        }
        cout<<f[n][V]<<endl;
    }
    return 0;
}
```
