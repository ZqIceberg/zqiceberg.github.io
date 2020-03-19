#### 递归实现指数型枚举

```cpp
//使用vector，dfs()只有一个参数
#include <iostream>
#include <cstdio>
#include <vector>
#include <fstream>

using namespace std;

int n;
vector<int> chosen;

void dfs(int x)
{
    if (x == n + 1)
    {
        for (int i = 0; i < chosen.size(); i++)
            printf("%d ", chosen[i]);
        puts("");

        return ;
    }

    //不选x
    dfs(x + 1);

    //选x
    chosen.push_back(x);
    dfs(x + 1);
    chosen.pop_back();
}

int main()
{
    //freopen("meiju02.in", "r", stdin);
    //freopen("meiju02.out", "w", stdout);

    cin >> n;

    dfs(1);

    return 0;
}

```

```cpp
//dfs()两个参数，从0开始搜，用二进制记录状态
#include <iostream>
#include <cstdio>

using namespace std;

int n;

void dfs(int u, int state)
{
    if (u == n)
    {
        for (int i = 0; i < n; i++)
            if (state >> i & 1)
                cout << i + 1 << ' ';
        puts("");

        return ;
    }

    dfs(u + 1, state);
    dfs(u + 1, state | 1 << u);
}

int main()
{
    cin >> n;
    dfs(0, 0);

    return 0;
}
```


#### 递归实现组合型枚举
```cpp
#include <iostream>
#include <cstdio>
#include <vector>
#include <fstream>

using namespace std;

int n, m;
vector<int> chosen;

void dfs(int x)
{
    if (chosen.size() > m || chosen.size() + (n - x + 1) < m) return ;
    
    if (x == n + 1)
    {
        for (int i = 0; i < chosen.size(); i++)
            printf("%d ", chosen[i]);
        puts("");

        return ;
    }
    //选x
    chosen.push_back(x);
    dfs(x + 1);
    chosen.pop_back();
    
    
    //不选x
    dfs(x + 1);
    

}

int main()
{
    //freopen("meiju03.in", "r", stdin);
    //freopen("meiju03.out", "w", stdout);

    cin >> n >> m;
    
    dfs(1);
    
    return 0;
}
```

```cpp
#include <iostream>
#include <cstdio>

using namespace std;

int n, m;

void dfs(int u, int sum, int state)
{
    if (sum > m || sum + n - u < m) return ;
    
    if (sum == m)
    {
        for (int i = 0; i < n; i++)
            if (state >> i & 1)
                cout << i + 1 << ' ';
        puts("");
        
        return ;
    }
    
    dfs(u + 1, sum + 1, state | 1 << u);
    dfs(u + 1, sum, state);
}

int main()
{
    cin >> n >> m;
    dfs(0, 0, 0);
    
    return 0;
}
```

#### P1036 选数
```cpp
#include <iostream>
#include <cstdio>
#include <vector>

using namespace std;

const int N = 30;

vector<int> chosen;
int q[N];
int n, k;
int res;

bool is_prime(int x)
{
    //试除法判断质数模板
    if (x < 2) return false;
    for (int i = 2; i <= x / i; i++)
        if (x % i == 0) return false;

    return true;
}

void dfs(int x)
{
    if (chosen.size() > k || chosen.size() + (n - x + 1) < k) return;   //剪枝，如果已经找到的个数，大于k个，或者剩下的都找来也不够k个就返回

    if (x == n + 1)    //问题边界
    {
        int sum = 0;
        for (int i = 0; i < chosen.size(); i++) sum += chosen[i];

        if (is_prime(sum))
        {
            res++;
            //for (int i = 0; i < idx; i++) printf("%d ", chosen[i]);
            //puts("");
        }

        return ;
    }

    //求解子问题，不选这个数
    dfs(x + 1);

    //求解子问题，选这个数
    chosen.push_back(q[x]);
    dfs(x + 1);
    chosen.pop_back();
}

int main()
{
    cin >> n >> k;
    for (int i = 1; i <= n; i++) cin >> q[i];

    dfs(1);  //从第一个数开始找

    cout << res << endl;

    return 0;
}
```

```cpp

```

```cpp
#include <iostream>
#include <cstdio>

using namespace std;

const int N = 25;

int q[N];
int n, m;

int is_prime(int x)
{
    if (x < 2) return 0;

    for (int i = 2; i <= x / i; i++)
        if (x % i == 0) return 0;
   
    return 1;
}

int dfs(int left_choose, int already_choose, int u)
{
    if (left_choose == 0) return is_prime(already_choose);

    int sum = 0;
    for (int i = u; i <= n; i++)
            sum += dfs(left_choose - 1, already_choose + q[i], i + 1);

    return sum;
}

int main()
{
    cin >> n >> m;
    for (int i = 1; i <= n; i++) scanf("%d", &q[i]);
    
    printf("%d\n", dfs(m, 0, 1));
    
    return 0;
}    

```

