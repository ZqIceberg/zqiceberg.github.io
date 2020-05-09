### Codeforces Round #636 (Div. 3) Editorial

#### 1343A - Candies
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
		for (int pw = 2; pw < 30; ++pw) {
			int val = (1 << pw) - 1;
			if (n % val == 0) {
				cerr << val << endl;
				cout << n / val << endl;
				break;
			}
		}
	}
	
	return 0;
}
```

#### 1343B - Balanced Array
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
		n /= 2;
		if (n & 1) {
			cout << "NO" << endl;
			continue;
		}
		cout << "YES" << endl;
		for (int i = 1; i <= n; ++i) {
			cout << i * 2 << " ";
		}
		for (int i = 1; i < n; ++i) {
			cout << i * 2 - 1 << " ";
		}
		cout << 3 * n - 1 << endl;  
		//2 4 6 8 1 3 5 11
		//前半段是i * 2。8是n*2(n=4)
		//后半段是1 3 5,每一个数都是少了1，总共少了n-1个1
		//然后还缺最后一个数没填，填上要补齐n-1还要补齐8这个位置，2*n
		//也就是2*n + (n - 1) = 3 * n - 1
	}
	
	return 0;
}
```

#### 1343C - Alternating Subsequence
```cpp
#include <bits/stdc++.h>

using namespace std;

int main() {
#ifdef _DEBUG
	freopen("input.txt", "r", stdin);
//	freopen("output.txt", "w", stdout);
#endif
	
	auto sgn = [&](int x) {
		if (x > 0) return 1;
		else return -1;
	};

	int t;
	cin >> t;
	while (t--) {
		int n;
		cin >> n;
		vector<int> a(n);
		for (auto &it : a) cin >> it;
		long long sum = 0;
		for (int i = 0; i < n; ++i) {
			int cur = a[i];
			int j = i;
			while (j < n && sgn(a[i]) == sgn(a[j])) {
				cur = max(cur, a[j]);
				++j;
			}
			sum += cur;
			i = j - 1;
		}
		cout << sum << endl;
	}
	
	return 0;
}
```

#### 1343D - Constant Palindrome Sum
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
		vector<int> cnt(2 * k + 1);
		for (int i = 0; i < n / 2; ++i) {
			++cnt[a[i] + a[n - i - 1]];
		}
		vector<int> pref(2 * k + 2);
		for (int i = 0; i < n / 2; ++i) {
			int l1 = 1 + a[i], r1 = k + a[i];
			int l2 = 1 + a[n - i - 1], r2 = k + a[n - i - 1];
			assert(max(l1, l2) <= min(r1, r2));
			++pref[min(l1, l2)];
			--pref[max(r1, r2) + 1];
		}
		for (int i = 1; i <= 2 * k + 1; ++i) {
			pref[i] += pref[i - 1];
		}
		int ans = 1e9;
		for (int sum = 2; sum <= 2 * k; ++sum) {
			ans = min(ans, (pref[sum] - cnt[sum]) + (n / 2 - pref[sum]) * 2);
		}
		cout << ans << endl;
	}
	
	return 0;
}
```
