### A. Yet Another Tetris Problem

*Description*

俄罗斯方块，落下来的方块，只有2 * 1规格的，高2 * 宽1。可以落下来一块，可以消掉一行，问给出的图形，能不能消成平地

*Solution*

问题转化为，序列当中，每一个数都可以 -1，其中的一个数可以 +2，能不能把所有的数都减成0。序列中，都是奇数，可以消掉；或者都是偶数。这样才能够通过+2的操作让他们平齐

*Code*

```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 110;

int a[N];
int n;
int T;

int main()
{
	scanf("%d", &T);
	
	while (T--)
	{
		scanf("%d", &n);

		for (int i = 1; i <= n; i++) scanf("%d", &a[i]);

		if (n == 1)
		{
			printf("YES\n");
			continue;
		}

		bool success = true;
		for (int i = 2; i <= n; i++)
			if ((a[i] & 1) != (a[i - 1] & 1))
			{
				success = false;
				break;
			}

		if (success) printf("YES\n");
		else printf("NO\n");
	}

	return 0;
}
```



### B. Yet Another Palindrome Problem

*Description*

判断序列当中，只有存在长度大于等于3的回文子序列。注意是子序列，不是子串

*Solution*

问题转化为，判断是否一个数，隔一位是否重复出现。

*Code*

```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 5010;

int a[N];
int T;
int n;

int main()
{
	scanf("%d", &T);

	while (T--)
	{
		scanf("%d", &n);

		for (int i = 1; i <= n; i++) scanf("%d", &a[i]);
		
		bool have_ans = false;
		for (int i = 1; i <= n; i++)
		{
			for (int j = i + 2; j <= n; j++)
				if (a[i] == a[j])
					have_ans = true;
			
			if (have_ans) break;
		}			


		if (have_ans) printf("YES\n");
		else printf("NO\n");
	}

	return 0;
}
```



### C. Frog Jumps

*Description*

一个字符串，包含L和R，青蛙遇到L只能往左跳，遇到R只能往右跳，青蛙需要从0跳到n+1，青蛙的步长最大为d，求最小的d是多少

*Solution*

问题转化为，字符串中，相邻的两个R之间距离，最大是多少。样例1中的举例是一个迷魂阵。分析出问题本质后，我们在头加装一个R，尾部加装一个R，然后双指针for一遍，看两个相邻的R，最大间距是多少。对于原字符串中没有R的，是成立的

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
		s = 'R' + s;
		s = s + 'R';

		int ans = 0;
		for (int i = 0, j, len = s.size(); i < len; i++)
		{
			if (s[i] == 'R')
			{
				for (j = i + 1; j < len; j++)
				{
					if (s[j] == 'R')
					{
						ans = max(ans, j - i);
						break;
					}
				}
			}

			i = j - 1;
		}
		
		cout << ans << endl;
	}

	return 0;
}
```



### D. Pair of Topics

*Description*

两个序列，求有多少对 ai + aj > bi + bj，n的取值范围是2*10^5

*Solution*

根据取值范围，判断是nlogn的算法，可能是二分

然后开始分析这个不等式，特别像初中的代数问题，进行一下不等式变化，学名叫“参数分离”，(ai - bi) + (aj - bj) > 0，令ci = ai - bi，c[i] + c[j] > 0，问题转化成，求新的序列中，对于c[i]，有多少个数和他相加大于0（例如，c[i] > 0的时候，序列中，大于他的相反数的位置和ci之间有多少个数），问题变成有单调性的了，就可以sort一遍，就二分。（考试的时候，想到要用二分，但是用了一个小时也没搓出来这个单调性，赛后反思，这些数学不等式，应该像做数学题目一样，在草稿纸上一步一步推一推，就能推出来，千万不要空想）

*Code*

```cpp
#include <bits/stdc++.h>

using namespace std;

typedef long long LL;

const int N = 2e5 + 10;

int a[N], b[N];
int c[N];
int n;

