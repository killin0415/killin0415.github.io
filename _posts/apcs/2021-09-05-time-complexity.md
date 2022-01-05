---
title: 時間複雜度
categories: cp list
layout: article
modified: 2021-09-05
---



###### tags:`APCScamp week1` `時間複雜度`  `貪婪演算法(greedy)`

---


#### 時間複雜度是一個估程式運行時間的方法，在解一些難題時，某些方法往往會因為時間問題而導致TLE，因此，估出一個好的時間複雜度是我們在優化程式的時候，不可或缺的判斷因素。

##### 定義O(K)其中K為此程式在最壞情況下所運行的時間，且將其寫成一個函式並將最高次項(或影響最大的項次令為K)，係數可以省略。
對於每個程式，其中基本操作如：算術運算、比較、賦值等皆為O(1)的操作，大部分時間都會落在迴圈、遞迴、及資料結構的操作上，如單層迴圈的複雜度為O(N)，雙層為O($N^2$) 等，若一個程式的運行時間為$3n^2 * n^2 * 2$，其最高項為$3n^2$ ，係數忽略後我們可以將其複雜度寫成O($N^2$)。此外，一般judge一秒可以跑10^8 筆整數運算，在寫的時候可以稍微估一下時間。

### 雙指針(two pointer,又名slide windows)
雙指針是一個能將O($N^2$)的程式改寫為O(N)的技巧，使用時機為當左指針遞增時，右指針只會遞增或遞減時使用。

---
EX:
給定一串字串，求最短的子字串包含所有小寫英文字母。
如果有多個最短的子字串，請輸出第一次出現的。如果這種子字串不存在，請輸出 QQ。

---

那這題我們可以發現，當我們定住左指針時，右指針一定會遞增，否則一定不會包含26個字母，因此這題就是一個典型的雙指針題型。

---
```cpp
#include<bits/stdc++.h>

using namespace std;

#define potato ios_base::sync_with_stdio(0); cin.tie(0); cout.tie(0)
#define pb emplace_back
#define mp make_pair
#define pii pair<int,int>

int arr[26]={0};
int l=0,r=0,cnt;//cnt紀錄目前英文字母相異的個數
int ans[2]={0,1000000};
string n;

int main()
{
    cin>>n; //字串長度
    cnt=1;
    arr[n[l]-'a']+=1;//記錄左指針的英文字母
    for (l=0;l<n.length();l++)
    { 
        while(r+1<n.length() && cnt<26)
        { 
            r++;
            if(arr[n[r]-'a']+1==1)
                cnt+=1;//如果未出現過就將cnt+1
            arr[n[r]-'a']+=1;
        }
        if (cnt<26)//如果跳出while迴圈但cnt仍<26代表r跑到底且l到r不存在26個相異字母
        {
            if (ans[1]-ans[0]+1!=1000001)//如果之前有找到存在26個相異字母的子字串則跳出while迴圈且輸出
                break;
            cout<<"QQ"<<endl;// 否則cout<<QQ並結束程式         
            return 0;
        }
        if(r-l<ans[1]-ans[0])//判斷當前字串的長度有沒有比上一個字串短並更新l跟r的數值
        {
            ans[1]=r;
            ans[0]=l;
        }
        arr[n[l]-'a']-=1;//將目前l所在字母的出現次數-1，如果移除後當前字串不存在26個字母，就將cnt-1
        if (arr[n[l]-'a']==0)
        {
            cnt-=1;
        }
    }
#     for (int i=ans[0];i<=ans[1];i++)//輸出字串
    {
        cout<<n[i]; 
    }
    cout<<endl;
    return 0;
}
```


---







