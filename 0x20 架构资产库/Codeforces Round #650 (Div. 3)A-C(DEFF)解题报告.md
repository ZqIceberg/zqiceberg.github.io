### A. Short Substrings

*Description*

原字符串 abac， 加密后的字符串 abbaac，加密方式是取出原字符串中每两个相邻的字符组成新的字符串。先给出加密后的字符串，求原字符串

*Solution*

b的第1个字符是a的第一个字符，之后b中奇数位置上的字符拼在一起就是a

*code*

```cpp
#include <bits/stdc++.h>

using namespace std;

int main()
{
	int T;
	scanf("%d", &T);

	while (T--)
	{
		string s;
		cin >> s;

		string ans;
		ans += s[0];

		for (int i = 1, len = s.size(); i < len; i += 2)
			ans += s[i];

		cout << ans << endl;
	}

	return 0;
}
```



### B. Even Array

*Description*

给定数组a[0 ... n-1]，所有的下标和这位存储的数字奇偶性相同，则称数组是good的。希望通过最少交换次数，将a变成good，请输出最小操作次数，如果无法变成，输出-1

*Solution*

for一遍，i是偶数/奇数、a[i]是偶数/奇数，这样对应的是OK的，对于对应的，i是偶数，a[i]是奇数；i是奇数，a[i]是偶数，对于不配对的情况，我们用两个变量维护偶数个下标和奇数个下标（偶数下标下存的是奇数）。如果两个变量相等，可以通过交换完成，如果不等，输出-1

*code*

```cpp
#include <bits/stdc++.h>

using namespace std;

int main()
{
	int T;
	scanf("%d", &T);

	while (T--)
	{
		int n;
		scanf("%d", &n);

		int a = 0, b = 0;
		for (int i = 0; i < n; i++)
		{
			int x;
			scanf("%d", &x);

			if ((i & 1) != (x & 1))
			{
				if ((i & 1) == 0) a++;
				else b++;
			}
		}
		if (a != b) printf("-1\n");
		else printf("%d\n", a);
	}

	return 0;
}

```



### C. Social Distance

*Description*

01序列，长度n，两个1之间的间距最小为k，问序列中，还可以放置几个1

*Solution*

对于是0的位置可以放置成1，但要注意和左右两边1的距离。问题变为，连续多个0的区间，可以放置几个1。这个区间需要分几种情况讨论，区间左右两边是1，左边是1，右边是1的情况，这决定了边界位置能不能放置1。这道题，可以巧妙的构造一下，序列的左边加上 1000  右边加上0001，然后正常去找连续0的区间。此时看这个区间能放多少个1，先把尾部去掉k个0，保持和后方1的安全距离，然后 这样放，放0001 0001  0001 看能这样放，放几个完整的循环。[思路参考](https://codeforces.com/blog/entry/78864?#comment-643158)

因为这道题，赛场上打铁，订正用了两个小时。

*code*

```cpp
#include <bits/stdc++.h>

using namespace std;

int n, k;
int T;

int main()
{
	scanf("%d", &T);
	while (T--)
	{
		scanf("%d%d", &n, &k);
		
		string s;
		cin >> s;

		s = '1' + string(k, '0') + s + string(k, '0') + '1';

		int cnt = 0;
		for (int i = 0, j, len = s.size(); i < len; i++)
		{
			if (s[i] == '1')
			{
				for (j = i + 1; j < len; j++)
					if (s[j] == '1') break;
				
				//找到一个区间的两端'1....1'
				//每个区间看能放多少个 001 001
				//区间的尾部先去掉k个0，构造出和后面1的合法距离
				int a = i + 1, b = j - k - 1;
				cnt += (b - a + 1) / (k + 1);

				j--;
				i = j;
			}
		}

		printf("%d\n", cnt);
	}

	return 0;
}
```



### D. Task On The Board

*Description*

读题是目前第一大障碍。

*Solution*



*code*



### A. Short Substrings

*Description*

*Solution*

*code*



### A. Short Substrings

*Description*

*Solution*

*code*



### A. Short Substrings

*Description*

*Solution*

*code*

