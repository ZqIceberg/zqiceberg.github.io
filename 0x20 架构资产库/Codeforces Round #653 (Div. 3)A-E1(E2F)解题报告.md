#### A. Required Remainder

*Description*

找到小于等于n，%x 余 y 的最大值

*Solution*



*Code*

```cpp
#include <bits/stdc++.h>

using namespace std;

int T;
int x, y, n;

int main()
{
	scanf("%d", &T);

	while (T--)
	{
		scanf("%d%d%d", &x, &y, &n);

		int t = n % x;
		if (t >= y)
		{
			printf("%d\n", n - (t - y));
		}
		else
		{
			printf("%d\n", n - t - (x - y));
		}
	}

	return 0;
}
```



#### B. Multiply by 2, divide by 6

*Description*

只能对一个数进行两种操作，乘以2，或者除以6。进过最少步数，能得到1，如果超过n步还不能得到1，输出-1

*Solution*

这道题耗费了很长时间，影响了D题

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
		
		if (n == 1)
		{
			printf("0\n");
			continue;
		}

		int n2 = 0, n3 = 0;
		int t = n;
		while (t % 2 == 0)
		{
			n2++;
			t /= 2;
		}

		while (t % 3 == 0)
		{
			n3++;
			t /= 3;
		}

		if (t > 1 || n2 > n3)
		{
			printf("-1\n");
			continue;
		}

		printf("%d\n", n3 - n2 + n3);
	}

	return 0;
}
```



#### C. Move Brackets

*Description*

一个左右括号序列，每一次操作，你可以把一个括号移动最前面，或者最后面。求最少移动次数，能得到一个合法括号序列。

*Solution*

一个合法的前缀，一定是左括号的个数大于等于右括号的个数

*Code*

```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 55;

int tt;
int ans;
int T, n;
string s;

int main()
{
	cin >> T;
	
	while (T--)
	{
		cin >> n;
		cin >> s;

		tt = 0, ans = 0;
		for (int i = 0; i < n; i++)
		{
			if (s[i] == '(') tt++;
			
			if (s[i] == ')')
			{
				if (tt == 0) ans++;
				else tt--;
			}
		}

		cout << ans << endl;
	}

	return 0;
}
```



#### D. Zero Remainder Array

*Description*

给一个序列，每一个数，可以加上x后，让这个数可以被k整除。但是x的变化是有规定的，x从0开始，每给一个数赋值，x就要++。问x最少变化多少次，能够得让序列中所有的数都被k整除

*Solution*

可悲可悲的一题，影响了一星期的状态

*Code*

```cpp
#include <bits/stdc++.h>

using namespace std;

typedef long long LL;

int T;
int n, k;

int main()
{
	scanf("%d", &T);

	while (T--)
	{
		scanf("%d%d", &n, &k);

		map<int, int> mp;
		while (n--)
		{
			int x;
			scanf("%d", &x);

			if (x % k == 0) continue;
			
			int r = k - x % k;
			mp[r]++;
		}

		LL ans = 0, x = 0;
		map<int, int>::iterator it;
		for (it = mp.begin(); it != mp.end(); it++)
		{
			int a = (*it).first, b = (*it).second;
			ans = max(ans, a + 1LL * (b - 1) * k);
		}

		if (ans == 0) printf("0\n");
		else printf("%lld\n", ans + 1);
	}	

	return 0;
}
```



#### E1. Reading Books (easy version)

*Description*

n组数据，{t，a，b}，a == 1，表示Alice喜欢，b==1，表示Bob喜欢。t表示读这本书花费的时间。从这些书中选出一些书，保证Alice至少喜欢k本，Bob至少喜欢k本，并且让读书花费的时间尽可能的少。

*Solution*

这道题，提示我们，熟练运算前缀和

*Code*

```cpp
#include <bits/stdc++.h>

using namespace std;

const long long INF = 1e18;

int n, k;
vector<int> A, B, C;
vector<int> sumA, sumB, sumC;

void solve(vector<int> &a, vector<int> &suma)
{
	suma.push_back(0);

	for (int i = 0; i < a.size(); i++)
	{
		int t = suma.back() + a[i];
		suma.push_back(t);
	}
}

int main()
{
	scanf("%d%d", &n, &k);

	for (int i = 0; i < n; i++)
	{
		int t, a, b;
		scanf("%d%d%d", &t, &a, &b);

		if (a == 1 && b == 0) A.push_back(t);
		if (a == 0 && b == 1) B.push_back(t);
		if (a == 1 && b == 1) C.push_back(t);
	}

	sort(A.begin(), A.end());
	sort(B.begin(), B.end());
	sort(C.begin(), C.end());

	solve(A, sumA);
	solve(B, sumB);
	solve(C, sumC);

	long long ans = INF;
	for (int i = 0; i < min((int)sumC.size(), k + 1); i++) //取[0, k]个
	{
		int j = k - i;

		if (j < (int)sumA.size() && j < (int)sumB.size())
		{
			ans = min(ans, (long long)(sumC[i] + sumA[j] + sumB[j]));
		}
	}

	if (ans == INF) printf("-1\n");
	else printf("%lld\n", ans);

	return 0;
}
```



#### E2. Reading Books (hard version)

*Description*



*Solution*



*Code*



#### F. Cyclic Shifts Sorting

*Description*



*Solution*



*Code*