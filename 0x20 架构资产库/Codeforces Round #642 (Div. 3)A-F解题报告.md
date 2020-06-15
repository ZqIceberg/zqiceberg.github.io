### Codeforces Round #642 (Div. 3)
做出了ABC，是构造题和数学，D是数据结构，set不会用，E和F是dp


#### A.MostUnstableArray
*Description:*

n个数，和为m，构造一个序列，相邻两个数的差的绝对值最大

*Solution:*

n = 1, or n =2 需要特判， 当n >= 3时, 0, m, 0, 0 , ... 这样构造

*Code:*

```cpp
#include <bits/stdc++.h>

using namespace std;

int main() {
#ifdef _DEBUG
	freopen("input.txt", "r", stdin);
//	freopen("output.txt", "w", stdout);
#endif
	
	int t;
	cin >> t;
	while (t--) {
		int n, m;
		cin >> n >> m;
		cout << min(2, n - 1) * m << endl;
	}
	
	return 0;
}
```

#### B.TwoArraysAndSwaps
*Description:*

两个数组a, b，在不超过k次交换的情况下，让a中所有元素之和最大，输出最大的和

*Solution:*

greedy，a从小到大排序，b从大到小排序，for一遍，ai大取ai，bi大取bi

*Code:*
```cpp
#include <bits/stdc++.h>

using namespace std;

int main() {
#ifdef _DEBUG
	freopen("input.txt", "r", stdin);
//	freopen("output.txt", "w", stdout);
#endif
	
	int t;
	cin >> t;
	while (t--) {
		int n, k;
		cin >> n >> k;
		vector<int> a(n);
		for (auto &it : a) cin >> it;
		vector<int> b(n);
		for (auto &it : b) cin >> it;
		sort(a.begin(), a.end());
		sort(b.rbegin(), b.rend());
		int ans = 0;
		for (int i = 0; i < n; ++i) {
			if (i < k) ans += max(a[i], b[i]);
			else ans += a[i];
		}
		cout << ans << endl;
	}
	
	return 0;
}
```

#### C.BoardMoves
*Description:*

给你一个n * n的矩阵，n为odd，把所有单元上的物品移动到一个单元上，问你最少的操作次数

*Solution:*

移动到矩阵的中心位置是合理的，转化为所有点到中心位置的距离的和

从里往外，一圈一圈的看，第1圈8个点，距离是1，第2圈16个点，距离是2，以此类推，最多 n / 2圈

*Code:*
```cpp
#include <bits/stdc++.h>

using namespace std;

int main() {
#ifdef _DEBUG
	freopen("input.txt", "r", stdin);
//	freopen("output.txt", "w", stdout);
#endif
	
	int t;
	cin >> t;
	while (t--) {
		int n;
		cin >> n;
		long long ans = 0;
		for (int i = 1; i <= n / 2; ++i) {
			ans += i * 1ll * i;
		}
		cout << ans * 8 << endl;
	}
	
	return 0;
}
```

#### D.ConstructingtheArray
*Description:*

长度为n的数组a，都是0，每次选取一段最长的连续区间[l, r]， 区间内都是0， 如果有多个区间满足条件，选取最左边的区间。

如果r - l + 1是奇数，a[(l+r)/2] = i；否则，a[(l + r - 1) / 2] = i

最终输出数组a

*Solution:*

每次选取符合条件的[l, r]，给对应的a[i]赋值，每一次赋值都会得到两个更小的区间。每一次操作，都需要对所有区间进行一次排序

*Code:*

