#### A. Candies

*Description*

一个等比数列，x + 2x + 4x + ... + 2^k-1 x = n，x是正整数，k大于1。给出n和k，输出满足条件的x

*Solution*

等式左边提取因子，变成等比数列求和 (2^k1 - 1) * x = n，左边是指数枚举，上升很快可以直接枚举，条件是能整除n的时候停下来。2^k-1的计算，用位运算不错。

*Code*

```cpp
#include <bits/stdc++.h>

using namespace std;

typedef long long LL;

int T;
int n;

int main()
{
	scanf("%d", &T);

	while (T--)
	{
		scanf("%d", &n);

		/*
		LL t = 2;
		for (int k = 2; ;k++)
		{
			t *= 2;
			if (n % (t - 1) == 0)
			{
				printf("%lld\n", n / (t - 1));
				break;
			}
		}
		*/

		for (int pw = 2; pw < 30; pw++)
		{
			LL val = (1 << pw) - 1;
			if (n % val == 0)
			{
				printf("%lld\n", n / val);
				break;
			}
		}
	}

	return 0;
}
```



#### B. Balanced Array

*Description*

n个正整数，前半段都是偶数，后半段都是奇数，前半段的和等于后半段的和，给出n请你构造出满足条件的序列

*Solution*

n / 2 如果是奇数，就无法构造，因为奇数个奇数的和还是奇数，无法和偶数那半段的和相等。

摆放的时候，先放偶数2 4 6 8 10 12，再放奇数1 3 5 7 9 x，最后一个奇数按这个规律是放11，但是有半段每一个数都比前半段对应的少1，为了让和相等，把差额不上，11 + 6(长度)

*Code*

```cpp
#include <bits/stdc++.h>

using namespace std;

int T;
int n;

int main()
{
	scanf("%d", &T);

	while (T--)
	{
		scanf("%d", &n);
		
		int len = n / 2;
		if (len & 1)   //奇数个奇数凑不出偶数
		{
			printf("NO\n");
			continue;
		}
		else
		{
			printf("YES\n");
			int i, a;
			for (i = 1, a = 2; i <= len; i++, a += 2)
				printf("%d ", a);

			for (i = 1, a = 1; i <= len - 1; i++, a += 2)
				printf("%d ", a);
			a += len;
			printf("%d\n", a);
		}
	}

	return 0;	
}
```



#### C. Alternating Subsequence

*Description*

交替的子序列，这个子序列是一个正数，一个负数，交替组成，题目求满足条件的子序列，要首先保证长度最长，再保证元素之和最大，请你输出元素之和

*Solution*

在原序列中去子序列出来，一个正，一个负的取，并且要长度最长，和最大。换句话，尽可能的取，取的越远越好。一段正数中，取一个数，一段负数中，取一个数。为了让和最大，我们就取每个区间中数值最大的即可

*Code*

```cpp
#include <bits/stdc++.h>

using namespace std;

typedef long long LL;

const int N = 2e5 + 10;

int T;
int n;
LL a[N];

int main()
{
	scanf("%d", &T);

	while (T--)
	{
		scanf("%d", &n);

		for (int i = 1; i <= n; i++) scanf("%lld", &a[i]);
		
		LL ans = 0;
		for (int i = 1, j; i <= n; i++)
		{
			LL x = a[i];
			for (j = i + 1; j <= n; j++)
			{
				if (a[j] * a[i] < 0) break;
				x = max(x, a[j]);
			}
			ans += x;
			i = j - 1;

			//printf("++ %lld\n", x);
		}

		printf("%lld\n", ans);
	}

	return 0;
}
```



#### D. Constant Palindrome Sum

*Description*

数组a，长度是偶数，希望两两之和相等（a[i] + a[n - i + 1]这个和相等），问最少操作几次可以达到这种状态。每一个数，给出的最大上限k，这位数，最小可以变成1，最大可以变成k

*Solution*

