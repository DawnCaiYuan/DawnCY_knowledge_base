## 线段树

使用场景:  
与区间查询更新有关可以考虑使用。  


实现：

```c++
//#include<bits/stdc++.h>
#include <algorithm>
#include <cctype>
#include <cmath>
#include <cstring>
#include <iostream>
#include <queue>
#include <vector>
using namespace std;
#define PI acos(-1.0)
const int INF = 0x4f4f4f4f;
const int NINF = -INF - 1;
typedef long long ll;

const int MAXN = 100005;
int n;
int treeLen;
int arr[MAXN];
int treeSum[MAXN];
int treeMax[MAXN];
int treeMin[MAXN];

// 树的构建
int constTreeSum(int st, int ed, int idx){
    treeLen = max(treeLen, idx);
    if(st == ed) {treeSum[idx] = arr[st]; return arr[st];}
    int mid = st + (ed - st)/2;
    return treeSum[idx] = constTreeSum(st, mid, idx*2+1) + constTreeSum(mid+1, ed, idx*2+2);
}

int constTreeMax(int st, int ed, int idx){
    treeLen = max(treeLen, idx);
    if(st == ed) {treeMax[idx] = arr[st]; return arr[st];}
    int mid = st + (ed - st)/2;
    return treeMax[idx] = max(constTreeMax(st, mid, idx*2+1),constTreeMax(mid+1, ed, idx*2+2));
}

int constTreeMin(int st, int ed, int idx){
    treeLen = max(treeLen, idx);
    if(st == ed) {treeMin[idx] = arr[st]; return arr[st];}
    int mid = st + (ed - st)/2;
    return treeMin[idx] = min(constTreeMin(st, mid, idx*2+1),constTreeMin(mid+1, ed, idx*2+2));
}

// 查询操作
int queryTreeSum(int st, int ed, int l, int r, int idx){
    if(st <= l && r <= ed) {return treeSum[idx];}
    if(ed < l || st > r || l > r) {return 0;}
    int mid = l + (r - l)/2;
    return queryTreeSum(st, ed, l, mid, idx*2+1) + 
                queryTreeSum(st, ed, mid+1, r, idx*2+2);
}

int queryTreeMax(int st, int ed, int l, int r, int idx){
    if(st <= l && r <= ed) {return treeMax[idx];}
    if(ed < l || st > r || l > r) {return NINF;}
    int mid = l + (r - l)/2;
    return max(queryTreeMax(st, ed, l, mid, idx*2+1), 
                queryTreeMax(st, ed, mid+1, r, idx*2+2));
}

int queryTreeMin(int st, int ed, int l, int r, int idx){
    if(st <= l && r <= ed) {return treeMin[idx];}
    if(ed < l || st > r || l > r) {return INF;}
    int mid = l + (r - l)/2;
    return min(queryTreeMin(st, ed, l, mid, idx*2+1),
                 queryTreeMin(st, ed, mid+1, r, idx*2+2));
}

/** 更新操作, 只在tree中更新，还需要在原数组更新
 * loc: 在原数组中更新的位置
 * num: 新值
 */
void updateTreeSum(int loc, int num, int l, int r, int idx){
    if(l <= loc && loc <= r){
        if(l == r) {treeSum[idx] = num; return;}
        int mid = l + (r - l) / 2;
        updateTreeSum(loc, num, l, mid, idx * 2 + 1);
        updateTreeSum(loc, num, mid+1, r, idx * 2 + 2);
        treeSum[idx] = treeSum[idx*2+1] + treeSum[idx*2+2];
    }
}

void updateTreeMax(int loc, int num, int l, int r, int idx){
    if(l <= loc && loc <= r){
        if(l == r) {treeMax[idx] = num; return;}
        int mid = l + (r - l) / 2;
        updateTreeMax(loc, num, l, mid, idx * 2 + 1);
        updateTreeMax(loc, num, mid+1, r, idx * 2 + 2);
        treeMax[idx] = max(treeMax[idx*2+1], treeMax[idx*2+2]);
    }
}

void updateTreeMin(int loc, int num, int l, int r, int idx){
    if(l <= loc && loc <= r){
        if(l == r) {treeMin[idx] = num; return;}
        int mid = l + (r - l) / 2;
        updateTreeMin(loc, num, l, mid, idx * 2 + 1);
        updateTreeMin(loc, num, mid+1, r, idx * 2 + 2);
        treeMin[idx] = min(treeMin[idx*2+1], treeMin[idx*2+2]);
    }
}

// 集成，方便使用
void constTree(){
    constTreeSum(0, n-1, 0);
    constTreeMax(0, n-1, 0);
    constTreeMin(0, n-1, 0);
    treeLen++;
}

int querySum(int a, int b){
    return queryTreeSum(a, b, 0, n-1, 0);
}

int queryMax(int a, int b){
    return queryTreeMax(a, b, 0, n-1, 0);
}

int queryMin(int a, int b){
    return queryTreeMin(a, b, 0, n-1, 0);
}

void updateTree(int loc, int num){
    updateTreeSum(loc, num, 0, n-1, 0);
    updateTreeMax(loc, num, 0, n-1, 0);
    updateTreeMin(loc, num, 0, n-1, 0);
    arr[loc] = num;
}

void printTree(){
    for(int i = 0 ;i < treeLen; i++){
        cout<<treeSum[i]<<' '<<treeMax[i]<<' '<<treeMin[i]<<endl;
    }
}

int main(){
    int opr, a, b, loc, num, flag;
    while(true){
        cout<<" 1:construction a new tree.\n 2:querySum.\n 3:queryMax.\n 4:queryMin\n 5:update\n 6:exit\n";
        opr = 0, a = 0, b = 0, loc = 0, num = 0, flag = false;
        cin>>opr;
        switch (opr)
        {
        case 1:
            cout<<"press length of the arr!\n";
            cin>>n;
            treeLen = 0;
            cout<<"cin the nums!\n";
            for(int i = 0; i < n; i++){
                cin>>arr[i];
            }
            constTree();
            printTree();
            break;
        case 2:
            cout<<"press the lowerbound a and the upperbound b!\n";
            cin>>a>>b;
            cout<<querySum(a, b)<<endl;
            break;
        case 3:
            cout<<"press the lowerbound a and the upperbound b!\n";
            cin>>a>>b;
            cout<<queryMax(a, b)<<endl;
            break;
        case 4:
            cout<<"press the lowerbound a and the upperbound b!\n";
            cin>>a>>b;
            cout<<queryMin(a, b)<<endl;
            break;
        case 5:
            cout<<"press the location and the value!\n";
            cin>>loc>>num;
            updateTree(loc, num);
            break;  
        case 6:
            flag = true;
            break; 
        default:
            break;
        }
        if(flag){
            cout<<"welcome"<<endl;
            break;
        }
    }
    system("pause");
    return 0;
}
/* test case
1
7
7 4 6 2 9 3 1
2
0 3
3
0 3
4
0 3
5
0 1
2
0 3
3
0 3
4
0 3
*/
```