int main()
{
	scanf("%d", &n);

	for (int i = 0; i < n; i++) scanf("%d", &a[i]);
	for (int i = 0; i < n; i++) scanf("%d", &b[i]);

	//ai - bi - (aj - bj) > 0, i < j
	for (int i = 0; i < n; i++) 
		c[i] = a[i] - b[i];

	sort(c, c + n);

	LL ans = 0;  //signed integer overflow
	for (int i = 0; i < n; i++)
	{
		if (c[i] <= 0) continue; 
		//我们从c[i]大于0的开始，往前找对应比自己相反数大的位置
		//pos...i这个区间，就是满足-c[i] + c[i] > 0的
		int pos = upper_bound(c, c + n, -c[i]) - c;
		ans += i - pos;
	}

	printf("%lld\n", ans);

	return 0;
}
```



### E. Sleeping Schedule

*Description*

一个小伙子，有一个睡觉计划，要么正常睡，要么提前1小时睡，如果入睡时间在[l, r]之间，就是一次good睡眠，问最多能有几次good睡眠

*Solution*

This is a very standard dynamic programming problem. *1700

anyway, it is not standard for me now

dp的分析方法真的有很多种思考，并且调试也非常难，如果你转移方程想的不对，是很难通过调试代码调出来的。我们先这样思考一下：

状态表示dp[ij]，前i次入睡，j次提前1小时入睡的所有选法，属性Max。初始状态dp[0,0] = 0

状态计算，dp[ij]分为两个子集，提前睡 + 不提前睡

提前睡：dp[i,j] = max(dp[i,j], dp[i-1,j-1] + check(s[i] - j));

不提前睡：dp[i, j-1] = max(dp[i, j-1], dp[i-1, j-1] + check(s[i] - (j-1)));

其中，int check(int x)判断x是不是好的入睡时间

*Code*

```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 2010;

int dp[N][N];
int a[N], s[N];
int n, h, l, r;

int check(int x)
{
	int t = x % h;

	if (t >= l && t <= r) return 1;
	else return 0;
}

int main()
{
	scanf("%d%d%d%d", &n, &h, &l, &r);

	for (int i = 1; i <= n; i++) scanf("%d", &a[i]);
	for (int i = 1; i <= n; i++) s[i] = s[i - 1] + a[i];

	memset(dp, -0x3f, sizeof dp);
	dp[0][0] = 0;
	for (int i = 1; i <= n; i ++)
		for (int j = 1; j <= i; j++)
		{
			dp[i][j - 1] = max(dp[i][j - 1], dp[i - 1][j - 1] + check(s[i] - (j - 1)));
			dp[i][j] = max(dp[i][j], dp[i - 1][j - 1] + check(s[i] - j));
		}

	int ans = 0;
	for (int j = 0; j <= n; j++)
		ans = max(ans, dp[n][j]);
	
	printf("%d\n", ans);

	return 0;
}
```



上面这个是这样转移的，从阶段i-1 转移到 阶段i，这里面我们遇到了dp[i,j-1]的式子，所以要注意边界不能是负数

如果我们从阶段i转移到阶段i+1，那么我们在处理边界的时候，就简单一些，结果是一样的，给出示例代码

```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 2020;

int dp[N][N];
int a[N], s[N];
int n, h, l, r;

int check(int x)
{
	int t = x % h;
	if (t >= l && t <= r) return 1;
	else return 0;
}

int main()
{
	scanf("%d%d%d%d", &n, &h, &l, &r);

	for (int i = 1; i <= n; i++) scanf("%d", &a[i]);

	for (int i = 1; i <= n; i++) s[i] = s[i - 1] + a[i];

	memset(dp, -1, sizeof dp);
	
  	dp[0][0] = 0;
	for (int i = 0; i < n; i++)
		for (int j = 0; j <= i; j++)  //这里要注意只能枚举到i，否则就把后面的阶段影响了
		{
			dp[i + 1][j] = max(dp[i + 1][j], dp[i][j] + check(s[i + 1] - j));
			dp[i + 1][j + 1] = max(dp[i + 1][j + 1], dp[i][j] + check(s[i + 1] - (j + 1))); //早睡1小时
		}
		
	int ans = 0;
	for (int j = 0; j <= n; j++)
		ans = max(ans, dp[n][j]);

	printf("%d\n", ans);

	return 0;
}
```

看了别人的代码，也有一些启发，还可以简单的写成

```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 2020;

int dp[N][N];
int a[N], s[N];
int n, h, l, r;

int check(int x)
{
	int t = x % h;
	if (t >= l && t <= r) return 1;
	else return 0;
}

int main()
{
	scanf("%d%d%d%d", &n, &h, &l, &r);

	for (int i = 1; i <= n; i++) scanf("%d", &a[i]);

	memset(dp, -1, sizeof dp);
	
  dp[0][0] = 0;
	int sum = 0;
	for (int i = 1; i <= n; i++)
	{
		sum += a[i];
		for (int j = 0; j <= i; j++)
		{
			dp[i][j] = max(dp[i - 1][j], dp[i - 1][j - 1]);
			if (check(sum - j)) dp[i][j] += 1;
		}
	}

	int ans = 0;
	for (int j = 0; j <= n; j++)
		ans = max(ans, dp[n][j]);

	printf("%d\n", ans);

	return 0;
}
```

首先，处理前缀和的时候，随用随处理，这个倒无所谓

dp[i,j] = max(dp[i-1,j], dp[i-1,j-1])，这个也比较好理解

if (check(sum-j)) dp[i,j]+=1  如果第i次入睡，提前1小时满足好习惯的条件，就+=1，这样的状态转移方程更加的直观



### F. Maximum White Subtree

*Description*



*Solution*



*Code*

