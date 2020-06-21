### A. Sum of Round Numbers

*Description*

一个自然数n，除了第一位非零，其他位都是零的话，n就是一个Round Number。给你一个数x，问他可有几个Round Number组成，输出个数，并输出具体方案

*Solution*

看样例5009，取出5000，9。问题转化为，从后往前逐位遍历，非零的就存起来，个位就乘1，十位乘10，百位乘$10^2$

*Code*

```cpp
#include <iostream>
#include <vector>

using namespace std;


int main()
{
	int T;
	cin >> T;

	while (T--)
	{
		int n;
		cin >> n;

		vector<int> ans;
		int pow = 1;
		while (n)
		{
			int t = n % 10;
			if (t)
				ans.push_back(t * pow);

			n /= 10;
			pow *= 10;
		}

		cout << ans.size() << endl;
		for (int i = 0, len = ans.size(); i < len; i++)
			cout << ans[i] << ' ';
		puts("");
	}

	return 0;
}
```



### B. Same Parity Summands

*Description*

给出整数n，n可以由k个数求和组成，这k个数要同时为奇数，或同时为偶数。如果存在，输出YES，输出具体方案。如果不存在输出NO。

*Solution*

这样构造偶数的情况：2 2 2 2 x（k-1个2，x是偶数），奇数的情况：1 1 1 1 x（k-1个奇数，x是奇数）。

*Code*

```cpp
#include <iostream>
#include <cstdio>

using namespace std;

int main()
{
	int T;
	cin >> T;

	while (T--)
	{
		int n, k;
		cin >> n >> k;

		bool success = false;
		//先判断偶数
		int t = n - 2 * (k - 1);
		if (t % 2 == 0 && t > 0)
		{
			success = true;
			cout << "YES" << endl;
			for (int i = 1; i <= k - 1; i++)
				printf("2 ");
			printf("%d\n", t);

			continue;
		}

		int odd = n - (k - 1);
		if (odd % 2 == 1 && odd > 0)
		{
			success = true;
			cout << "YES" << endl;
			for (int i = 1; i <= k - 1; i++)
				printf("1 ");
			printf("%d\n", odd);

			continue;
		}

		if (!success) cout << "NO" << endl;
	}

	return 0;
}
```



### C. K-th Not Divisible by n

*Description*

输出第k个不能被n整除的数

*Solution*

比如 n = 3, k = 7,   1 2 **3** 4 5 **6** 7 8 **9** 10，这个是周期性质的出现能被n整除，每一个周期里，有n-1个不能被整除的数字，1个可以被整除的数字。注意要的是第k个不能被整除的数。
$$
k + \frac {k - 1}{n - 1}， k加上小于k的能被n整除的个数，就得到第k个数的真实大小
$$
*Code*

```cpp
#include <iostream>

using namespace std;

int main()
{
	int T;
	cin >> T;

	while (T--)
	{
		int n, k;
		cin >> n >> k;

		int need = (k - 1) / (n - 1);
		cout << k + need << endl;
	}

	return 0;
}
```



### D. Alice, Bob and Candies

*Description*

一个数组，A从左往右取数，B从右往左取数，每一次最少取一个数，A先手。当前取数的一方，不断取数，直到他累积取数的大小比另一方累积取数的大小大的时候，就停下来，另外一方继续。整个游戏，当无法满足取数条件时，游戏结束。输出游戏进行了几步，A一共取了多少，B一共取了多少。

*Solution*

A先取，双指针模拟A和B取到第几个数，用last维护上一个人取了多少数，直到两个指针撞上结束游戏

*Code*

```cpp
#include <iostream>
#include <cstdio>

using namespace std;

const int N = 1010;

int A[N];

int main()
{
	int T;
	cin >> T;

	while (T--)
	{
		int n;
		scanf("%d", &n);

		for (int i = 1; i <= n; i++) scanf("%d", &A[i]);

		int step = 1;
		int last = A[1];
		int a = last, b = 0;
		int l = 1, r = n + 1;
		while (l < r)
		{
			{
			//b继续吃
			int sum = 0;
			while (sum <= last)
			{
				r--;	
				if (l >= r) break;  //碰面就break出来
				
				sum += A[r];
			}

			b += sum;
			if (sum) step++;

			last = sum;
			}

			{
			//a继续吃
			int sum = 0;
			while (sum <= last)
			{
				l++;
				if (l >= r) break;
				
				sum += A[l];
			}

			a += sum;
			if (sum) step++;

			last = sum;
			}
		}

		printf("%d %d %d\n", step, a, b);
	}

	return 0;
}
```



