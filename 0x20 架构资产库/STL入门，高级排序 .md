先照着进阶指南，这个部分，统一介绍一遍

### 3440 - 统计数字
```cpp
#include <iostream>
#include <cstdio>
#include <map>

using namespace std;

map<int, int> mp;
int n;

int main()
{
	scanf("%d", &n);

	int cnt = 0;
	while (n--)
	{
		int x;
		scanf("%d", &x);

		mp[x]++;
		if (mp[x] > cnt) cnt = mp[x];
	}

	for (map<int, int>::iterator it = mp.begin(); it != mp.end(); it++)
	{
		if (it->second == cnt) printf("%d ", it->first);
	}
	puts("");

	return 0;
}

```

### 3460 - 战士
```cpp
#include <iostream>
#include <cstdio>
#include <vector>

using namespace std;

const int N = 1e5 + 10;

vector<int> h[N];

int main()
{
	int n, m;
	scanf("%d%d", &n, &m);

	while (m--)
	{
		int op, x, y;
		scanf("%d%d%d", &op, &x, &y);

		if (op == 1)
		{
			h[x].push_back(y);
		}
		if (op == 2)
		{
			printf("%d\n", h[x][y - 1]);
		}
	}

	return 0;
}
```

### 3470 - multiset常规操作
```cpp
#include <iostream>
#include <cstdio>
#include <set>

using namespace std;

multiset<int> st;

int main()
{
	int n;
	scanf("%d", &n);

	multiset<int>::iterator it;
	while (n--)
	{
		
		int op;
		int x;
		
		scanf("%d", &op);
		if (op == 1)
		{
			scanf("%d", &x);
			st.insert(x);
		}
		if (op == 2)
		{
			scanf("%d", &x);
			it = st.find(x);
			if (it == st.end()) continue;
			
			st.erase(it);
		}
		if (op == 3)
		{
			scanf("%d", &x);
			it = st.upper_bound(x);

			if (it == st.end()) printf("max\n");
			else printf("%d\n", *it);
		}
		if (op == 4)
		{
			scanf("%d", &x);
			it = st.lower_bound(x);

			if (it == st.begin()) printf("min\n");
			else printf("%d\n", *(--it));
		}
		if (op == 5)
		{
			if (st.empty()) continue;
			
			it = st.end();
			--it;
			
			printf("%d\n", *it);
			st.erase(it);
		}
		if (op == 6)
		{
			if (st.empty()) continue;

			it = st.begin();
			printf("%d\n", *it);
			st.erase(it);
		}
	}
	
	for (it = st.begin(); it != st.end(); it++)
		printf("%d ", *it);
	puts("");
	
	return 0;
}
```