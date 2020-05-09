### Codeforces Round #629 (Div. 3) Editorial

#### 1328A - Divisibility Problem
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
		int a, b;
		cin >> a >> b;
		if (a % b == 0) cout << 0 << endl;
		else cout << b - a % b << endl;
	}
	
	return 0;
}
```

#### 1328B - K-th Beautiful String
```cpp
#include <bits/stdc++.h>

using namespace std;

#define forn(i, n) for (int i = 0; i < int(n); i++)

int main() {
    int t;
    cin >> t;
    forn(tt, t) {
        int n, k;
        cin >> n >> k;
        string s(n, 'a');
        for (int i = n - 2; i >= 0; i--) {
            if (k <= (n - i - 1)) {
                s[i] = 'b';
                s[n - k] = 'b';
                cout << s << endl;
                break;
            }
            k -= (n - i - 1);
        }
    }
}
```

#### 1328C - Ternary XOR
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
		string x;
		cin >> n >> x;
		string a(n, '0'), b(n, '0');
		for (int i = 0; i < n; ++i) {
			if (x[i] == '1') {
				a[i] = '1';
				b[i] = '0';
				for (int j = i + 1; j < n; ++j) {
					b[j] = x[j];
				}
				break;
			} else {
				a[i] = b[i] = '0' + (x[i] - '0') / 2;
			}
		}
		cout << a << endl << b << endl;
	}
	
	return 0;
}
```

#### 1328D - Carousel
```cpp
#include <bits/stdc++.h>

using namespace std;

int solve() {
	int n;
	cin >> n;
	vector<int> a(n);
	for (int i = 0; i < n; ++i) {
		cin >> a[i];
	}
	
	if (count(a.begin(), a.end(), a[0]) == n) {
		cout << 1 << endl;
		for (int i = 0; i < n; ++i) {
			cout << 1 << " ";
		}
		cout << endl;
		return 0;
	}
	
	if (n % 2 == 0) {
		cout << 2 << endl;
		for (int i = 0; i < n; ++i) {
			cout << i % 2 + 1 << " ";
		}
		cout << endl;
		return 0;
	}
	
	for (int i = 0; i < n; ++i) {
		if (a[i] == a[(i + 1) % n]) {
			vector<int> ans(n);
			for (int j = 0, pos = i + 1; pos < n; ++pos, j ^= 1) {
				ans[pos] = j + 1;
			}
			for (int j = 0, pos = i; pos >= 0; --pos, j ^= 1) {
				ans[pos] = j + 1;
			}
			cout << 2 << endl;
			for (int pos = 0; pos < n; ++pos) {
				cout << ans[pos] << " ";
			}
			cout << endl;
			return 0;
		}
	}
	
	cout << 3 << endl;
	for (int i = 0; i < n - 1; ++i) {
		cout << i % 2 + 1 << " ";
	}
	cout << 3 << endl;
    return 0;    
}

int main() {
#ifdef _DEBUG
	freopen("input.txt", "r", stdin);
//	freopen("output.txt", "w", stdout);
#endif

int q;
cin >> q;
for (int qq = 0; qq < q; qq++) {
    solve();
}

	return 0;
}
```

#### 1328E - Tree Queries

#### 1328F - Make k Equal

