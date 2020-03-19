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
