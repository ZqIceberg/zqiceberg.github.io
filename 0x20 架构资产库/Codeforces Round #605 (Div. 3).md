### Codeforces Round #605 (Div. 3) Editorial

#### 1272A - Three Friends
```cpp
#include <bits/stdc++.h>

using namespace std;

int calc(int a, int b, int c) {
	return abs(a - b) + abs(a - c) + abs(b - c);
}

int main() {
#ifdef _DEBUG
	freopen("input.txt", "r", stdin);
//	freopen("output.txt", "w", stdout);
#endif
	
	int q;
	cin >> q;
	for (int i = 0; i < q; ++i) {
		int a, b, c;
		cin >> a >> b >> c;
		int ans = calc(a, b, c);
		for (int da = -1; da <= 1; ++da) {
			for (int db = -1; db <= 1; ++db) {
				for (int dc = -1; dc <= 1; ++dc) {
					int na = a + da;
					int nb = b + db;
					int nc = c + dc;
					ans = min(ans, calc(na, nb, nc));
				}
			}
		}
		cout << ans << endl;
	}
	
	return 0;
}
```

#### 1272B - Snow Walking Robot
```cpp
#include <bits/stdc++.h>

using namespace std;

const string MOVES = "LRUD";

int main() {
#ifdef _DEBUG
	freopen("input.txt", "r", stdin);
//	freopen("output.txt", "w", stdout);
#endif
	
	int q;
	cin >> q;
	for (int i = 0; i < q; ++i) {
		string s;
		cin >> s;
		map<char, int> cnt;
		for (auto c : MOVES) cnt[c] = 0;
		for (auto c : s) ++cnt[c];
		int v = min(cnt['U'], cnt['D']);
		int h = min(cnt['L'], cnt['R']);
		if (min(v, h) == 0) {
			if (v == 0) {
				h = min(h, 1);
				cout << 2 * h << endl << string(h, 'L') + string(h, 'R') << endl;
			} else {
				v = min(v, 1);
				cout << 2 * v << endl << string(v, 'U') + string(v, 'D') << endl;
			}
		} else {
			string res;
			res += string(h, 'L');
			res += string(v, 'U');
			res += string(h, 'R');
			res += string(v, 'D');
			cout << res.size() << endl << res << endl;
		}
	}
	
	return 0;
}
```

#### 1272C - Yet Another Broken Keyboard
```cpp
#include <bits/stdc++.h>

using namespace std;

int main() {
#ifdef _DEBUG
	freopen("input.txt", "r", stdin);
//	freopen("output.txt", "w", stdout);
#endif
	
	int n, k;
	cin >> n >> k;
	string s;
	cin >> s;
	set<char> st;
	for (int i = 0; i < k; ++i) {
		char c;
		cin >> c;
		st.insert(c);
	}
	
	long long ans = 0;
	for (int i = 0; i < n; ++i) {
		int j = i;
		while (j < n && st.count(s[j])) ++j;
		int len = j - i;
		ans += len * 1ll * (len + 1) / 2;
		i = j;
	}
	cout << ans << endl;
	
	return 0;
}
```

#### 1272D - Remove One Element
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
	vector<int> a(n);
	for (int i = 0; i < n; ++i) {
		cin >> a[i];
	}
	int ans = 1;
	
	vector<int> rg(n, 1);
	for (int i = n - 2; i >= 0; --i) {
		if (a[i + 1] > a[i]) rg[i] = rg[i + 1] + 1;
		ans = max(ans, rg[i]);
	}
	
	vector<int> lf(n, 1);
	for (int i = 1; i < n; ++i) {
		if (a[i - 1] < a[i]) lf[i] = lf[i - 1] + 1;
		ans = max(ans, lf[i]);
	}
	
	for (int i = 0; i < n - 2; ++i) {
		if (a[i] < a[i + 2]) ans = max(ans, lf[i] + rg[i + 2]);
	}
	
	cout << ans << endl;
	
	return 0;
}
```

#### 1272E - Nearest Opposite Parity


#### 1272F - Two Bracket Sequences



