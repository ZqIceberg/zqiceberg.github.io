#### A. Divisibility Problem

*Description*

两个整数a，b，把a最少加几次1，能够让a整除b

*Solution*

如果a%b == 0，加0次；否则，a至少加b - a % b，能够让a%b==0

*Code*

```cpp
#include <bits/stdc++.h>

using namespace std;

int T;
int a, b;

int main()
{
	scanf("%d", &T);

	while (T--)
	{
		scanf("%d%d", &a, &b);
		
		if (a % b == 0) 
			printf("0\n");
		else
			printf("%d\n", b - (a + b) % b);
	}

	return 0;
}
```



#### B. K-th Beautiful String

*Description*

长度为n的字符串，由n-2个a和2个b组成，问第k个字典序排列是什么

*Solution*

观察两个b的位置，我们发现第一个b从n-2到0，第二个b在第一个b的右边区间从右往左放置

```
aaabb
aabab
aabba
abaab
ababa
abbaa
baaab
baaba
babaa
bbaaa
```

我自己的思路是第1层1个b，第2层2个b，第3层3个b，然后看第k的排列，位于第几层，那么第一个b的位置就找到，然后把k减去前i层的和，就得到第2个b在本层的具体位置。然而这个思路，怎么写也不对，不好讨论k层刚好到第i层的边上，求和费死劲了

Mike的题解：枚举第一个b的位置i，i从n-2位置往左移，右边的区间 n-1-i是第二个b的活动范围，如果k <= n-1-i，那么就定位到答案了，否则，k -= n-1-i

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
		string s = string(n, 'a');

		//枚举左边b的位置
		for (int i = n - 2; i >= 0; i--)
		{
			if (k <= n - 1 - i)
			{
				s[i] = 'b';
				s[n - k] = 'b';
				cout << s << endl;
				break;
			}

			k -= n - 1 - i;
		}
	}

	return 0;
}
```



#### C. Ternary XOR

*Description*

一个三进制数，拆分成两个数之和，让这两个数的最大值最小，输出方案

*Solution*

从高位拆分，如果是2，一人一个1，如果是0，两人都是0，如果遇到1，那么这个1给第一个人，从这位起，后面所有的数都给第二个人，这样就保证了第1个人较大，并且是最小

*Code*

```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 1e5 + 10;

char s[N];
char a[N], b[N];
int T;
int n;

int main()
{
	scanf("%d", &T);
	while (T--)
	{
		scanf("%d%s", &n, s);
		
		int pos = -1;
		for (int i = 0; i < n; i++)
		{
			if (s[i] == '2') a[i] = '1', b[i] = '1';
			if (s[i] == '0') a[i] = '0', b[i] = '0';

			if (s[i] == '1') 
			{
				a[i] = '1', b[i] = '0';
				pos = i;
				break;
			}
		}
		
		if (pos != -1)
		{
			for (int i = pos + 1; i < n; i++)
			{	
				a[i] = '0';
				b[i] = s[i];
			}
		}

		for (int i = 0; i < n; i++) printf("%c", a[i]);
		puts("");
		for (int i = 0; i < n; i++) printf("%c", b[i]);
		puts("");
		
	}

	return 0;
}
```



#### D. Carousel

*Description*

旋转木马有n个图案，给这些图案涂色，相邻的不同图案，不能图相同的颜色。问最少用几种颜料能涂掉。如过用3种，那么颜色种类是1、2、3.

*Solution*

贪心，数学，构造，真是一道好题。我炸了而已

如果只有一种图案，图一种颜色

如果n是偶数，那么通过用1212的涂法，肯定满足条件，最多用了两种涂料。

如果n是奇数，那么需要分情况讨论。

一、相邻两个图案没有相同的

二、相邻两个图案有相同的，可以认为两个图案合并成一个图案，那么n就变成了偶数，就可以用刚才偶数的方案涂掉。

那么，问题来了

1.怎么判断只有一种颜色

2.n是奇数的时候，怎么发现有两个相邻图案是相同的，并且正确的涂上颜色。如果从第1个图案开始图，谁知道那哪个位置是要图那两个相同图案的呢

*Code*

```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 2e5 + 10;

int T, n;
int a[N];
int ans[N];

int main()
{
	scanf("%d", &T);

	while (T--)
	{
		scanf("%d", &n);
		for (int i = 0; i < n; i++) scanf("%d", &a[i]);

		map<int, int> mp;
		for (int i = 0; i < n; i++)
			if (mp.find(a[i]) == mp.end()) mp[a[i]] = 1;

		if (mp.size() == 1)
		{
			printf("1\n");
			for (int i = 0; i < n; i++) printf("1 ");
			puts("");

			continue;
		}

		if (n % 2 == 0)
		{
			printf("2\n");
			for (int i = 0, j = 1; i < n; i++, j ^= 1)
				ans[i] = j + 1;

			for (int i = 0; i < n ; i++) printf("%d ", ans[i]);
			puts("");
			continue;
		}
		else
		{
			int pos = -1;
			for (int i = 0; i < n; i++)
			{
				if (a[i] == a[(i + 1) % n] && pos == -1) pos = i;
			}

			if (pos == -1)
			{
				for (int i = 0, j = 1; i < n - 1; i++, j ^= 1)
					ans[i] = j + 1;
				ans[n] = 3;

				printf("3\n");
				for (int i = 0; i < n - 1; i++) printf("%d ", ans[i]);
				printf("3\n");
			}
			else
			{
				for (int i = pos + 1, j = 1; i < n; i++, j ^= 1)
					ans[i] = j + 1;
				for (int i = pos, j = 1; i >= 0; i--, j ^= 1)
					ans[i] = j + 1;

				printf("2\n");
				for (int i = 0; i < n; i++) printf("%d ", ans[i]);
				puts("");
			}
		}
	}

	return 0;
}
```

