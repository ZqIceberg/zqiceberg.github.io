#### A. Candies and Two Sisters

*Description*

A、B两人分n块糖，a + b = n, a > b，求满足条件的a和b，一共有多少种分法

*Solution*

分情况讨论，n <=2的时候，无解

再讨论n是偶数的时候，n是奇数的时候

*Code*

```cpp
#include <bits/stdc++.h>

using namespace std;

int T;
int n, a, b;

int main()
{
	scanf("%d", &T);

	while (T--)
	{
		scanf("%d", &n);

		if (n <= 2)
		{
			printf("0\n");
			continue;
		}

		if (n & 1)
		{
			printf("%d\n", n / 2);
		}
		else
		{
			printf("%d\n", n / 2 - 1);
		}
	}

	return 0;
}
```



#### B. Construct the String

*Description*

构造长度为n的字符串，长度为a的子串中，有b个不同的字符

*Solution*

我的思路是aaaabcdaaaabcd，这样循环，然后去前n个字符

Mike的题解是，如果b个不同，123b123b123b123b这样循环n的字符

*Code*

```cpp
#include <bits/stdc++.h>

using namespace std;

int T;
int n, a, b;

int main()
{
	cin >> T;
	while (T--)
	{
		cin >> n >> a >> b;
		
		string s = "";
		if (a != b)
			s = string(a - b - 1, 'a');
		
		for (int i = 0; i < b; i++)
			s += 'a' + i;

		string ans = "";
		for (int i = 1; i <= n / b + 1; i++) 
			ans += s;

		for (int i = 0; i < n; i++) cout << ans[i];
		cout << endl;
	}

	return 0;
}
```



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
		int n, a, b;
		cin >> n >> a >> b;
		for (int i = 0; i < n; ++i) {
			cout << char('a' + i % b);
		}
		cout << endl;
	}
	
	return 0;
}
```



#### C. Two Teams Composing

*Description*

n个学生，分成a、b两个队，a队里技能不能重复，b队里只有一种重复的技能，a、b的长度相同。求满足条件下，队伍的长度x

*Solution*

一共有diff个不同的技能，出现次数最多的技能max，出现次数是maxcnt

两个情况：

把max技能全都分配给b队，a队剩下不同的技能个数是diff-1

把max技能，留一个，剩下的分配给b队，a队剩下不同的技能个数是diff

求两种情况的max

*Code*

```cpp
#include <bits/stdc++.h>

using namespace std;

int T, n;

int main()
{
	scanf("%d", &T);

	while (T--)
	{
		scanf("%d", &n);
		
		map<int, int> mp;
		for (int i = 0; i < n; i++)
		{
			int a;
			scanf("%d", &a);
			mp[a]++;
		}

		int diff = mp.size();
		
		int maxcnt = 0;
		map<int, int>::iterator it;
		for (it = mp.begin(); it != mp.end(); it++)
			if ((*it).second > maxcnt)
				maxcnt = (*it).second;

		int ans = max(min(diff - 1, maxcnt), min(diff, maxcnt - 1));

		printf("%d\n", ans);
	}

	return 0;
}
```



#### D. Anti-Sudoku

*Description*

数独，最多修改9个字符，让每行，每列，每一个小9宫格，都至少有两个重复的数字

*Solution*

Mike的题解是，把全局的2，填成1，就达到目的。

我的思路是，挑9个位置的数字，进行调整。下面给出代码示例

*Code*

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
		for (int i = 0; i < 9; ++i) {
			string s;
			cin >> s;
			for (auto &c : s) if (c == '2') c = '1';
			cout << s << endl;
		}
	}
	
	return 0;
}
```



