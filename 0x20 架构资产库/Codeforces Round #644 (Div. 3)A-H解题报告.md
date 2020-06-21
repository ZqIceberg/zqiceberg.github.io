#### A. Minimal Square

*Description*

给定两个长方形，长宽a，b，用一个正方形把两个长方形框起来，希望这个正方形的边长最短

*Solution*

两个长方形，要么是平行着放，要么是叠起来放

*Code*

```cpp
#include <iostream>
#include <cstdio>
#include <algorithm>

using namespace std;

int main()
{
	int T;
	scanf("%d", &T);

	while (T--)
	{
		int a, b;
		scanf("%d%d", &a, &b);
	
		int len = min(max(a, b + b), max(a + a, b));
		printf("%d\n", len * len);
	}

	return 0;
}

```



#### B. Honest Coach

*Description*

给出n个数，分成两组，要求一组中的最大值和另外一组的最小值之差的绝对值最小

*Solution*

sort一遍，遍历一遍寻找最小的相邻两个数的差值

*Code*

```cpp
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

const int N = 55;

int a[N];

int main()
{
	int T;
	cin >> T;

	while (T--)
	{
		int n;
		cin >> n;

		for (int i = 0; i < n; i++) cin >> a[i];

		sort(a, a + n);

		int ans = 0x3f3f3f3f;
		for (int i = 1; i < n; i++)
		{
			int t = a[i] - a[i - 1];
			ans = min(ans, t);
		}

		cout << ans << endl;
	}

	return 0;
}
```



#### C. Similar Pairs

*Description*

给出一个规则，如果x，y相似，那么x，y同时是奇数或者同时是偶数，或者两者之间差的绝对值是1

给出T个数组，判断这个数组能否划分出很多对数，每一对数都是相似的

*Solution*

这道题的实质是，判断奇数和偶数的个数，首先数据保证n个数，n是偶数。如果有偶数个偶数，那一定有偶数的奇数，即使是0个。如果奇数个偶数，那一定有奇数个奇数，此时，我们要判断数组中是否存在相邻的一对奇偶数，他们的差值是1。在遍历数组的时候，先sort一遍，统计偶数个数的时候，顺便查看一下是否存在相邻两个数差值是1

*Code*

```cpp
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 55;

int a[N];

int main()
{
	int T;
	cin >> T;

	while (T--)
	{
		int n;
		cin >> n;

		for (int i = 0; i < n; i++) cin >> a[i];

		sort(a, a + n);

		int odd = 0, even = 0;
		bool delta = false;

		for (int i = 0; i < n; i++)
		{
			if (a[i] % 2 == 0) even++;
			else odd ++;

			if (i > 0 && (a[i] - a[i - 1]) == 1) delta = true;
		}

		if (even % 2 == 0) {cout << "YES" << endl; continue;}
		else if (delta) {cout << "YES" << endl; continue;}
		else {cout << "NO" << endl; continue;}
	}

	return 0;
}
```



#### D. Buying Shovels

*Description*

小伙子要买n双鞋，鞋店的鞋子都是整盒出售的，鞋店有k种包装盒，分别有1双鞋，2双鞋，...，k双鞋，这些种类的包装。小伙子只能买一种包装盒类型，刚好买n双鞋子，希望买的盒数最小。

*Solution*

每个盒子里的鞋数，必要要能整除n，希望买的盒数最小，就是希望盒子里的鞋数尽可能大。问题转化为，求不大于k的n的最大约数

*Code*

```cpp
#include <bits/stdc++.h>

using namespace std;

int T;
int n, k;

int main()
{
	scanf("%d", &T);
	while (T--)
	{
		scanf("%d%d", &n, &k);

		int ans = n;                      //最差情况下买n盒，每盒1个鞋子
		for (int i = 1; i <= n / i; i++)  //找到小于k的最大约数，i是n的约数，就判断i和n/i是否满足条件，更新ans
			if (n % i == 0) 
			{
				if (i <= k) ans = min(ans, n / i);

				if (n / i <= k) ans = min(ans, i);
			}

		printf("%d\n", ans);
	}

	return 0;
}
```