#### P1157 组合的输出
```cpp
#include <iostream>
#include <cstdio>
#include <vector>

using namespace std;

vector<int> A;
vector<vector<int> > ANS;
int n, m;

void dfs(int x)  //当前选择的数是x
{
    if (A.size() > m || A.size() + (n - x + 1) < m) return ;  //lyd大神的指引

    if (x == n + 1)
    {
        ANS.push_back(A);
        return ;
    }

    //不选x
    dfs(x + 1);

    //选x
    A.push_back(x);
    dfs(x + 1);
    A.pop_back();
}

int main()
{
    cin >> n >> m;

    dfs(1); //从1开始

    vector<vector<int> >:: iterator iter;
    for (iter = ANS.end() - 1; iter >= ANS.begin(); iter--)
    {
        vector<int> T = *iter;
        for (int i = 0; i < T.size(); i++) printf("%3d", T[i]);
        puts("");
    }
   
    return 0;
}
```

```cpp
#include <iostream>
#include <cstdio>

using namespace std;

int a[25];
bool st[25];
int n, m;

void dfs(int k)
{
    if (k > m)
    {
        for (int i = 1; i <= m; i++) printf("%3d", a[i]);
        puts("");

        return ;
    }

    int t = a[k - 1];
    for (int i = t; i <= n; i++)
    {
        if (st[i]) continue;

        a[k] = i;
        st[i] = true;
        dfs(k + 1);
        st[i] = false;
    }
}

int main()
{
    cin >> n >> m;

    a[0] = 1;
    dfs(1);

    return 0;
}

```

```cpp
#include <iostream>
#include <cstdio>

using namespace std;

int n, m;
int a[25];

void dfs(int k)  //当前找了k个数
{
    if (k > m)  //超过m个，找够了，输出，return上一层
    {
        for (int i = 1; i <= m; i++)
            printf("%3d", a[i]);
        puts("");

        return ;
    }

    int t = a[k - 1];
    for (int i = t + 1; i <= n; i++)  //当前的数，要大于前一个找到的数
    {
        a[k] = i;
        dfs(k + 1);
    }
}

int main()
{
    cin >> n >> m;

    dfs(1);  //当前找了1个数

    return 0;
}
```



#### 递归实现排列型枚举
```cpp
#include <iostream>
#include <cstdio>
#include <fstream>

using namespace std;

int order[20];
bool chosen[20];
int n;

void dfs(int k)
{
    if (k == n + 1)
    {
        for (int i = 1; i <= n; i++)
            cout << order[i] << ' ';
        puts("");
    }
    
    for (int i = 1; i <= n; i++)
    {
        if (chosen[i]) continue;
        
        order[k] = i;
        chosen[i] = true;
        
        dfs(k + 1);
        
        chosen[i] = false;
    }
}

int main()
{
    //freopen("meiju03.in", "r", stdin);
    //freopen("meiju03.out", "w", stdout);

    cin >> n;
    
    dfs(1);
    
    return 0;
}

```

```cpp
#include <iostream>
#include <cstdio>

using namespace std;

int order[20];
bool chosen[20];
int n;

void dfs(int k)
{
    if (k == n)
    {
        for (int i = 0; i < n; i++)
            cout << order[i] << ' ';
        puts("");
    }
    
    for (int i = 1; i <= n; i++)
    {
        if (chosen[i]) continue;
        
        order[k] = i;
        chosen[i] = true;
        
        dfs(k + 1);
        
        chosen[i] = false;
    }
}

int main()
{
    cin >> n;
    
    dfs(0);
    
    return 0;
}

```

```cpp
#include <iostream>
#include <cstdio>
#include <vector>

using namespace std;

int n;
vector<int> path;

void dfs(int u, int state)
{
    if (u == n)
    {
        vector<int>::iterator i;
        for (i = path.begin(); i < path.end(); i++)
            cout << *i << ' ';
        puts("");
    }
    
    for (int i = 0; i < n; i++)
        if (!(state >> i & 1))
        {
            path.push_back(i + 1);
            dfs(u + 1, state | 1 << i);
            path.pop_back();
        }
}

int main()
{
    cin >> n;
    dfs(0, 0);
    
    return 0;
}

```
