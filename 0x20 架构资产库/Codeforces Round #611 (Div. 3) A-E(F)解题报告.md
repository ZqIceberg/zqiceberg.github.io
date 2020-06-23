### A. Minutes Before the New Year

*Description*

在迎接新年到来的夜晚，给出现在的时间，求距离跨年敲钟有多少分钟

*Solution*

一整天的时间转化成分钟，减去现在的时间，就是剩下的分钟数

*Code*

```cpp
#include <bits/stdc++.h>

using namespace std;

int T;
int hh, mm;

int main()
{
	scanf("%d", &T);

	int sum = 24 * 60;
	while (T--)
	{
		scanf("%d%d", &hh, &mm);

		printf("%d\n", sum - hh * 60 - mm);
	}

	return 0;
}
```



### B. Candies Division

*Description*

k个小朋友分n块糖，每个人分得到的糖，比较平均，要么是a，要么是a+1，不能有其他的分法。并且得到a+1块糖的人数不能超过k/2，问最多会用掉多少块糖

*Solution*

每个人平均分一波（注意是一波，不是一块，把n能整除k的部分都分掉，剩下的就是一些尾巴，当前不够k块的情况）。当剩下的糖，小于k/2块，那么就直接分掉；如果剩下的糖，大于k/2，最多只能用 k/2块。

锻炼分情况讨论

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

		//每个人平均先分一个或者0个
		int sum = (n / k) * k;

		int left = n - sum;
		if (left <= k / 2) sum += left;
		else sum += k / 2;

		printf("%d\n", sum);
	}

	return 0;
}
```



### C. Friends and Gifts

*Description*

n个人，编号是1~n，每个人都要送给另外一个人礼物，注意不能自己送给自己。现给出一个序列，其中有的数列是0，表示这个人还没想好送谁礼物，非0，表示这个人要送给谁礼物。请你把0的部分填写完整

*Solution*

官方题解是图论，然并没看懂。自己的想法大致如下，以第一个样例为例，5 0 0 2 4

没送出去礼物的：2 3

没收到礼物的：1 3

问题就转换为把“1，3”填写到符合条件的第2个空和第3个空上，注意自己不能送给自己。

那么就这样逐个的去放，如果遇到自己送给自己的情况，没收到礼物的序列中，就和下一位swap一下再放，就肯定是合法的。如果到了最后一个数的时候，发现自己送给自己，身后又没有人可以交换。就和序列当中第一个已经放置的数字，进行交换

*Code*

```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 2e5 + 10;

int a[N];
int A[N], B[N];
bool st[N];
int n;

int main()
{
	scanf("%d", &n);
	
	int idx = 0;
	for (int i = 1; i <= n; i++)
	{
		scanf("%d", &a[i]);
		st[a[i]] = true;
		
		if (a[i] == 0) A[idx++] = i;
	}

	int pos = 0;
	for (int i = 1; i <= n; i++)
		if (!st[i]) B[pos++] = i;

	/*
	for (int i = 0; i < idx; i++) printf("%d ", A[i]);
	puts("");
	for (int i = 0; i < pos; i++) printf("%d ", B[i]);
	puts("");
	*/

	//A[] 没送出去的
	//B[] 没收到的
	for (int i = 0; i < idx; i++)
	{
		if (A[i] == B[i])
		{
			if (i != idx - 1)
			{
				swap(B[i], B[i + 1]);
				a[A[i]] = B[i];
			}
			else
			{
				a[A[i]] = B[i];
				swap(a[A[0]], a[A[i]]);	
			}
		}
		else
			a[A[i]] = B[i];
	}

	for (int i = 1; i <= n; i++) printf("%d ", a[i]);
	puts("");

	return 0;
}
```



### D. Christmas Trees

*Description*

xi序列，表示圣诞树的位置，现在有m个人要一次插入的x轴上，要保证每个人距离最近的树的距离之和是最小的。注意，树、人的位置，不能重叠

*Solution*

挨着树左右两边放的贪心策略是正确的，然后，把初始状态下可以插入的positon，维护到队列里，然后就来bfs，每次取队头出来，插入，同时维护一下距离树的距离累计到sum里。插入之后，要把下一个可以插入的位置维护进队列里。

这道题x和y的取值范围非常大，开数组维护状态是爆空间的，要开hash，可以用map<int, int>

比赛的时候，最后一分钟，h.find(x) != h.end()，这个函数想不好怎么写了，然后写了一个h[x] == 0来判断这个点是否可用，结果是WA在了第三个测试点上。结束之后，看了一下函数怎么写，改掉这块就瞬间AC。冲D题的路上逐渐出现曙光

同时，告诉我们，比赛时候用的知识点和课本上的知识点不是一一对应的，轻重缓急是不对应的。然后，整个比赛的过程，就和我们做了很多年运维是一样的，经验丰富、工具用的熟悉（算法竞赛就是基本算法和代码用的熟不熟，能不能各种角度用），有经验的运维听、摸服务器大致能判断一下哪里出现问题，或者可能要出问题的现象。这些道理都很相似。比如这个D题，我知道要维护x轴上的点能不能用，第一想法是bool st[]，然后，遇到x的取值范围有负数，就用偏移量的方式处理一下。在本地跑，可以，但是CF上就CE。开map<int, int>，判断一个点是否可用的方式，如果用h[x] == 0来判断会产生很多多余的h[x]=0，这个lyd大神在书中有讲，看来这些坑大神都亲身经历过。

*Code*

```cpp
#include <bits/stdc++.h>