#### E. Polygon

*Description*

Polygon，多边形。有一个二维数组，初始每个格子都是0。二维数组的上方和左侧各有一排大炮。大炮发射导弹，直线运行，遇到边界或者遇到前面发射的炮弹就会停下来，占一个格子，格子置1。给出T组数据，都是些棋谱，问这个棋谱是不是这些大炮打出来的。

*Solution*

如果棋谱中，存在一个炮弹，既能继续往右，又能继续往下，那么这个棋谱就不合法，因为炮弹是停不下来的，要要再飞一会。问题转化为，二维数组中，是否存在一个1，右边是0，下边也是0。

*Code*

```cpp
#include <iostream>
#include <cstdio>
#include <cstring>

using namespace std;

const int N = 55;

string g[N];
int n;

int main()
{
	int T;
	cin >> T;

	while (T--)
	{
		cin >> n;

		for (int i = 0; i < n; i++)
			cin >> g[i];

		bool success = true;
		for (int i = 0; i < n - 1; i++)
			for (int j = 0; j < n - 1; j++)
				if (g[i][j] == '1' && g[i + 1][j] == '0' && g[i][j + 1] == '0')
					success = false;

		if (success) cout << "YES" << endl;
		else cout << "NO" << endl;
	}

	return 0;
}
```



#### F. Spy-string

*Description*

n个字符串，是由一个模板串变化过来的，每一个字符串和模板串相比，最多相差一个字符。模板串可能有多种情况，请给出一个合理情况，如果不存在，输出 -1

*Solution*

以a1位原型构造模板串，可以枚举a1的每一位，用 a - z 替换。用这个构造出来的模板串，去和剩下的 n - 1个字符串去比较，如果和某一个字符串有两个字符不同，当前构造就是非法的。如果配对一遍，发现n-1个字符都合法，那么当前构造的模板串就是合法的，输出走人。最后毛线都没输出，就输出 -1 ，over

*Code*

```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 15;

string a[N];
int n, m;
int T;

void solve()
{
	//取出来第一个字符串
	string ans = a[0];

	//模拟变化一位，然后和每一个进行匹配，看有没有不同字符超过两个的，超过就不是合法方案
	for (int i = 0; i < m; i++)
	{
		char t = ans[i]; //备份原字符
		for (char c = 'a'; c <= 'z'; c++)
		{
			ans[i] = c;
			
			bool flag = true;
			for (int j = 0; j < n; j++)
			{
				int cnt = 0;
				for (int k = 0; k < m; k++)
					if (ans[k] != a[j][k]) cnt++;
				
				//printf("---%d\n", cnt);

				if (cnt >= 2)
				{
					//说明ans变化的这一位不是合法方案，就不用枚举后面的字符串了
					//换一个字符
					flag = false;
					break;
				}
			}
		
			if (flag)
			{
				cout << ans << endl;
				return ;
			}
		}
		ans[i] = t;
	}

	printf("-1\n");
	return ;
}

int main()
{
	scanf("%d", &T);

	while (T--)
	{
		scanf("%d%d", &n, &m);
		for (int i = 0; i < n; i++) cin >> a[i];

		solve();
	}

	return 0;
}
```



#### G. A/B Matrix

*Description*

一个二维矩阵，由 0 1 组成，每一行都有a个1，每一列都有b个1。请你构造出合法的矩阵，并输出，如果无法构造，就输出 -1

*Solution*

每一行都有a个1，每一列都有b个1，这个 1 不仅在行中出现，还在列中出现。那么按照行统计1的个数和按列统计1的个数要一致，否则矛盾。a * n = b * m，这是判断有无解的方法，下面要进行构造怎么放这些1