```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 10;

char g[N][N];
int T;

int main()
{
	scanf("%d", &T);

	while (T--)
	{
		for (int i = 0; i < 9; i++) scanf("%s", g[i]);
		
		g[0][0] = g[0][1], g[1][3] = g[1][4], g[2][6] = g[2][7];
		g[3][1] = g[3][2], g[4][4] = g[4][5], g[5][7] = g[5][8];
		g[6][2] = g[6][1], g[7][5] = g[7][4], g[8][8] = g[8][7];

		for (int i = 0; i < 9; i++) printf("%s\n", g[i]);
	}

	return 0;
}
//Mike`s solution is excellent!
```



#### E1. Three Blocks Palindrome (easy version)

*Description*

*1700, ai <= 26

序列中，选出3个子序列，构造出aaaabbbaaaa的回文模型，求满足条件下，回文子序列的最长长度

easy版本，

*Solution*

预处理前i个字符中，1~26每个数字出现的次数，回文分为3个block，xxxxxyyyxxxx，第一个block和第三个block是相同的关系，第二个block是一个关系。

回文子序列，有两个情况，只有一个数字组成，由一个数字组成

只有一种数字组成的，就枚举一遍1~26，谁出现的次数最多

两个数字组成的情况，枚举第二个block的l，r两个端点。

[l, r]出现的同一个数字最多的情况，[0,l-1] [r+1, n]出现相同的数字的情况，三个block拼接一起。枚举的时候直接1~26枚举。

预处理，Mo`s algorithm（莫队NB），做前缀和的时候，注意0-index还是1-index

*Code*

```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 2010;

int a[N];
int cnt[30][N];
int T, n;

int main()
{
	scanf("%d", &T);

	while (T--)
	{
		scanf("%d", &n);
		for (int i = 1; i <= n; i++) scanf("%d", &a[i]);

		memset(cnt, 0, sizeof cnt);
		for (int i = 1; i <= n; i++)
		{
			for (int j = 1; j <= 26; j++) cnt[j][i] = cnt[j][i - 1];
			cnt[a[i]][i]++;
		}

		int ans = 0;
		for (int i = 1; i <= 26; i++) ans = max(ans, cnt[i][n]);

		for (int l = 2; l <= n; l++)
		{
			for (int r = l; r < n; r++)
			{
				int cntin = 0, cntout = 0;
				for (int k = 1; k <= 26; k++)
				{
					cntin = max(cntin, cnt[k][r] - cnt[k][l - 1]);
					cntout = max(cntout, min(cnt[k][l - 1], cnt[k][n] - cnt[k][r]));
				}
				
				ans = max(ans, cntin + cntout * 2);
			}
		}

		printf("%d\n", ans);
	}

	return 0;
}
```



#### E2. Three Blocks Palindrome (hard version)

*Description*

*1800, ai <= 200

*Solution*

在第一个block中取k个字符，那么就要在第三个block中取k个字符，我们可以贪心的取，取最左边的k个，取最右边的k个

用vector<int> pos[220]维护每个数字出现的位置

做枚举l，r边界的时候，就可以改变成，第一个第三个block取i，那么左右两边最多各取 pos[i].size / 2，就可以枚举1~200，左右两边取i的情况

中间部分，枚举1~200，看这个区间内哪个字符出现的次数最多

调试的过程中，下标问题特别多

*Code*

```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 2e5 + 10;

int cnt[220][N];
int T, n;
int a[N];

int main()
{
	scanf("%d", &T);

	while (T--)
	{
		vector<int> pos[220];
		
		scanf("%d", &n);
		for (int i = 1; i <= n; i++) scanf("%d", &a[i]);

		for (int i = 1; i <= n; i++)
		{
			for (int j = 1; j <= 200; j++) cnt[j][i] = cnt[j][i - 1];
			cnt[a[i]][i]++;
			pos[a[i]].push_back(i);
		}

		int ans = 0;
		for (int i = 1; i <= 200; i++)
		{
			int sz = pos[i].size();
			ans = max(ans, sz);

			for (int p = 0; p < sz / 2; p++)
			{
				int l = pos[i][p] + 1, r = pos[i][cnt[i][n] - p - 1] - 1; //0-index
			
				for (int k = 1; k <= 200; k++)
				{
					int sum = cnt[k][r] - cnt[k][l - 1];
					ans = max(ans, sum + (p+1)*2);
				}
			}
		}

		printf("%d\n", ans);
	}

	return 0;
}
```

