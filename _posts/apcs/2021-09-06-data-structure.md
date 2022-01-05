---
title: 資料結構
categories: cp list
layout: article
modified: 2021-09-06
---

###### tags: `APCScamp week1` `資料結構`

## 資料結構

##### 資料結構可以有效的幫我們存取資料，不同的題目需要不同的資料結構來存取，以下介紹幾個資料結構：stack、queue、deque、priority_queue、set、map等。c++有STL資料庫可以供我們使用這些資料結構，不需要自己手刻。

### stack

stack(堆疊)是一種後進先出的資料結構(LIFO)，需要支援以下操作：

1. stk.top()回傳stack中最上面的元素
2. stk.pop()將最上面的元素移除
3. stk.push()將元素移入stack裡
4. stk.size()回傳stack當前的大小
5. stk.empty()回傳一個bool值，如果當前stack為空回傳true，反之回傳false。

---
EX：
給定一個字串，若有任何相鄰的字元相同，就會被消掉。舉例來說，假設一開始的字串是 cabbab：

cabbab -> caab -> cb
第一步中中間有兩個 b 相鄰，消掉後又有兩個 a 相鄰，最後剩下 cb。

---
輸入只有一行S長度不會超過200000，只由小寫英文字母組成。
輸出字串運算後的結果。

---
只要相鄰兩個就刪除，乍看之下可以O(n^2)爆搜，可是這樣複雜度會爛掉，再仔細看一下，其實就是括號匹配的弱化版！

---
從字串由由左往右掃，每次把元素丟到stack裡，遇到相同的就pop掉
這樣就達到O(n)操作了！

---
以下是程式碼：

```cpp
#include<bits/stdc++.h>
using namespace std;

#define potato ios_base::sync_with_stdio(0); cin.tie(0); cout.tie(0)
#define pb emplace_back
#define mp make_pair
#define pii pair<int,int>

stack<int> stk;
string s,l="";

int main()
{
    potato;
    cin>>s;//輸入字串
    stk.push(s[0]);
    for(int i=1;i<s.length();i++)
    {
        if(!stk.empty() && stk.top() == s[i])//如果stack不為空且當目前輸入與stack最上層相同時，把上層元素pop掉。
            stk.pop();
        else    
            stk.push(s[i]);//否則把目前元素push進去。
    }
    while(!stk.empty())
    {
        l+=stk.top();//將stack字元加入l字串
        stk.pop();
    }
    reverse(l.begin(),l.end());//由於stack pop出來的是反過來的，所以要reverse。
    cout<<l<<'\n';//輸出並換行
}
```

---
### queue
queue(佇列)是一種先進先出的資料結構(FIFO)，需要支援以下操作：

1. que.front()回傳stack中最前面的元素
2. que.pop()將最上面的元素移除
3. que.push()將元素移入queue裡
4. que.size()回傳queue當前的大小
5. que.empty()回傳一個bool值，如果當前queue為空回傳true，反之回傳false。

---

EX：
每次進行：(1)將最上面的一張牌捨棄掉
        (2)將現在牌組最上面的一張牌移動至牌組最下方

---
假設第i次操作的時候捨棄掉的牌的編號為Ai，那草原戰士們想要你回答Q個問題，每次會問一個m，問你Am的值為多少？

---
輸入將有2行。第一行將有兩個正整數N，Q，代表有幾張牌和有幾個問題要回答。第二行有Q個數字，第i個是Mi，代表一次詢問。

請輸出Q行，每一行代表A Mi。

---
這題主要是將一堆卡牌進行以上的操作之後問第m次操作後所取出的卡牌，由於每次會取出最上面的卡牌並將下一張卡牌塞到下面，因此，我們可以使用queue的資料結構來處理這題。

```cpp
#include<bits/stdc++.h>

using namespace std;

#define potato ios_base::sync_with_stdio(0); cin.tie(0); cout.tie(0)
#define pb push_back
#define mp make_pair
#define pii pair<int,int>


int n,m,k;
queue<int> que;
vector<int> vec;

int main()
{
    potato;
    cin>>n>>m;
    for (int i=1;i<=n;i++)
    {
        que.push(i);
    }//初始化一組有n張的卡牌
    for (int i=0;i<n;i++)
    {
        vec.pb(que.front());//紀錄第i次操作所取出的卡牌
        que.pop();
        que.push(que.front());//取出後將下一張卡牌塞進queue的最後面
        que.pop();
    }
    for (int i=0;i<m;i++)
    {
        cin>>k;
        cout<<vec[k-1]<<'\n';//由於記錄第i次操作的vector是0 base，而題目的要求是1 base，所以將題目要求-1後輸出並換行
    }
}
}
```



### deque

deque(雙端佇列)是一種結合vector跟stack跟queue特性的一種容器，一個deque可以支援以下操作：

1. 新增、存取、刪除deque後端的資料
2. 新增、存取、刪除deque前端的資料

---
給定一串長度為n的序列，求i|i<=n-k+1中，所有[i,i+k]的最大值。
其中1<=k<=n<10^6

---
對於這題，雖然我們可以從0~n-k做一次max_element，但顯然這複雜度是糟的，因為max_element的複雜度為O(K)，整體複雜度為O(NK)，顯然是糟的。我們重複比較過多次相同的元素，即使他不能是我要的，因此我們需要一個資料結構來維持其"單調性"，那因為它是一個區間，我們沒有辦法透過priority_queue(後面會提到)來維護，因此我們可以透過頭尾控制這個區間，當目前的最大值超過我要的區間就pop掉，即為"隊列"，即為'單調對列'。

---
```cpp
#include<bits/stdc++.h>

using namespace std;


#define potato ios_base::sync_with_stdio(0); cin.tie(0); cout.tie(0)
#define pb emplace_back
#define mp make_pair
#define pii pair<int,int>
#define MOD 1000000009

int n,k;
int pep,tar,pivot;
deque<pii> vec;


int main()
{
    potato;
    cin>>n>>k;
    for(int i=0;i<n;i++)
    {
        if(!vec.empty() && vec.front().second+k<=i) vec.pop_front();//如果當前最大值已經超過目前的範圍就pop掉
        cin>>pep;
        while(!vec.empty() && vec.back().first<pep)vec.pop_back();//維護deque單調性(即使其保持大到小的排序)
        vec.push_back(mp(pep,i));
        if(i>=k-1)
            cout<<vec.front().first<<' ';//輸出前最大值

    }    
}
```

### 堆疊(heap、priority_queue)

heap又稱priority_queue(簡稱pq)，是一種能維護極值的資料結構，支援新增、刪除元素到pq頂端，其中每項操作皆為O(logn)，可以自定義比較函數或是使用內建函式達到維護最大值(或最小值)的作用。

通常pq都會跟著greedy演算法使用，因此例題到後面再講www。

### set、map

set(集合)，map(映射)皆為唯一性且排序過的資料結構，若不想要其唯一性也可使用multi_set，multi_map等(但multi_map很雞肋就是了)。

#### set 
顧名思義，可在裡面存取元素且不重複，支援以下操作：
1. s.insert(value)、s.erase(value) 新增、刪除資料
2. s.find(value) 回傳value所在的iterator，若不存在則回傳s.end()
3. s.count(value) 回傳value的個數，在set只回傳0 or 1

#### map
如同數學上的函數定義，由一個key映照到value上，其中一個key只會映照到一個value上，支援以下操作：
1. mp[key]=value 將key映照到value上
2. mp.erase(value) 同set
3. mp.find(value) mp.count(value)同set

一樣沒有例題(我真的找不到好的例題嗚嗚嗚)