第一感觉，每一行的1要放在一起，没必要散开放，加大对自己的伤害。那么问题就转化为，连续的1，连续1，一行一行的，怎么错位摆放能够满足每一列刚好是b个1，这个错位，术语叫“位移”。n个位移，刚好到第m列的时候是一个轮回。假设位移为d，(d * n) % m == 0。从小到大枚举，求d。

*Code*

```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 55;

int g[N][N];
int n, m, a, b;
int T;

int main()
{
	scanf("%d", &T);

	while (T--)
	{
		scanf("%d%d%d%d", &n, &m, &a, &b);

		if (n * a != m * b)
		{
			printf("NO\n");
			continue;
		}
		else
		{
			memset(g, 0, sizeof g);
			
			int d = 1;
			while (1)
			{
				if (d * n % m == 0) break;
				d++;
			}

			for (int i = 0, dx = 0; i < n; i++, dx += d)
				for (int j = 0; j < a; j++)
					g[i][(j + dx) % m] = 1;
				
			printf("YES\n");
			for (int i = 0; i < n; i++)
			{
				for (int j = 0; j < m; j++)
					printf("%d", g[i][j]);
				puts("");
			}
		}
	}

	return 0;
}
```



#### H. Binary Median

*Description*

一共有 2^m 个字符串，是由 01 组成的，给出n个字符串，把这 n 个 01 字符串从中去掉，求最后的中位数是什么

*Solution*

这道题的特点是，字符串是二进制的 01 字符串，不能直接求中位数，需要转化成10进制计算。

初始中位数是 ans = (k-1)/2，如果扣掉的数字，比ans小，ans往右移动一个，如果新的中位数，是之前被扣掉的，就要继续往右移动一位，再判断。判断是否被扣掉，开hash处理。

这里还分一下，总数是奇数的时候，中位数只有一个；总数是偶数的时候，中位数是中间两个数偏左边的那个。这就造成一个现象。偶数的时候，如果扣掉的数，在ans右边，扣完是奇数个，ans不用动。奇数的时候，如果扣掉的数，在ans左边，扣完是偶数个，ans也不用动。

*Code*

```cpp
#include <bits/stdc++.h>

using namespace std;

typedef long long LL;

vector<LL> q;
map<LL, LL> mp; //开hash
int T;
int n, m;

LL check(string s)
{
	LL t = 0, p = 1;
	for (int i = s.size() - 1; i >= 0; i--)
	{
		t += (s[i] - '0') * p;
		p *= 2;
	}

	return t;
}

int main()
{
	scanf("%d", &T);
	while (T--)
	{
		q.clear(); mp.clear();
		
		scanf("%d%d", &n, &m);
		string s;
		for (int i = 0; i < n; i++)
		{
			cin >> s;
			LL t = check(s);
			
			//printf("---%d\n", t);
			q.push_back(t);
		}

		LL k = 1LL << m;
		LL ans = (k - 1) / 2;  //初始中位数

		//处理n个删除的字符串
		for (int i = 0; i < n; i++)
		{
			mp[q[i]] = 1;  //标记这个数出现过
			
			if (k % 2 == 0)  //even的中位数有两个，我们默认取靠前的那个
			{
				if (q[i] <= ans) //删掉数的中位数前面，中位数往后移动，如果移动一位后，发现这个数也是被删掉的，还要继续移动
				{
					ans++;
					while (mp[ans]) ans++;
				}
				//在ans后面删掉，中位数不受影响
				
			}

			if (k % 2 == 1)
			{
				//在ans后面，中位数往前移动一位
				if (q[i] >= ans)
				{
					ans--;
					while (mp[ans]) ans--;
				}
				//在ans前面，中位数不受影响
			}

			k--; //维护总个数
			//printf("---%lld\n", ans);
		}

		string st;
		for (int i = 0; i < m; i++)
			if (ans >> i & 1) st += '1';
			else st += '0';

		reverse(st.begin(), st.end());
		cout << st << endl;
	}

	return 0;
}
```