### E. Special Elements

*Description*

一个数组a，$a_i$是某段子序列之和，那么$a_i$就是special的，求数组中有几个special的数字

*Solution*

用cnt<int>维护$a_i$的出现次数，双指针，枚举每一位为起点，区间长度大于等于2的区间，如果这个区间的和sum超过 n ，就可以continue枚举下一个起点。ans += sum在数组中出现的次数，把sum出现的次数置零。

*Code*

```cpp
#include <iostream>
#include <vector>

using namespace std;

int main()
{
	int T;
	cin >> T;

	while (T--)
	{
		int n;
		cin >> n;

		vector<int> a(n);
		vector<int> cnt(n + 1);

		for (int i = 0; i < n; i++)
		{
			cin >> a[i];
			cnt[a[i]]++;
		}

/*
		for (int l = 0; l < n; ++l) {
			int sum = 0;
			for (int r = l; r < n; ++r) {
				sum += a[r];
				if (l == r) continue;
				if (sum <= n) {
					ans += cnt[sum];
					cnt[sum] = 0;
				}
			}
		}
*/

		int ans = 0;
		for (int l = 0; l < n; l++)
		{
			int sum = a[l];
			for (int r = l + 1; r < n; r++)
			{
				sum += a[r];
				if (sum > n) continue;  //ai <= n  所以sum超过n就不要考虑了
				
				ans += cnt[sum];
				cnt[sum] = 0;
			}
		}

		cout << ans << endl;
	}

	return 0;
}
```



### F. Binary String Reconstruction

*Description*

一个字符串，相邻两个字符为一组，一组中，0个1，记为n0类型，1个1，记为n1类型，2个1，记为n2类型。题目给出n0，n1，n2的值，构造出合法的字符串

*Solution*

先构造出n1个“10”，令s = 101010...，然后在s[1]插入n0个0，在s[0]插入n2个1。这里需要熟悉string的函数，string(n2, '1')生成一个n2个'1'的字符串，s.insert(0, string(n2, 1))，在0位置插入n2个1

*Code*

```cpp
#include <iostream>
#include <cstring>

using namespace std;

int main()
{
	int T;
	cin >> T;

	while (T--)
	{
		int a, b, c;
		cin >> a >> b >> c;

		if (b == 0)  //没有01
		{
			if (a != 0)
				cout << string(a + 1, '0') << endl;
			else
				cout << string(c + 1, '1') << endl;

			continue;
		}

		string s = "";
		for (int i = 0; i <= b; i++)
		{
			if (i & 1) s += '0';
			else s += '1';
		}

		s.insert(1, string(a, '0'));
		s.insert(0, string(c, '1'));

/*
		while (a--)
		{
			s.insert(1, "0");
		}

		while (c--)
		{
			s.insert(0, "1");
		}
*/
		cout << s << endl;
	}

	return 0;
}
```



### G. Special Permutation

*Description*

一个序列，1~n组成，两两之间差的绝对值 在[2, 4]之间，给出n，如果能构造出合法方案，输出，否则输出-1

*Solution*

1个数，2个数，3个数，都是无法构造的。

先是从n往下，把奇数输出，然后中间接“4  2”，右边继续接6, 8 ,10, ..., 不超过n的最大偶数

*Code*

```cpp
#include <iostream>

using namespace std;

int main()
{
	int T;
	cin >> T;

	while (T--)
	{
		int n;
		cin >> n;
		
		if (n < 4) {cout << -1 << endl;  continue;}

		for (int i = n; i >= 1; i--)
			if (i & 1) cout << i << ' ';
		cout << 4 << ' ' << 2 << ' ';

		for (int i = 6; i <= n; i += 2)
			cout << i << ' ';
		cout << endl;
	}

	return 0;
}
```