### 棋盘上的DFS

#### 啊哈解救小哈p81

代码版本v1

```cpp
#include <iostream>
#include <fstream>

using namespace std;

const int N = 20;

int g[N][N], book[N][N];
int minn = 0x3f;
int startx, starty, desx, desy;
int n, m;

int ne[4][2] = {{0,1}, {1,0}, {0,-1}, {-1,0}};

void dfs(int x, int y, int step)
{
	if (x == desx && y == desy)
	{
		if (step < minn) minn = step;
		return ;
	}

	for (int i = 0; i < 4; i++)
	{
		int xx = x + ne[i][0];
		int yy = y + ne[i][1];

		if (xx < 1 || xx > n || yy < 1 || yy > m) continue;

		if (g[xx][yy] == 0 && book[xx][yy] == 0)
		{
			book[xx][yy] = 1;

			dfs(xx, yy, step + 1);

			book[xx][yy] = 0;
		}
	}

	return ;
}

int main()
{
	freopen("migong02.in", "r", stdin);
	freopen("migong02.out", "w", stdout);

	cin >> n >> m;

	for (int i = 1; i <= n; i++)
		for (int j = 1; j <= m; j++)
			cin >> g[i][j];

	cin >> startx >> starty >> desx >> desy;

	book[startx][starty] = 1;
	dfs(startx, starty, 0);

	cout << minn << endl;

	return 0;
}

```

代码版本v2
```cpp
#include <iostream>

using namespace std;

const int N = 55;

int g[N][N];
bool st[N][N];
int n, m;
int sx, sy, ex, ey;
int minn = 0x3f3f;

void dfs(int x, int y, int step)
{
	if (x == ex && y == ey)
	{
		if (step < minn) minn = step;

		return ;
	}

	int dx[] = {0, 1, 0, -1}, dy[] = {1, 0, -1, 0};
	for (int i = 0; i < 4; i++)
	{
		int a = x + dx[i], b = y + dy[i];
		if (a < 1 || a > n || b < 1 || b > m) continue;
		if (g[a][b] == 1 || st[a][b]) continue;

		st[a][b] = true;
		dfs(a, b, step + 1);  //此方格可以走
		st[a][b] = false;
	}
}

int main()
{
	cin >> n >> m;
	for (int i = 1; i <= n; i++)
		for (int j = 1; j <= m; j++)
			cin >> g[i][j];

	cin >> sx >> sy >> ex >> ey;

	st[sx][sy] = true;
	dfs(sx, sy, 0);

	cout << minn << endl;

	return 0;
}
```

#### P2670 扫雷游戏

```cpp
#include <iostream>
#include <fstream>
#include <bits/stdc++.h>

using namespace std;

const int N = 110;

char g[N][N];  
char d[N][N];  //记录方案
int n, m;

int main()
{
	//freopen("saolei.in", "r", stdin);
	cin >> n >> m;

	for (int i = 1; i <= n; i++)
		for (int j = 1; j <= m; j++)
			cin >> g[i][j];

	for (int x = 1; x <= n; x++)
		for (int y = 1; y <= m; y++)
		{
			if (g[x][y] == '*')
			{
				d[x][y] = g[x][y];
				continue;
			}

			int sum = 0;
			for (int i = x - 1; i <= x + 1; i++ )
				for (int j = y - 1; j <= y + 1; j++)
				{
					if (i > 0 && i <= n && j > 0 && j <= m && g[i][j] == '*') sum++;
				}

			d[x][y] = '0' + sum;
		}

	for (int i = 1; i <= n; i++)
	{
		for (int j = 1; j <= m; j++)
			cout << d[i][j];
		puts("");
	}

	return 0;
}
```

#### P1219 [USACO1.5]八皇后 Checker Challenge

斜线，反斜线方向的表示

```cpp
#include <iostream>
#include <cstdio>

using namespace std;

const int N = 15;

int col[N], dg[N * 2], udg[N * 2];
char g[N][N];
int ans[N];
int n;
int res;

void dfs(int u)
{
	if (u == n)
	{
		res++;
		if (res <= 3)
		{
			for (int i = 0; i < n; i++)
				for (int j = 0; j < n; j++)
					if (g[i][j] == 'Q') printf("%d ", j + 1);
				
			puts("");
		}
	}
	
	for (int i = 0; i < n; i++)
	{
		if (!col[i] && !dg[u + i] && !udg[i - u + n])
		{
			col[i] = dg[u + i] = udg[i - u + n] = 1;
			g[u][i] = 'Q';
			dfs(u + 1);
			col[i] = dg[u + i] = udg[i - u + n] = 0;
			g[u][i] = '.';
		}
	}
}

int main()
{
	cin >> n;
	
	for (int i = 0; i < n; i++)
		for (int j = 0; j < n; j++)
			g[i][j] = '.';
		
	dfs(0);
	
	cout << res << endl;
	
	return 0;
}
```

#### T112203 蓝桥杯赛迷宫

```cpp
#include <iostream>
#include <cstdio>
#include <cstring>
#include <algorithm>
#include <fstream>

using namespace std;

const int N = 110;

char g[N][N];
int f[N][N];

int n, m;
bool wuxian;
int mstep, step = 1;
int dx[4] = {0, 1, 0, -1}, dy[4] = {1, 0, -1, 0};

void dfs(int x, int y)
{
    for (int i = 0; i < 4; i++)
    {
        int xx = x + dx[i], yy = y + dy[i];
        if (xx < 0 || xx > n-1 || yy < 0 || yy > m-1) continue ;
   
        if ((g[x][y] == 'L' && g[xx][yy] == 'Q') || (g[x][y] == 'Q' && g[xx][yy] == 'B') ||
            (g[x][y] == 'B' && g[xx][yy] == 'S') || (g[x][y] == 'S' && g[xx][yy] == 'L'))
            {
                if (f[xx][yy]) //可以无限循环
                {
                    wuxian = true;
                    return ;
                }

                f[xx][yy] = 1; step++;
                mstep = max(mstep, step);
                dfs(xx, yy);
                f[xx][yy] = 0; step--;
            }
    }

    return ;
}


int main()
{
    //freopen("LQBS3.in", "r", stdin);
    //freopen("LQBS3.out", "w", stdout);

    cin >> n >> m;
    for (int i = 0; i < n; i++) cin >> g[i];

    for (int i = 0; i < n; i++)
    {
        for (int j = 0; j < m; j++)
        {
            if (g[i][j] == 'L')
            {
                memset(f, 0, sizeof f);
                dfs(i, j);
            }

            if (wuxian)
            {
                cout << -1 << endl;
                return 0;
            }
        }
    }

    cout << mstep / 4 << endl;

    return 0;
}
```
