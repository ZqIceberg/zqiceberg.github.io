### Codeforces Round #642 (Div. 3)
做出了ABC，是构造题和数学，D是数据结构，set不会用，E和F是dp


### 1353A - Most Unstable Array
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
		int n, m;
		cin >> n >> m;
		cout << min(2, n - 1) * m << endl;
	}
	
	return 0;
}
```

### 1353B - Two Arrays And Swaps
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
		int n, k;
		cin >> n >> k;
		vector<int> a(n);
		for (auto &it : a) cin >> it;
		vector<int> b(n);
		for (auto &it : b) cin >> it;
		sort(a.begin(), a.end());
		sort(b.rbegin(), b.rend());
		int ans = 0;
		for (int i = 0; i < n; ++i) {
			if (i < k) ans += max(a[i], b[i]);
			else ans += a[i];
		}
		cout << ans << endl;
	}
	
	return 0;
}
```

### 1353C - Board Moves
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
		int n;
		cin >> n;
		long long ans = 0;
		for (int i = 1; i <= n / 2; ++i) {
			ans += i * 1ll * i;
		}
		cout << ans * 8 << endl;
	}
	
	return 0;
}
```

### 1353D - Constructing the Array
```cpp
#include <bits/stdc++.h>

using namespace std;

struct cmp {
	bool operator() (const pair<int, int> &a, const pair<int, int> &b) const {
		int lena = a.second - a.first + 1;
		int lenb = b.second - b.first + 1;
		if (lena == lenb) return a.first < b.first;
		return lena > lenb;
	}
};

int main() {
#ifdef _DEBUG
	freopen("input.txt", "r", stdin);
//	freopen("output.txt", "w", stdout);
#endif
	
	int t;
	cin >> t;
	while (t--) {
		int n;
		cin >> n;
		set<pair<int, int>, cmp> segs;
		segs.insert({0, n - 1});
		vector<int> a(n);
		for (int i = 1; i <= n; ++i) {
			pair<int, int> cur = *segs.begin();
			segs.erase(segs.begin());
			int id = (cur.first + cur.second) / 2;
			a[id] = i;
			if (cur.first < id) segs.insert({cur.first, id - 1});
			if (id < cur.second) segs.insert({id + 1, cur.second});
		}
		for (auto it : a) cout << it << " ";
		cout << endl;
	}
	
	return 0;
}
```

### 1353E - K-periodic Garland
```cpp
#include <bits/stdc++.h>

using namespace std;

int main() {
#ifdef _DEBUG
	freopen("input.txt", "r", stdin);
//	freopen("output.txt", "w", stdout);
#endif
	
	auto solve = [](const string &s) {
		int n = s.size();
		int all = count(s.begin(), s.end(), '1');
		int ans = all;
		vector<int> res(n);
		int pref = 0;
		for (int i = 0; i < n; ++i) {
			int cur = (s[i] == '1');
			pref += cur;
			res[i] = 1 - cur;
			if (i > 0) res[i] += min(res[i - 1], pref - cur);
			ans = min(ans, res[i] + all - pref);
		}
		return ans;
	};
	
	int t;
	cin >> t;
	while (t--) {
		int n, k;
		string s;
		cin >> n >> k >> s;
		vector<string> val(k);
		int cnt = count(s.begin(), s.end(), '1');
		for (int i = 0; i < n; ++i) {
			val[i % k] += s[i];
		}
		int ans = 1e9;
		for (auto &it : val) ans = min(ans, solve(it) + (cnt - count(it.begin(), it.end(), '1')));
		cout << ans << endl;
	}
	
	return 0;
}
```

### 1353F - Decreasing Heights
```cpp
#include <bits/stdc++.h>

using namespace std;

#define forn(i, n) for (int i = 0; i < int(n); ++i)

const long long INF64 = 1e18;

int main() {
#ifdef _DEBUG
	freopen("input.txt", "r", stdin);
//	freopen("output.txt", "w", stdout);
#endif
	
	int t;
	cin >> t;
	while (t--) {
		int n, m;
		cin >> n >> m;
		vector<vector<long long>> a(n, vector<long long>(m));
		forn(i, n) forn(j, m) {
			cin >> a[i][j];
		}
		long long a00 = a[0][0];
		long long ans = INF64;
		forn(x, n) forn(y, m) {
			long long need = a[x][y] - x - y;
			if (need > a00) continue;
			a[0][0] = need;
			vector<vector<long long>> dp(n, vector<long long>(m, INF64));
			dp[0][0] = a00 - need;
			forn(i, n) forn(j, m) {
				long long need = a[0][0] + i + j;
				if (need > a[i][j]) continue;
				if (i > 0) dp[i][j] = min(dp[i][j], dp[i - 1][j] + a[i][j] - need);
				if (j > 0) dp[i][j] = min(dp[i][j], dp[i][j - 1] + a[i][j] - need);
			}
			ans = min(ans, dp[n - 1][m - 1]);
		}
		cout << ans << endl;
	}
	
	return 0;
}
```