[https://www.cnblogs.com/stelayuri/p/12749332.html](https://www.cnblogs.com/stelayuri/p/12749332.html)这篇题解，写的很清晰，备份一下

[https://www.cnblogs.com/axiomofchoice/p/12749281.html](https://www.cnblogs.com/axiomofchoice/p/12749281.html)，这篇也很清晰

使用[差分]对定值x进行标记，x是a[i] + a[n - i + 1]，x是不知道的，是可以不断变化的，但是取值范围是确定的，[2, k * 2]。这样就可以针对每一个定值进行枚举分析（显然这样TLE）

“由于我们每次修改都是区间加，并且离线操作，可以考虑用差分数组”，对区间进行操作，用到差分

*Code*

 ```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 2e5 + 10;

int a[N * 2], b[N * 2], p[N];
int n, k;
int T;

void insert(int l, int r, int c)
{
	b[l] += c;
	b[r + 1] -= c;
}

void print()
{
	for (int i = 2; i <= k * 2; i++)
			printf("%d ", b[i]);
		puts("");
}

int main()
{
	scanf("%d", &T);

	while (T--)
	{
		memset(b, 0, sizeof b);  //每一次要clear一下b[]

		scanf("%d%d", &n, &k);

		for (int i = 1; i <= n; i++) scanf("%d", &p[i]);

		for (int i = 1; i <= n / 2; i++)
		{
			int x = p[i], y = p[n - i + 1];
			int minn = min(x, y), maxn = max(x, y);
			
			//差分数组标记原理
			//定值在[2, minn]之间
			insert(2, minn, 2);

			//定值在[maxn + k + 1, 2k]之间
			insert(maxn + k + 1, k * 2, 2);

			//定值在[minn+1, maxn+k]之间
			insert(minn + 1, maxn + k, 1);
		
			//sum = x + y的时候，不要操作，把前面的1补回来
			b[x + y] -= 1;
			b[x + y + 1] += 1;
		}
		
		for (int i = 2; i <= k * 2; i++)
			b[i] += b[i - 1];
		
		int ans = 0x3f3f3f3f;
		for (int i = 2; i <= k * 2; i++)
			ans = min(ans, b[i]);

		printf("%d\n", ans);
	}

	return 0;
}
 ```



#### E. Weights Distributing

*Description*

n个点，m条边，从a走到b，再从b走到c，给出m条边的权值数组，但是并没有限制哪条边的权值是哪个。题目求，如何分配这些权值，让a->b->c的代价最小

*Solution*

[bfs]

图论里，从一个点走到另一个点的最短路径，是Floyed。这道题也是这个思路（我是看Mike题解长大的）

两条路径，a->b, b->c，假设两条路径有一个交叉点x，a->x->b, b->x->c，这条路线分为a-x b-x b-x c-x这四段，其中两段是重复的。我们需要计算出，这四段最小的步数分别是什么，然后每走一步就相当于消耗掉一个权值（就是Distrbuting的过程）比如，b-x 走了2步，那就分配两个最小权值给这条路径。a-x b-x b-x c-x这四段，进一步的，分为b-x；a-x, b-x, c-x，这样的两部分。如何求a-x的最小值呢，从a出发做一遍bfs，用ds[]维护为个点到a的最小值。for一遍x的位置，找到a-x, b-x,c-x的最小值。比如最小值是5步，那么我们给出权值序列前5个权值的前缀和。

这道题，需要对开long long，最后枚举最小值的时候也要给ans初始一个很大的数，0x3f3f3f3f是不行的，需要1e18

*Code*

```cpp
#include <bits/stdc++.h>

using namespace std;

typedef long long LL;

const int N = 2e5 + 10, INF = 1e9;

int h[N], e[N * 2], ne[N * 2], idx;
LL w[N], s[N];
int n, m, a, b, c;
int da[N], db[N], dc[N]; //a, b, c三点到其他每个点的最短距离

void add(int a, int b)
{
	e[idx] = b, ne[idx] = h[a], h[a] = idx++;
}

void bfs(int u, int du[])
{
	queue<int> q;
	q.push(u);
	du[u] = 0;

	while (!q.empty())
	{
		int t = q.front();
		q.pop();

		for (int i = h[t]; i != -1; i = ne[i])
		{
			int j = e[i];
			if (du[j] == 0x3f3f3f3f) 
			{	
				du[j] = du[t] + 1;
				q.push(j);
			}
		}
	}
}

int main()
{
	int T;
	scanf("%d", &T);

	while (T--)
	{
		scanf("%d%d%d%d%d", &n, &m, &a, &b, &c);

		for (int i = 1; i <= m; i++) scanf("%lld", &w[i]);

		sort(w + 1, w + 1 + m);
		for (int i = 1; i <= m; i++) s[i] = s[i - 1] + w[i]; 
		
		memset(h, -1, sizeof h);
		for (int i = 1; i <= m; i++)
		{
			int a, b;
			scanf("%d%d", &a, &b);

			add(a, b); 
			add(b, a);
		}

		memset(da, 0x3f, sizeof da);
		memset(db, 0x3f, sizeof db);
		memset(dc, 0x3f, sizeof dc);
		bfs(a, da);
		bfs(b, db);
		bfs(c, dc);

		//for (int i = 1; i <= n; i++) printf("%d ", dc[i]);
		//puts("");

		LL ans = 1e18;
		for (int i = 1; i <= n; i++)  //枚举x这个固定点
		{
			int t = da[i] + db[i] +dc[i];
			if (t <= m) //走了几步
			{
				//printf("---%d %d\n", i, t);
				ans = min(ans, s[db[i]] + s[t]);  //走了几步，可靠前面小的边用
			}
		}
		printf("%lld\n", ans);
	}

	return 0;
}
```



#### F. Restore the Permutation by Sorted Segments

*Description*



*Solution*

[https://www.cnblogs.com/axiomofchoice/p/12749281.html](https://www.cnblogs.com/axiomofchoice/p/12749281.html)

*Code*