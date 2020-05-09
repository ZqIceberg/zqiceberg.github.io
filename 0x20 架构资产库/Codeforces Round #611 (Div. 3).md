### Codeforces Round #611 (Div. 3) Editorial

#### 1283A - Minutes Before the New Year
```cpp
#include <bits/stdc++.h>

using namespace std;

int main() {
#ifdef _DEBUG
	freopen("input.txt", "r", stdin);
//	freopen("output.txt", "w", stdout);
#endif
	
	int q;
	scanf("%d", &q);
	for (int i = 0; i < q; ++i) {
		int h, m;
		scanf("%d %d", &h, &m);
		printf("%d\n", 1440 - h * 60 - m);
	}
	
	return 0;
}
```

#### 1283B - Candies Division
```cpp
#include <bits/stdc++.h>

using namespace std;

int main() {
#ifdef _DEBUG
	freopen("input.txt", "r", stdin);
//	freopen("output.txt", "w", stdout);
#endif
	
	int q;
	cin >> q;
	for (int i = 0; i < q; ++i) {
		int n, k;
		cin >> n >> k;
		int full = n - n % k;
		full += min(n % k, k / 2);
		cout << full << endl;
	}
	
	return 0;
}
```

#### 1283C - Friends and Gifts
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
	vector<int> f(n);
	vector<int> in(n), out(n);
	for (int i = 0; i < n; ++i) {
		cin >> f[i];
		--f[i];
		if (f[i] != -1) {
			++out[i];
			++in[f[i]];
		}
	}
	
	vector<int> loops;
	for (int i = 0; i < n; ++i) {
		if (in[i] == 0 && out[i] == 0) {
			loops.push_back(i);
		}
	}
	if (loops.size() == 1) {
		int idx = loops[0];
		for (int i = 0; i < n; ++i) {
			if (in[i] == 0 && i != idx) {
				f[idx] = i;
				++out[idx];
				++in[i];
				break;
			}
		}
	} else if (loops.size() > 1) {
		for (int i = 0; i < int(loops.size()); ++i) {
			int cur = loops[i];
			int nxt = loops[(i + 1) % int(loops.size())];
			f[cur] = nxt;
			++out[cur];
			++in[nxt];
		}
	}
	loops.clear();
	vector<int> ins, outs;
	for (int i = 0; i < n; ++i) {
		if (in[i] == 0) ins.push_back(i);
		if (out[i] == 0) outs.push_back(i);
	}
	assert(ins.size() == outs.size());
	for (int i = 0; i < int(outs.size()); ++i) {
		f[outs[i]] = ins[i];
	}
	
	for (int i = 0; i < n; ++i) {
		cout << f[i] + 1 << " ";
	}
	cout << endl;
	
	return 0;
}
```



#### 1283D - Christmas Trees
```cpp
#include <bits/stdc++.h>

using namespace std;

mt19937 rnd(time(NULL));

int main() {
#ifdef _DEBUG
	freopen("input.txt", "r", stdin);
//	freopen("output.txt", "w", stdout);
#endif
	
	int n, m;
	cin >> n >> m;
	vector<int> x(n);
	for (int i = 0; i < n; ++i) {
		cin >> x[i];
	}
	queue<int> q;
	map<int, int> d;
	for (int i = 0; i < n; ++i) {
		d[x[i]] = 0;
		q.push(x[i]);
	}
	long long ans = 0;
	vector<int> res;
	while (!q.empty()) {
		if (int(res.size()) == m) break;
		int cur = q.front();
		q.pop();
		if (d[cur] != 0) {
			ans += d[cur];
			res.push_back(cur);
		}
		if (!d.count(cur - 1)) {
			d[cur - 1] = d[cur] + 1;
			q.push(cur - 1);
		}
		if (!d.count(cur + 1)) {
			d[cur + 1] = d[cur] + 1;
			q.push(cur + 1);
		}
	}
	cout << ans << endl;
	shuffle(res.begin(), res.end(), rnd);
	for (auto it : res) cout << it << " ";
	cout << endl;
	
	return 0;
}
```

