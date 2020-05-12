### 1352A - Sum of Round Numbers

```cpp
#include <bits/stdc++.h>

using namespace std;

int main() {
	int t;
	cin >> t;
	while (t--) {
		int n;
		cin >> n;
		vector<int> ans;
		int power = 1;
		while (n > 0) {
			if (n % 10 > 0) {
				ans.push_back((n % 10) * power);
			}
			n /= 10;
			power *= 10;
		}
		cout << ans.size() << endl;
		for (auto number : ans) cout << number << " ";
		cout << endl;
	}
}
```

### 1352B - Same Parity Summands

```cpp
#include <bits/stdc++.h>

using namespace std;

int main() {
	int t;
	cin >> t;
	while (t--) {
		int n, k;
		cin >> n >> k;
		int n1 = n - (k - 1);
		if (n1 > 0 && n1 % 2 == 1) {
			cout << "YES" << endl;
			for (int i = 0; i < k - 1; ++i) cout << "1 ";
			cout << n1 << endl;
			continue;
		}
		int n2 = n - 2 * (k - 1);
		if (n2 > 0 && n2 % 2 == 0) {
			cout << "YES" << endl;
			for (int i = 0; i < k - 1; ++i) cout << "2 ";
			cout << n2 << endl;
			continue;
		}
		cout << "NO" << endl;
	}
}
```

### 1352C - K-th Not Divisible by n

```cpp
#include <bits/stdc++.h>

using namespace std;

int main() {
	int t;
	cin >> t;
	while (t--) {
		int n, k;
		cin >> n >> k;
		int need = (k - 1) / (n - 1);
		cout << k + need << endl;
	}
}
```

### 1352D - Alice, Bob and Candies

```cpp
#include <bits/stdc++.h>

using namespace std;

int main() {
	int t;
	cin >> t;
	while (t--) {
		int n;
		cin >> n;
		vector<int> a(n);
		for (auto &it : a) cin >> it;
		int l = 0, r = n - 1;
		int suml = 0, sumr = 0;
		int cnt = 0, ansl = 0, ansr = 0;
		while (l <= r) {
			if (cnt % 2 == 0) {
				int nsuml = 0;
				while (l <= r && nsuml <= sumr) {
					nsuml += a[l++];
				}
				ansl += nsuml;
				suml = nsuml;
			} else {
				int nsumr = 0;
				while (l <= r && nsumr <= suml) {
					nsumr += a[r--];
				}
				ansr += nsumr;
				sumr = nsumr;
			}
			++cnt;
		}
		cout << cnt << " " << ansl << " " << ansr << endl;
	}
}
```

### 1352E - Special Elements

```cpp
#include <bits/stdc++.h>

using namespace std;

int main() {
	int t;
	cin >> t;
	while (t--) {
		int n;
		cin >> n;
		vector<int> a(n);
		vector<int> cnt(n + 1);
		int ans = 0;
		for (auto &it : a) {
			cin >> it;
			++cnt[it];
		}
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
		cout << ans << endl;
	}
}
```

### 1352F - Binary String Reconstruction

```cpp
#include <bits/stdc++.h>

using namespace std;


int main() {
	int t;
	cin >> t;
	while (t--) {
		int n0, n1, n2;
		cin >> n0 >> n1 >> n2;
		if (n1 == 0) {
			if (n0 != 0) {
				cout << string(n0 + 1, '0') << endl;
			} else {
				cout << string(n2 + 1, '1') << endl;
			}
			continue;
		}
		string ans;
		for (int i = 0; i < n1 + 1; ++i) {
			if (i & 1) ans += "0";
			else ans += "1";
		}
		ans.insert(1, string(n0, '0'));
		ans.insert(0, string(n2, '1'));
		cout << ans << endl;
	}
}
```

### 1352G - Special Permutation

```cpp
#include <bits/stdc++.h>

using namespace std;

int main() {
	int t;
	cin >> t;
	while (t--) {
		int n;
		cin >> n;
		if (n < 4) {
			cout << -1 << endl;
			continue;
		}
		for (int i = n; i >= 1; --i) {
			if (i & 1) cout << i << " ";
		}
		cout << 4 << " " << 2 << " ";
		for (int i = 6; i <= n; i += 2) {
			cout << i << " ";
		}
		cout << endl;
	}
}
```