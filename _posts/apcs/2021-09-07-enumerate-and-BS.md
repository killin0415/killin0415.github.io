---
title: 枚舉 和 二分搜尋法
categories: cp list
layout: article
modified: 2021-09-06
---

###### tags: `APCScamp week1` `枚舉` `二分搜尋法`

### 枚舉

##### 顧名思義，就是將所有情況一一列舉出來，由於時間複雜度的關係，枚舉往往不是一個好的寫法，但如果透過剪枝、加速等動作，也許可以撈到部分分數或者是AC。


### 二分搜尋法
 
##### 一般搜尋有兩種方法：線性搜尋跟二分搜尋。線性搜尋就是很直覺的跑O(n)次找到我們要的目標，但配合其他程式碼，時間複雜度往往會來到O(n^2)甚至更高，因此我們需要一種新的搜尋法──二分搜尋法。
二分搜尋法透過以下操作：
1. 建立中心點
2. 如果比他大往右邊找，反之
3. 以此遞迴下去

透過分治的概念，達成二分搜的想法，完成O(logn)的搜尋演算法。

C++有內建函數 lower_bound(),upper_bound()等函數可以達成二分搜。

---
APCS 1月第三題：切割費用

有一根長度為 L 的棍子，你會把這個棍子切割 n 次。

假設一開始棍子左端放在數線上 0 的位置，棍子的右端放在數線上 L 的位置，每次的切割會給定一個介於 0 到 L 的數字表示要切個的位置，你要把穿過個這位置的棍子切成兩段，而所需的花費就等於所切割的棍子的長度。

第一行有兩個整數 n,L。

接下來 n 行每行有兩個整數 x,i，表示 x 位置被切過一刀，而這刀是全部的切割中的第 i 刀，保證 i 是介於 [1,n] 的整數且不會重複。

輸出一個整數表示總共的切割費用，答案可能超過 2^31 但不會超過2^60。

---
想法是透過set存取每一刀的位置，並二分搜上一刀的位置找出這一到所位於的線段，再將答案存起來

```cpp
#include<bits/stdc++.h>

using namespace std;

#define potato ios_base::sync_with_stdio(0); cin.tie(0); cout.tie(0)
#define pb emplace_back
#define mp make_pair
#define pii pair<int,int>

set<int> s;
vector<pii> vec;
int n,len;
int x,y;
int r,l,ans,key;

int main()
{
    potato;
    cin>>n>>len;
    for(int i=0;i<n;i++)
    {
        cin>>y>>x;//存取每一刀的位置
        vec.pb(x,y);
    }
    sort(vec.begin(),vec.end());//依刀的順序排序
    for(int i=0;i<n;i++)
    {
        key=vec[i].second;
        auto it = s.lower_bound(key);//找到上一刀的位置
        
        if(it != s.end()) r = *it;//如果有找到就將右界設在這一刀
        else r = len;//否則右界設在末端
        
        if(it != s.begin()) l = *--it;//如果有找到就將左界設在這一刀前面的位置 
        else l = 0;//否則設在最前端
        
        ans += r - l ;//兩邊相減求線段長度
        s.insert(key);//把現在這刀存起來   
    }
     cout<<ans;       
}
```

