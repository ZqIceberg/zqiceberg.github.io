#### A. Three Friends

*Description*

三个数，a，b，c，每个数可以+1，-1，或者不变，求两个数之差的绝对值之和最小

*solution*

暴力枚举三重for循环

*Code*

```cpp
#include <bits/stdc++.h>

using namespace std;

int T;
int x, y, z;

int main()
{
	scanf("%d", &T);

	while (T--)
	{
		scanf("%d%d%d", &x, &y, &z);
		
		int ans = 2e9;
		for (int i = x - 1; i <= x + 1; i++)
			for (int j = y - 1; j <= y + 1; j++)
				for (int k = z - 1; k <= z + 1; k++)
				{
					ans = min(ans, abs(i-j) + abs(i-k) + abs(j-k));
				}

		printf("%d\n", ans);
	}

	return 0;
}
```



#### B. Snow Walking Robot

*Description*

扫雪机器人从(0, 0)出发，除了起点可以经过两次，其他点只能经过一次。给你一串命令，上下左右组成，求一个合法命令集合，长度最长。如果没有合法的命令集合，就输出0。这些命令可以打乱顺序

*solution*

这句话挺关键的“Note that you can choose **any** order of remaining instructions (you don't need to minimize the number of swaps or any other similar metric).” 这句话，直接让你无视命令串的顺序，直接去观察问题的本质。

如果只有上下命令，上一个下一个，再多就非法

如果只有左右命令，上一个下一个，再多就非法

如果既有上下左右的命令，上下，左右要配对出现，然后可以顺时针饶一圈的路线，肯定不会走重复的。

*Code*

```cpp
#include <bits/stdc++.h>

using namespace std;

int T;
string s;

int main()
{
	cin >> T;

	while (T--)
	{
		cin >> s;

		int L = 0, R = 0, U = 0, D = 0;
		for (int i = 0, len = s.size(); i < len; i++)
		{
			if (s[i] == 'L') L++;
			if (s[i] == 'R') R++;
			if (s[i] == 'U') U++;
			if (s[i] == 'D') D++;
		}

		int minn1 = min(L, R);
		int minn2 = min(U, D);

		if (minn1 == 0 && minn2 == 0)
		{
			cout << 0 << endl;
			puts("");
		}
		else if (minn1 == 0 && minn2 != 0)
		{
			cout << 2 << endl;
			cout << "UD" << endl;
		}
		else if (minn1 != 0 && minn2 == 0)
		{
			cout << 2 << endl;
			cout << "LR" << endl;
		}
		else
		{
			cout << 2 * minn1 + 2 * minn2 << endl;
			for (int i = 0; i < minn1; i++) cout << 'L';
			for (int i = 0; i < minn2; i++) cout << 'U';
			for (int i = 0; i < minn1; i++) cout << 'R';
			for (int i = 0; i < minn2; i++) cout << 'D';
			puts("");
		}
	}

	return 0;
}
```



#### C. Yet Another Broken Keyboard

*Description*

一个字符串，其中有一些字符是无法打印的，求可以打印的子串个数之和
$$
\frac{n * (n + 1)}{2}
$$
*solution*

two pointer遍历一遍，每遇到一个合法子串，就把这个子串的子串个数加起来

*Code*

```cpp
#include <bits/stdc++.h>

using namespace std;

int n, k;
string s;
map<char, int> mp;

int main()
{
	cin >> n >> k;
	cin >> s;

	while (k--)
	{
		char c;
		cin >> c;

		mp[c] = 1;
	}

	long long ans = 0;
	for (int i = 0, j, len = s.size(); i < len; i++)
	{
		for (j = i; j < len; j++)
			if (mp.find(s[j]) == mp.end()) break;
		
		//printf("--%d %d\n", i, j);
		int n = j - i;
		ans += 1LL * n * (n + 1) / 2;
		i = j;
	}
	
	printf("%lld\n", ans);

	return 0;
}
```



#### D. Remove One Element

*Description*

求一个序列的最长上升子序列，你可以删除一个点后再求，或者不删除。

*solution*

如果不删除点，就是LIC，如果删除一个点i，就转化为，以i - 1为终点的最长上升子序列 + 以i+1为起点的最长上升子序列 之和

*Code*

```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 2e5 + 10;

int n;
int a[N];
int dp1[N], dp2[N];

int main()
{
	scanf("%d", &n);

	for (int i = 1; i <= n; i++) scanf("%d", &a[i]);

	dp1[1] = 1;
	for (int i = 2; i <= n; i++)
		if (a[i] > a[i - 1]) dp1[i] = dp1[i - 1] + 1;
		else dp1[i] = 1;

	dp2[n] = 1;
	for (int i = n - 1; i >= 1; i--)
		if (a[i] < a[i + 1]) dp2[i] = dp2[i + 1] + 1;
		else dp2[i] = 1;

	//for (int i = 1; i <= n; i++) printf("%d ", dp2[i]);
	//puts("");

	int ans = -1;
	for (int i = 1; i <= n; i++)
	{
		ans = max(ans, dp1[i]);
		
		int t = 0;
		if (i - 1 >= 1 && i + 1 <= n && a[i - 1] < a[i + 1]) 
		{
			t += dp1[i - 1];
			t += dp2[i + 1];
		}
		ans = max(ans, t);
	}

	printf("%d\n", ans);
	
	return 0;
}
```



#### E. Nearest Opposite Parity

*Description*

给出一个数组a[N], 每个点可以向左跳，或者向右跳a[i]的距离，当他遇到奇偶性不相同的点的时候停下来，如果无法遇到奇偶性不同的点，就输出-1， 否则输出最小步数

*solution*

难度: *1900

这个难度的，就基本要啃好几个小时，才理解。有几个知识点：

- 反向建边，
- 哪些点作为起始点入队，
- bfs可以解决最短路问题

```
10
4 5 7 6 7 5 4 4 6 4
```

预处理环节，a[1] = 4, 可以向右跳到a[5] = 7，奇偶性不同。让1入队，dist[1] = 1，表示，1走一步就可实现奇偶不同。然后建立一条边 5 -> 1。

这样bfs的时候，1出队，看谁能跳到1上，比如这些点是t，就看dist[t]是不是1，如果是1，说明t有一步走到结果的路线，否则，dist[t] = dist[1] + 1，表示t走两步能到达奇偶不同。

结合代码理解会容易很多，特别关键的是，理解初始化时，第一批入队的点

*Code*

```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 2e5 + 10;

int h[N], e[N * 2], ne[N * 2], idx;
int d[N]; //维护距离
int a[N];
int n;

void add(int a, int b)
{
	e[idx] = b, ne[idx] = h[a], h[a] = idx++;
}

int main()
{
	scanf("%d", &n);

	for (int i = 1; i <= n; i++) scanf("%d", &a[i]);
	
	memset(h, -1, sizeof h);
	memset(d, -1, sizeof d);

	queue<int> q;
	for (int i = 1; i <= n; i++)
	{
		if (i - a[i] >= 1)
		{
			if (a[i] % 2 != (a[i - a[i]]) % 2)
			{
				d[i] = 1;
				q.push(i);  //作为起点
			}
			add(i - a[i], i);
		}

		if (i + a[i] <= n)
		{
			if (a[i] % 2 != (a[i + a[i]]) % 2)
			{
				d[i] = 1;
				q.push(i);
			}
			add(i + a[i], i);
		}
	}

	while (!q.empty())
	{
		int t = q.front();
		q.pop();

		for (int i = h[t]; i != -1; i = ne[i])
		{
			int j = e[i];
			if (d[j] == -1)
			{
				d[j] = d[t] + 1;
				q.push(j);
			}
		}
	}
	
	for (int i = 1; i <= n; i++) printf("%d ", d[i]);
	puts("");

	return 0;
}
```



#### F. Two Bracket Sequences

*Description*

两个字符串s1, s2，由'('  ')' 组成，求一个合法字符串，s1和s2是其子串，字符可以不连续。

*solution*

这是一道dp题，*2200，威力特别大，“有点竞赛题的意思，综合锻炼代码能力”

dp[i,j,k]，s1前i个字符，s2前j个字符，左括号比右括号多k个

关于合法的左右括号匹配规则，(((())，左括号的个数要大于等于右括号的个数，才是合法的。如果左括号多，那直接就死了，如果是左括号多，后面可以补右括号补成合法的

还有一个难点，需要你输出合法方法，需要一个Node pre[i, j, k]，维护是从哪个点走过来的

还有一个难点，s1，s2都用完了还不合法后面还要补右括号，补多少右括号，是一个问题。就是看dp[n, m, k]遍历一遍k层，左括号比右括号多几个，就表明后面需要补多少个右括号

题目需要输出长度最短的s，那么补最少的右括号最好的。

深刻认识了*2200的难度

Code

```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 210;

struct Node
{
	int up, down, k;
	char ch;

	Node(){}

	Node(int a, int b, int c, char d)
	{
		up = a;
		down = b;
		k = c;
		ch = d;
	}
};

Node pre[N][N][2 * N];
int dp[N][N][2 * N];
char s1[N], s2[N];

bool checkmin(int &x, int y)
{
	if (x == -1 || y < x)
	{
		x = y;
		return true;
	}

	return false;
}

int main()
{
	scanf("%s%s", s1 + 1, s2 + 1);

	int len1 = strlen(s1 + 1), len2 = strlen(s2 + 1);
	
	memset(dp, -1, sizeof dp);
	dp[0][0][0] = 0;

	for (int i = 0; i <= len1; i++)
		for (int j = 0; j <= len2; j++)
			for (int k = 0; k < 2 * N; k++)
				if (dp[i][j][k] != -1)
				{
					int nxti = i, nxtj = j;
					if (i + 1 <= len1 && s1[i + 1] == '(') nxti++;
					if (j + 1 <= len2 && s2[j + 1] == '(') nxtj++;

					if (k + 1 < 2 * N && checkmin(dp[nxti][nxtj][k + 1], dp[i][j][k] + 1))
						pre[nxti][nxtj][k + 1] = Node(i, j, k, '(');

					nxti = i, nxtj = j;
					if (i + 1 <= len1 && s1[i + 1] == ')') nxti++;
					if (j + 1 <= len2 && s2[j + 1] == ')') nxtj++;
					
					if (k + 1 < 2 * N && checkmin(dp[nxti][nxtj][k - 1], dp[i][j][k] + 1))
						pre[nxti][nxtj][k - 1] = Node(i, j, k, ')');
				}
	
	int len = 2 * N;
	int up = len1, down = len2, last = 0;
	for (int i = 0; i < 2 * N; i++)
		if (dp[len1][len2][i] != -1 && dp[len1][len2][i] + i < len)
		{
			len = dp[len1][len2][i] + i;
			last = i;
			break;
		}

	string ans = "";
	for (int i = 0; i < last; i++) ans += ')';

	int ans_len = dp[len1][len2][last];
	for (int i = 0; i < ans_len; i++)
	{
		Node t = pre[up][down][last];
		ans += t.ch;
		up = t.up, down = t.down, last = t.k;
	}

	reverse(ans.begin(), ans.end());
	cout << ans << endl;

	return 0;
}
```

