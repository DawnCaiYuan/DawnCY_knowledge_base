## 搜索
### BFS
1. 广度优先，每次都执行所有可行选择，常利用队列实现，且题目往往涉及最小步数等关键词。  
2. 模板：  
```c++

```
3. 相关题目
    1. Dungeon Master(POJ_2251)
        - 链接：  
        https://vjudge.net/problem/POJ-2251
        - 题意：  
        三维空间逃亡的最少时间。
        - 思路：  
        普通BFS
        - 代码：  
        ```c++
        //#include<bits/stdc++.h>
        #include <algorithm>
        #include <cctype>
        #include <cmath>
        #include <cstring>
        #include <string>
        #include <iostream>
        #include <queue>
        #include <vector>
        using namespace std;
        #define PI acos(-1.0)
        #define PB push_back
        const int INF = 0x4f4f4f4f;
        const int NINF = -INF - 1;
        typedef long long ll;

        struct point{
            int x, y, z;
            point(int a, int b, int c){x = a; y = b; z = c;}
            point(){x = 0; y = 0; z = 0;}
            bool operator ==(point a){
                return a.x == x && a.y == y && a.z == z;
            }
        };

        const int MXN = 35;
        int L, R, C;
        char dungeon[MXN][MXN][MXN];
        bool vis[MXN][MXN][MXN];
        int times[MXN][MXN][MXN];
        int sx, sy, sz, ex, ey, ez;
        int dx[6] = {1, 0, -1, 0, 0, 0}, dy[6] = {0,1, 0 ,-1, 0, 0}, dz[6] = {0,0 , 0, 0, 1, -1};

        void bfs(){
            point s(sx,sy,sz), e(ex, ey, ez);
            queue<point> q;
            q.push(s);
            times[sx][sy][sz] = 0;
            while(q.size()){
                point cur = q.front(); q.pop();
                if(cur == e){
                    return;
                }
                for(int i = 0; i < 6; i++){
                    int nx = cur.x + dx[i], ny = cur.y + dy[i], nz = cur.z + dz[i];
                    if(nx >= 0 && nx < L && ny >= 0 && ny < R && nz >= 0 && nz < C && dungeon[nx][ny][nz] != '#' && vis[nx][ny][nz] == false){
                        vis[nx][ny][nz] = true;
                        q.push(point(nx, ny, nz));
                        times[nx][ny][nz] = times[cur.x][cur.y][cur.z] + 1;
                    }
                }
            }
        }

        int main(){
            while(true){
                cin>>L>>R>>C;
                if(L == 0 && R == 0 && C == 0){break;}
                for(int i = 0; i < L; i++){
                    for(int j = 0; j < R; j++){
                        for(int k = 0; k < C; k++){
                            cin>>dungeon[i][j][k];
                            if(dungeon[i][j][k] == 'S'){
                                sx = i; sy = j; sz = k;
                            }
                            if(dungeon[i][j][k] == 'E'){
                                ex = i; ey = j; ez = k;
                            }
                        }
                    }
                }
                memset(vis, false, sizeof(vis));
                memset(times, -1, sizeof(times));
                bfs();
                if(times[ex][ey][ez] == -1){
                    cout<<"Trapped!"<<endl;
                }else{
                    cout<<"Escaped in "<<times[ex][ey][ez]<<" minute(s)."<<endl;
                }
            }
            return 0;
        }
        ```


### DFS
1. 深度优先，失败才返回，常利用递归实现。
2. 模板：  
```c++
const int MXN = 4f4f4f4f;
bool vis[MXN][MXN];
int dx[4] = {1, -1, 0, 0}, dy[4] = {0, 0, 1, -1};
bool limit[MXN][MXN];

int dfs(int x, int y){
    if(满足退出条件){
        退出;
    }
    // 进入新路口
    for(int i = 0; i < 4; i++){
        int nx = x + dx[i], ny = y + dy[i];
        if(满足限制条件){
            dfs(nx, ny);
        }
    }
}
```
3. 相关题目
    1. 棋盘问题(POJ_1321)
        - 链接：  
        https://vjudge.net/problem/POJ-1321
        - 题意：  
        在一个给定形状的棋盘（形状可能是不规则的）上面摆放棋子，棋子没有区别。要求摆放时任意的两个棋子不能放在棋盘中的同一行或者同一列，请编程求解对于给定形状和大小的棋盘，摆放k个棋子的所有可行的摆放方案C。  
        - 思路：  
        普通二维DFS
        - 代码：
        ``` c++
        //#include<bits/stdc++.h>
        #include <algorithm>
        #include <cctype>
        #include <cmath>
        #include <cstring>
        #include <string>
        #include <iostream>
        #include <queue>
        #include <vector>
        using namespace std;
        #define PI acos(-1.0)
        #define PB push_back
        const int INF = 0x4f4f4f4f;
        const int NINF = -INF - 1;
        typedef long long ll;

        const int MXN = 10;
        int n, k;
        char chs[MXN][MXN];
        bool vis[MXN][MXN];
        int judge[MXN]; // 第i列是否能下
        ll ans;

        void dfs(int x, int cc){
            if(cc == 0){
                ans++;
                return;
            }
            for(int i = x; i < n; i++){
                for(int j = 0; j < n; j++){
                    if(judge[j] && chs[i][j] == '#' && vis[i][j] == false){
                        judge[j] = 0; vis[i][j] = true;
                        dfs(i+1, cc-1);
                        judge[j] = 1; vis[i][j] = false;
                    }
                }
            }
        }

        int main(){
            while(true){
                cin>>n>>k;
                if(n == -1 && k == -1){break;}
                for(int i = 0; i < n; i++){
                    for(int j = 0; j < n; j++){
                        cin>>chs[i][j];
                    }
                }
                memset(vis, false, sizeof(vis));
                memset(judge, 1, sizeof(judge));
                ans = 0;
                dfs(0, k);
                cout<<ans<<endl;
            }

            return 0;
        }
        ```
    2. 

