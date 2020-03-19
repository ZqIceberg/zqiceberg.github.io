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