贴一份set<pair<int, int> >实现的代码
```cpp
#include <bits/stdc++.h>

using namespace std;

struct cmp {
	bool operator() (const pair<int, int> &a, const pair<int, int> &b) const {
		int lena = a.second - a.first + 1;
		int lenb = b.second - b.first + 1;
		if (lena == lenb) return a.first < b.first;
		return lena > lenb;
	}
};

int main() {
#ifdef _DEBUG
	freopen("input.txt", "r", stdin);
//	freopen("output.txt", "w", stdout);
#endif
	
	int t;
	cin >> t;
	while (t--) {
		int n;
		cin >> n;
		set<pair<int, int>, cmp> segs;
		segs.insert({0, n - 1});
		vector<int> a(n);
		for (int i = 1; i <= n; ++i) {
			pair<int, int> cur = *segs.begin();
			segs.erase(segs.begin());
			int id = (cur.first + cur.second) / 2;
			a[id] = i;
			if (cur.first < id) segs.insert({cur.first, id - 1});
			if (id < cur.second) segs.insert({id + 1, cur.second});
		}
		for (auto it : a) cout << it << " ";
		cout << endl;
	}
	
	return 0;
}
```

贴一份priority_queue<Node>，自定义数据结构，重载小于符号的代码。[参考链接](https://www.cnblogs.com/nonameless/p/12894291.html#4275196340)
```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 2e5 + 10;

struct Seg
{
	int l, r;

	bool operator< (const Seg& W)const
	{
		if ((r - l + 1) != (W.r - W.l + 1)) return (r - l + 1) < (W.r - W.l + 1);
		return l > W.l;
	}
};

int a[N];
int T;
int n;

int main()
{
	scanf("%d", &T);

	while (T--)
	{
		priority_queue<Seg> q;

		scanf("%d", &n);
		Seg seg;
		seg.l = 1, seg.r = n;
		q.push(seg);
		
		memset(a, 0, sizeof a);
		for (int i = 1; i <= n; i++)
		{
			Seg t = q.top();q.pop();

			int l = t.l, r = t.r;
			//printf("---%d %d\n", l , r);

			if ((r - l + 1) % 2)  //这个地方写的啰嗦
			{
				int p = (l + r) / 2;
				a[p] = i;
				if (l <= p - 1)
				{
					Seg t1;
					t1.l = l, t1.r = p - 1;
					q.push(t1);
				}
				if (p + 1 <= r)
				{
					Seg t2;
					t2.l = p + 1, t2.r = r;
					q.push(t2);
				}
			}
			else
			{
				int p = (l + r - 1) / 2;
				a[p] = i;
				if (l <= p - 1)
				{
					Seg t1;
					t1.l = l, t1.r = p - 1;
					q.push(t1);
				}
				if (p + 1 <= r)
				{
					Seg t2;
					t2.l = p + 1, t2.r = r;
					q.push(t2);
				}
			}
		}

		for (int i = 1; i <= n; i++) printf("%d ", a[i]);
		puts("");
	}
}
```


#### E.K-periodicGarland
*Description:*

长度为n的01字符串，每一次操作改变一个字符，0变1，或者1变0，问最少几次操作可以使任意相邻的两个1的距离为k

*Solution:*

线性dp，讨论第i个位置，这个位置我们可以让他是0，也可以让他是1，先不考虑这个位置之前是0还是1，那么

让这个位置是0, f[i][0] = min(f[i-1][0], f[i-1][1]) + a[i] == 1，前一个点是0或者1都可以，当前点如果是1的话，还要操作一次

让这个位置是1，f[i][1] = min(f[p][1] + s[i - 1] - s[p], s[i - 1]) + (a[i] == '0'); 当前点是1，要么，i-k的那个位置是1，中间距离都是0；要么，当前点是第一个1。我们用前缀和维护1的个数，这里判断i-k的位置时，要判断一下不能小于0。最后当前位置是0的话，还要再操作一次，变成1

*Code:*
```cpp
//所以对于测试数据很多的情况还是慎用memset

#include <bits/stdc++.h>

using namespace std;

const int N = 1e6 + 10;

int f[N][2];  //要第i位是0，要第i位是1
char a[N];
int s[N];
int T;
int n, k;

int main()
{
	scanf("%d", &T);

	while (T--)
	{
		scanf("%d%d", &n, &k);

		scanf("%s", a + 1);

		//memset(f, 0, sizeof f);  //使用memset会TLE，25000组数据
		//memset(s, 0, sizeof s);

		for (int i = 0 ; i <= n; i++)
		{
			f[i][0] = 0, f[i][1] = 0, s[i] = 0;
		}

		for (int i = 1; i <= n; i++) s[i] = s[i - 1] + (a[i] - '0');

		//处理完成前缀和，下面开始分情况，要第i位是0，要第i为是1的情况

		f[1][0] = f[1][1] = 0;
		for (int i = 1; i <= n; i++)
		{
			//如果a[i]是1还要操作一下
			f[i][0] = min(f[i - 1][0], f[i - 1][1]) + (a[i] == '1');  

			int p = max(0, i - k);
			f[i][1] = min(f[p][1] + s[i - 1] - s[p], s[i - 1]) + (a[i] == '0');
		}

		int ans = min(f[n][0], f[n][1]);
		printf("%d\n", ans);
	}

	return 0;
}

```

#### F.DecreasingHeights
*Description:*

给定一个矩阵，可以向右走，或者向下走，每一个位置都给定了一个高度，例如当前位置高度是x，那么下一步的高度，必须是x+1。你可以无限次降低任意一个位置的高度，让你能够从(1,1)走到(n,m)。请输出最少要修改几次高度。注意，起始位置(1,1)的高度，你也可以修改

*Solution:*

数字三角形模型

因为起始位置高度可以修改，这就导致了问题的复杂，如果起始位置高度固定，那么就可以用数字三角形模型，一路走到(n, m)。如果我们枚举起始高度，那么肯定TLE

分析，我们走的路线是a1, a2, a3, ...，对应的每一个位置的操作次数的t1, t2, t3, ... ，（a1位置操作高度t1次），ai - ti + 1 = ai+1 - ti+1，这是题目中的限定条件。等式两边同时加上min(ti, t i+1)，等式依然成立。扩展到k个点，等式依然成立。那么 ai - ti -min(t1,t2,...,tk) + 1 = ai+1 - ti+1 - min(t1,t2,...,tk)成立的话，ti的最小值，就不能是负数，最小顶多是0，那么ti存在0的话，就说明存在一个位置没有对高度进行操作。

利用上面的分析，我们枚举n^2个点，假定这个点是高度没有操作的位置，然后用数字三角形模型做一遍DP，时间复杂度O(n^ 2 * n^2)

*Code:*
```cpp
#include <bits/stdc++.h>

using namespace std;

typedef long long LL;

const int N = 110;
const LL INF = 1e18;  //不能用0x3f太小了

LL a[N][N], f[N][N];
int T;
int n, m;

int main()
{
	scanf("%d", &T);

	while (T--)
	{
		scanf("%d%d", &n, &m);

		for (int i = 1; i <= n; i++)
			for (int j = 1; j <= m; j++)
				scanf("%lld", &a[i][j]);

		LL ans = INF;
		for (int i = 1; i <= n; i++)
			for (int j = 1; j <= m; j++)
			{
				//枚举不用操作的点
				
				//memset(f, 0x3f, sizeof f);
				for (int i = 0; i <= n; i++)
					for (int j = 0; j <= m; j++)
						f[i][j] = INF;

				f[1][0] = 0;  //[1,1]只能从[1,0]过来
				
				for (int x = 1; x <= n; x++)
					for (int y = 1; y <= m; y++)
					{
						LL d = abs(x - i) + abs(y - j);
						
						if (x <= i && y <= j && a[x][y] >= a[i][j] - d) //[x,y]在定点的左上方
							f[x][y] = min(f[x-1][y], f[x][y-1]) + a[x][y] - (a[i][j] - d);

						if (x >= i && y >= j && a[x][y] >= a[i][j] + d) //[x,y]在定点的右下方
							f[x][y] = min(f[x-1][y], f[x][y-1]) + a[x][y] - (a[i][j] + d);
					}

				ans = min(ans, f[n][m]);		
			}

		printf("%lld\n", ans);
	}

	return 0;
}
```