### 二分
1. 前提：  
数组要求已经有序(这里默认非递减)  
2. 类型： 
   - 普通二分(=。是否存在一个i，s.t. arr[i] == target)  
    ```c++
    int bs(vector<int>& arr, int l, int r, int target){
        while(l <= r){
            int mid = l + (r - l)/2;
            if(arr[mid] == target){
                return mid;
            }else if(arr[mid] < target){
                l = mid + 1;
            }else{
                r = mid - 1;
            }
        }
        return -1;
    }
    ```

    - lower_bound(>=。是否存在一个i，s.t. arr[j] >= target当且仅当j>=i; arr[j] < target当且仅当j < i)  
    如：求arr数组中有多少元素小于target?  
    num = lower_bound(arr.begin(), arr.end(), target) - arr.begin();
    ```c++
    int lbs(vector<int>& arr, int target){
        int l = 0, r = arr.size() - 1;
        while(l <= r){
            int mid = l + (r-l)/2;
            if(arr[mid] < target){
                l = mid + 1;
            }else{
                r = mid - 1;
            }
        }
        return r + 1;
    }
    ```

    - upper_bound(>。是否存在一个i，s.t. arr[j] > target当且仅当j>=i; arr[j] <= target当且仅当j < i)  
    如：求arr数组中有多少元素小于等于target?  
    num = upper_bound(arr.begin(), arr.end(), target) - arr.begin();
    ``` c++
    int rbs(vector<int>& arr, int target){
        int l = 0, r = arr.size() - 1;
        while(l < = r){
            int mid = l + (r - l) / 2;
            if(arr[mid] <= target){
                l = mid + 1;
            }else{
                r = mid - 1;
            }
        }
        return l;
    }
    ```
3. 相关题目  
    1. New Year and Ascent Sequence(CF_R1500)
        - 链接：  
        https://codeforces.com/contest/1284/problem/B
        - 题意：  
        数组arr中若存在一组i,j(i < j)使得arr[i] < arr[j]即为有效。现给n组数组，可重复选取两个数组拼接，则共有n^2个拼接数组([1,2] + [3, 4] = [1, 2, 3, 4])，问这n^2个拼接数组中有多少个有效。  
        - 思路：  
        若p有效或者q有效，则p+q或者q+p必定有效。
        若p无效且q无效，则p中最小值小于q中最大值时p+q有效。
        为此，需要维护n个数组中哪些本身就有效，以及他们的最小最大值。最后，ans = 有效数组数量 * n + 非有效数组的拼接中有效数组数量 + 非有效数组数量 * 有效数组数量;
        - 代码：
        ``` c++
        //#include<bits/stdc++.h>
        #include <algorithm>
        #include <cctype>
        #include <cmath>
        #include <cstring>
        #include <string>
        #include <iostream>
        #include <queue>
        #include <vector>
        using namespace std;
        #define PI acos(-1.0)
        #define PB push_back
        const int INF = 0x4f4f4f4f;
        const int NINF = -INF - 1;
        typedef long long ll;
        int main(){
            ios_base::sync_with_stdio(0);
            cin.tie(nullptr);
            int n; cin>>n;
            ll ans = 0; ll zq = 0;
            vector<int> mx;
            vector<int> mn;
            for(int j = 0; j < n; j++){
                int ln; cin>>ln;
                int tmx = NINF, tmn = INF, tmp = 0, flag = 0;
                for(int i= 0; i < ln; i++){
                    cin>>tmp;
                    tmx = max(tmp, tmx);
                    tmn = min(tmp, tmn);
                    if(tmp > tmn){
                        flag = 1;
                    }
                }
                if(flag){
                    ans += n;
                    zq++;
                }else{
                    mx.PB(tmx); mn.PB(tmn);
                }
            }
            sort(mx.begin(), mx.end());
            sort(mn.begin(), mn.end());

            for(int i = 0; i < mx.size(); i++){
                int j = lower_bound(mn.begin(), mn.end(), mx[i]) - mn.begin();
                ans += j;
                ans += zq;
            }
            cout<<ans<<endl;
            //system("pause");
            return 0;
        }
        ```

    