using namespace std;

typedef pair<int, int> PII;
typedef long long LL;

const int M = 2e5 + 10;
const int N = 2e9 + 10, delta = 1e9;

int a[M];
map<int, int> h; //开哈希记录哪些点有树，或者已有人
int n, m;
vector<int> ans; //维护人的位置，记录答案
LL sum;
queue<PII> q; //维护可以放置的点

int main()
{
	scanf("%d%d", &n, &m);

	for (int i = 0; i < n; i++)
	{
		scanf("%d", &a[i]);
		h[a[i] + delta] = 1; //树的位置标记掉
	}

	for (int i = 0; i < n; i++)
	{
		int x = a[i];
		if (h.find(x - 1 + delta) == h.end()) q.push(make_pair(x, x - 1));
		if (h.find(x + 1 + delta) == h.end()) q.push(make_pair(x, x + 1));
	}

	while (!q.empty())
	{
		PII t = q.front();
		q.pop();
		
		int tree = t.first, pos = t.second;

		if (h[pos + delta] == 1) continue;

		ans.push_back(pos);
		h[pos + delta] = 1;
		sum += abs(pos - tree);
		//printf("--%d %d\n", tree, pos);

		if (ans.size() == m) break;

		if (h.find(pos + 1 + delta) == h.end()) q.push(make_pair(tree, pos + 1));
		if (h.find(pos - 1 + delta) == h.end()) q.push(make_pair(tree, pos - 1));
	}

	printf("%lld\n", sum);
	for (int i = 0; i < ans.size(); i++) printf("%d ", ans[i]);
	puts("");

	return 0;
}
```



### E. New Year Parties

*Description*

为了庆祝新年，每个人都跑到房子里（房子遍布在1-n的区间上），每个房间有一个坐标x，每个人，只能朝前走一步或者朝后走一步。人跑到x-1的位置，或者x+1的位置上。求如何移动，能够使用最少的房子，求如何移动，能够使用最多的房子

*Solution*

使用最少的房子，以三个有人的相邻房子为例，最好的情况情况，把三个房子的人凑到中间的房子里去。那么就是以3个长度为单位，最多能优化到一个房子里

使用最多的房子，如果一个房子里有人，先分一个人给前面的房子，然后看房子里如果还剩下2人以上，就再分一个人给后面的房子。（对于“0 2 2 1”，演变成"1 2 1 1"）

（这道题，第二天晚上做了1个小时，也是没作对，虽然我做过一遍了。主要问题是没有想到正确的贪心思路，然后初步给出的贪心想法，就会给测试点一个一个的卡出问题，然后在原思路上继续修改也无法调对。用正确的贪心思路，改过之后，一提交马上就跑出AC）

*Code*

```cpp
#include <bits/stdc++.h>

using namespace std;

const int N = 2e5 + 10;

int n;
int a[N];
int a1[N], a2[N];

int main()
{
	scanf("%d", &n);
	
	int x;	
	for (int i = 1; i <= n; i++) 
	{
		scanf("%d", &x);
		a[x]++;
	}

	memcpy(a1, a, sizeof a1);
	memcpy(a2, a, sizeof a2);

	int cnt1 = 0;
	for (int i = 1; i <= n; i++)
	{
		if (a1[i] != 0) 
		{
			cnt1++;
			i += 2;
		}
	}

	for (int i = 1; i <= n; i++)
	{
		if (a2[i] == 0) continue;

		if (a2[i - 1] == 0)
		{
			a2[i - 1] ++;
			a2[i] --;
		}

		if (a2[i] >= 2)
		{
			a2[i + 1] ++;
			a2[i]--;
		}
	}

	int cnt2 = 0;
	for (int i = 0; i <= n + 1; i++)
		if (a2[i]) cnt2++;

	printf("%d %d\n", cnt1, cnt2);

	return 0;
}
```