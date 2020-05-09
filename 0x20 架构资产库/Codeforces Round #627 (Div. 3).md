### Codeforces Round #627 (Div. 3) Editorial

#### 1324A - Yet Another Tetris Problem
```cpp
for i in range(int(input())):
	n = int(input())
	cnto = sum(list(map(lambda x: int(x) % 2, input().split())))
	print('YES' if cnto == 0 or cnto == n else 'NO')
```

#### 1324B - Yet Another Palindrome Problem
```cpp
for i in range(int(input())):
	n = int(input())
	s = list(map(int, input().split()))
	ok = False
	for i in range(n):
		for j in range(i + 2, n):
			if s[i] == s[j]: ok = True
	print('YES' if ok else 'NO')
```

#### 1324C - Frog Jumps
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
		string s;
		cin >> s;
		vector<int> pos;
		pos.push_back(0);
		for (int i = 0; i < int(s.size()); ++i) {
			if (s[i] == 'R') pos.push_back(i + 1);
		}
		pos.push_back(s.size() + 1);
		int ans = 0;
		for (int i = 0; i < int(pos.size()) - 1; ++i) {
			ans = max(ans, pos[i + 1] - pos[i]);
		}
		cout << ans << endl;
	}
	
	return 0;
}
```

#### 1324D - Pair of Topics
```cpp
#include <bits/stdc++.h>

using namespace std;

int main() {
#ifdef _DEBUG
	freopen("input.txt", "r", stdin);
//	freopen("output.txt", "w", stdout);
#endif
	
	int n;
	cin >> n;
	vector<int> a(n), b(n);
	for (auto &it : a) cin >> it;
	for (auto &it : b) cin >> it;
	vector<int> c(n);
	for (int i = 0; i < n; ++i) {
		c[i] = a[i] - b[i];
	}
	sort(c.begin(), c.end());
	
	long long ans = 0;
	for (int i = 0; i < n; ++i) {
		if (c[i] <= 0) continue;
		int pos = lower_bound(c.begin(), c.end(), -c[i] + 1) - c.begin();
		ans += i - pos;
	}
	
	cout << ans << endl;
	
	return 0;
}
```

#### 1324E - Sleeping Schedule

#### 1324F - Maximum White Subtree

