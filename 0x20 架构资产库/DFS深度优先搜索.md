#### 递归实现指数型枚举

```cpp
//使用vector，dfs()只有一个参数
#include <iostream>
#include <cstdio>
#include <vector>
#include <fstream>

using namespace std;

int n;
vector<int> chosen;

void dfs(int x)
{
    if (x == n + 1)
    {
        for (int i = 0; i < chosen.size(); i++)
            printf("%d ", chosen[i]);
        puts("");

        return ;
    }

    //不选x
    dfs(x + 1);

    //选x
    chosen.push_back(x);
    dfs(x + 1);
    chosen.pop_back();
}

int main()
{
    //freopen("meiju02.in", "r", stdin);
    //freopen("meiju02.out", "w", stdout);

    cin >> n;

    dfs(1);

    return 0;
}

```

```cpp
//dfs()两个参数，从0开始搜，用二进制记录状态
#include <iostream>
#include <cstdio>

using namespace std;

int n;

void dfs(int u, int state)
{
    if (u == n)
    {
        for (int i = 0; i < n; i++)
            if (state >> i & 1)
                cout << i + 1 << ' ';
        puts("");

        return ;
    }

    dfs(u + 1, state);
    dfs(u + 1, state | 1 << u);
}

int main()
{
    cin >> n;
    dfs(0, 0);

    return 0;
}
```


#### 递归实现组合型枚举
```cpp
#include <iostream>
#include <cstdio>
#include <vector>
#include <fstream>

using namespace std;

int n, m;
vector<int> chosen;

void dfs(int x)
{
    if (chosen.size() > m || chosen.size() + (n - x + 1) < m) return ;
    
    if (x == n + 1)
    {
        for (int i = 0; i < chosen.size(); i++)
            printf("%d ", chosen[i]);
        puts("");

        return ;
    }
    //选x
    chosen.push_back(x);
    dfs(x + 1);
    chosen.pop_back();
    
    
    //不选x
    dfs(x + 1);
    

}

int main()
{
    //freopen("meiju03.in", "r", stdin);
    //freopen("meiju03.out", "w", stdout);

    cin >> n >> m;
    
    dfs(1);
    
    return 0;
}
```

```cpp
#include <iostream>
#include <cstdio>

using namespace std;

int n, m;

void dfs(int u, int sum, int state)
{
    if (sum > m || sum + n - u < m) return ;
    
    if (sum == m)
    {
        for (int i = 0; i < n; i++)
            if (state >> i & 1)
                cout << i + 1 << ' ';
        puts("");
        
        return ;
    }
    
    dfs(u + 1, sum + 1, state | 1 << u);
    dfs(u + 1, sum, state);
}

int main()
{
    cin >> n >> m;
    dfs(0, 0, 0);
    
    return 0;
}
```

#### P1036 选数
```cpp
#include <iostream>
#include <cstdio>
#include <vector>

using namespace std;

const int N = 30;

vector<int> chosen;
int q[N];
int n, k;
int res;

bool is_prime(int x)
{
    //试除法判断质数模板
    if (x < 2) return false;
    for (int i = 2; i <= x / i; i++)
        if (x % i == 0) return false;

    return true;
}

void dfs(int x)
{
    if (chosen.size() > k || chosen.size() + (n - x + 1) < k) return;   //剪枝，如果已经找到的个数，大于k个，或者剩下的都找来也不够k个就返回

    if (x == n + 1)    //问题边界
    {
        int sum = 0;
        for (int i = 0; i < chosen.size(); i++) sum += chosen[i];

        if (is_prime(sum))
        {
            res++;
            //for (int i = 0; i < idx; i++) printf("%d ", chosen[i]);
            //puts("");
        }

        return ;
    }

    //求解子问题，不选这个数
    dfs(x + 1);

    //求解子问题，选这个数
    chosen.push_back(q[x]);
    dfs(x + 1);
    chosen.pop_back();
}

int main()
{
    cin >> n >> k;
    for (int i = 1; i <= n; i++) cin >> q[i];

    dfs(1);  //从第一个数开始找

    cout << res << endl;

    return 0;
}
```

```cpp

```

```cpp
#include <iostream>
#include <cstdio>

using namespace std;

const int N = 25;

int q[N];
int n, m;

int is_prime(int x)
{
    if (x < 2) return 0;

    for (int i = 2; i <= x / i; i++)
        if (x % i == 0) return 0;
   
    return 1;
}

int dfs(int left_choose, int already_choose, int u)
{
    if (left_choose == 0) return is_prime(already_choose);

    int sum = 0;
    for (int i = u; i <= n; i++)
            sum += dfs(left_choose - 1, already_choose + q[i], i + 1);

    return sum;
}

int main()
{
    cin >> n >> m;
    for (int i = 1; i <= n; i++) scanf("%d", &q[i]);
    
    printf("%d\n", dfs(m, 0, 1));
    
    return 0;
}    

```

#### P1157 组合的输出
```cpp
#include <iostream>
#include <cstdio>
#include <vector>

using namespace std;

vector<int> A;
vector<vector<int> > ANS;
int n, m;

void dfs(int x)  //当前选择的数是x
{
    if (A.size() > m || A.size() + (n - x + 1) < m) return ;  //lyd大神的指引

    if (x == n + 1)
    {
        ANS.push_back(A);
        return ;
    }

    //不选x
    dfs(x + 1);

    //选x
    A.push_back(x);
    dfs(x + 1);
    A.pop_back();
}

int main()
{
    cin >> n >> m;

    dfs(1); //从1开始

    vector<vector<int> >:: iterator iter;
    for (iter = ANS.end() - 1; iter >= ANS.begin(); iter--)
    {
        vector<int> T = *iter;
        for (int i = 0; i < T.size(); i++) printf("%3d", T[i]);
        puts("");
    }
   
    return 0;
}
```

```cpp
#include <iostream>
#include <cstdio>
#include <vector>

using namespace std;

vector<int> A;
//vector<vector<int> > ANS;
int n, m;

void dfs(int x)  //当前选择的数是x
{
    if (A.size() > m || A.size() + (n - x + 1) < m) return ;  //lyd大神的指引

    if (x == n + 1)
    {
        for(int i = 0; i < A.size(); i++)
            printf("%3d", A[i]);
        puts("");

        return ;
    }

    //选x
    A.push_back(x);
    dfs(x + 1);
    A.pop_back();

    //不选x
    dfs(x + 1);
}
```

```cpp
#include <iostream>
#include <cstdio>

using namespace std;

int a[25];
bool st[25];
int n, m;

void dfs(int k)
{
    if (k > m)
    {
        for (int i = 1; i <= m; i++) printf("%3d", a[i]);
        puts("");

        return ;
    }

    int t = a[k - 1];
    for (int i = t; i <= n; i++)
    {
        if (st[i]) continue;

        a[k] = i;
        st[i] = true;
        dfs(k + 1);
        st[i] = false;
    }
}

int main()
{
    cin >> n >> m;

    a[0] = 1;
    dfs(1);

    return 0;
}

```

```cpp
#include <iostream>
#include <cstdio>

using namespace std;

int n, m;
int a[25];

void dfs(int k)  //当前找了k个数
{
    if (k > m)  //超过m个，找够了，输出，return上一层
    {
        for (int i = 1; i <= m; i++)
            printf("%3d", a[i]);
        puts("");

        return ;
    }

    int t = a[k - 1];
    for (int i = t + 1; i <= n; i++)  //当前的数，要大于前一个找到的数
    {
        a[k] = i;
        dfs(k + 1);
    }
}

int main()
{
    cin >> n >> m;

    dfs(1);  //当前找了1个数

    return 0;
}
```



#### 递归实现排列型枚举
```cpp
#include <iostream>
#include <cstdio>
#include <fstream>

using namespace std;

int order[20];
bool chosen[20];
int n;

void dfs(int k)
{
    if (k == n + 1)
    {
        for (int i = 1; i <= n; i++)
            cout << order[i] << ' ';
        puts("");
    }
    
    for (int i = 1; i <= n; i++)
    {
        if (chosen[i]) continue;
        
        order[k] = i;
        chosen[i] = true;
        
        dfs(k + 1);
        
        chosen[i] = false;
    }
}

int main()
{
    //freopen("meiju03.in", "r", stdin);
    //freopen("meiju03.out", "w", stdout);

    cin >> n;
    
    dfs(1);
    
    return 0;
}

```

```cpp
#include <iostream>
#include <cstdio>

using namespace std;

int order[20];
bool chosen[20];
int n;

void dfs(int k)
{
    if (k == n)
    {
        for (int i = 0; i < n; i++)
            cout << order[i] << ' ';
        puts("");
    }
    
    for (int i = 1; i <= n; i++)
    {
        if (chosen[i]) continue;
        
        order[k] = i;
        chosen[i] = true;
        
        dfs(k + 1);
        
        chosen[i] = false;
    }
}

int main()
{
    cin >> n;
    
    dfs(0);
    
    return 0;
}

```

```cpp
#include <iostream>
#include <cstdio>
#include <vector>

using namespace std;

int n;
vector<int> path;

void dfs(int u, int state)
{
    if (u == n)
    {
        vector<int>::iterator i;
        for (i = path.begin(); i < path.end(); i++)
            cout << *i << ' ';
        puts("");
    }
    
    for (int i = 0; i < n; i++)
        if (!(state >> i & 1))
        {
            path.push_back(i + 1);
            dfs(u + 1, state | 1 << i);
            path.pop_back();
        }
}

int main()
{
    cin >> n;
    dfs(0, 0);
    
    return 0;
}

```

#### P1706 全排列问题
```cpp
#include <iostream>
#include <cstdio>

using namespace std;

const int N = 10;

int q[N], st[N];
int n;

void dfs(int x)
{
    if (x == n + 1)
    {
        for (int i = 1; i <= n; i++) printf("%5d", q[i]);
        puts("");

        return ;
    }

    for (int i = 1; i <= n; i++)
    {
        if (!st[i])
        {
            q[x] = i;
            st[i] = 1;

            dfs(x + 1);

            st[i] = 0;
        }
    }
}

int main()
{
    cin >> n;

    dfs(1);

    return 0;
}

```



#### T114580 奥数题
`算法：枚举`
```cpp
#include<iostream>
using namespace std;
int main()
{
    int a,b,c,d,e,f,g,h,i,total=0;
    //第一个数 
    for(a=1;a<=9;a++)
    for(b=1;b<=9;b++)
    for(c=1;c<=9;c++)
    //第二个数
    for(d=1;d<=9;d++)
    for(e=1;e<=9;e++)
    for(f=1;f<=9;f++)
    //第三个数
    for(g=1;g<=9;g++)
    for(h=1;h<=9;h++)
    for(i=1;i<=9;i++)
    {
        if(a!=b && a!=c && a!=d && a!=e && a!=f && a!=g && a!=h && a!=i
                &&b!=c && b!=d && b!=e && b!=f && b!=g && b!=h && b!=i
                        && c!=d && c!=e && c!=f && c!=g && c!=h && c!=i
                                &&d!=e && d!=f && d!=g && d!=h && d!=i
                                        && e!=f && e!=g && e!=h && e!=i
                                                && f!=g && f!=h && f!=i
                                                        && g!=h && g!=i
                                                                && h!=i
                                && a*100+b*10+c+d*100+e*10+f==g*100+h*10+i )
        {
            total++;
            printf("%d%d%d+%d%d%d=%d%d%d\n",a,b,c,d,e,f,g,h,i);
        }
    }
    printf("%d",total/2);

    return 0;
}

```

提示：173+286=469 ，286 + 173=469 是同一组合
```cpp
#include <cstdio>
#include <iostream>
#include <fstream>

using namespace std;

int a[100],book[100],n=9;
int c[1000];
int ans;

void dfs(int step){
    int i;
    if(step==n+1)
    {
        if((a[1]*100+a[2]*10+a[3]+a[4]*100+a[5]*10+a[6])==(a[7]*100+a[8]*10+a[9]))
        {
            c[a[1]*100+a[2]*10+a[3]]=1;
            if(c[a[4]*100+a[5]*10+a[6]]==0)
            {
                printf("%d + %d = %d\n",a[1]*100+a[2]*10+a[3],a[4]*100+a[5]*10+a[6],a[7]*100+a[8]*10+a[9]);
                ans++;
            }
        }
        return ;
    }
   
    for(int i=1;i<=n;++i){
        if(book[i]==0){
            a[step]=i;
            book[i]=1;
   
            dfs(step+1);
            book[i]=0;
        }
    }
    return ;
}

int main()
{
    dfs(1);

    cout << endl << ans << endl;
    return 0;
}

```
#### T114582 素数环
```cpp
#include <iostream>
#include <cstdio>

using namespace std;

const int N = 8;

int path[10];
bool st[10];
int sum;

bool check(int a, int b)
{   
    int x = a + b;
    
    if (x < 2) return false; 
    for (int i = 2; i <= x / i; i++)
        if (x % i == 0) return false;
    
    return true;
}

void dfs(int u)
{
    if (u == N + 1)
    {
        if (check(path[N], path[1]))
        {
            for (int i = 1; i <= N; i++) printf("%d ", path[i]);
            puts("");
            
            sum++;
        }   
    }

    for (int i = 1; i <= N; i++)
        if (u == 1 || (!st[i] && check(i, path[u - 1]))) //第一个数，或者和前一个找到的数之和是素数
        {
            st[i] = true;
            path[u] = i;

            dfs(u + 1);

            st[i] = false;
        }
}

int main()
{
    dfs(1);
    cout << sum << endl;

    return 0;
}
```

`next_permutation` 香香香
```cpp
#include <iostream>
#include <cstdio>
#include <cmath>
#include <algorithm>

using namespace std;

const int N = 8;

int a[] = {1, 2, 3, 4, 5, 6, 7, 8};
int sum;

bool check(int a, int b)
{   
    int x = a + b;
    if (x < 2) return false; 
    for (int i = 2; i <= x / i; i++)
        if (x % i == 0) return false;
    
    return true;
}

int main()
{
    int T = 1;
    for (int i = 1; i <= N; i++) T *= i;

    while (T--)
    {   
        next_permutation(a, a + N);

        bool flag = true;;
        for (int i = 0; i < N; i++)
            if (!check(a[(i - 1 + N) % N], a[i]))
            {   
                flag = false;
                break;
            }

        if (flag)
        {
            for (int i = 0; i < N; i++) printf("%d ", a[i]);
            puts("");

            sum++;
        }      
    }
   
    printf("%d\n", sum);

    return 0;
}

```

#### UVA524 素数环 Prime Ring Problem
原题数据卡格式
```
#include <iostream>
#include <cstdio>
#include <cstring>
#include <fstream>

using namespace std;

const int N = 20;

int n;
int idx;
int path[N];
bool st[N];

bool check(int a, int b)
{   
    int x = a + b;
    if (x < 2) return false; 
    for (int i = 2; i <= x / i; i++)
        if (x % i == 0) return false;
    
    return true;
}

void dfs(int u)
{   
    if (u == n + 1 && check(path[n], 1))
    {   
        for (int i = 1; i < n; i++) printf("%d ", path[i]);
        printf("%d\n", path[n]);
    }
    
    for (int i = 2; i <= n; i++)
        if (!st[i] && check(path[u - 1], i))
        {   
            st[i] = true;
            path[u] = i;

            dfs(u + 1);

            st[i] = false;
        }
}

int main()
{
    while (cin >> n)
    {
        if (idx) puts("");
        printf("Case %d:\n", ++idx);

        memset(path, 0, sizeof path);
        path[1] = 1;  //第一个数就是1不动，这样环就不会转了

        memset(st, 0, sizeof st);

        dfs(2); //从第2个数开始搜索
    }

    return 0;
}
```


#### 自然数的拆分

```cpp
/一本通书上例题，给出的std，a[10001] = {1}这么写好坑啊
//刚学的，以为是数组全部初始化成1，其实只是需要a[0]=1
//我太弱了..

#include <iostream>
#include <cstdio>
#include <cstring>

using namespace std;

const int N = 20;

int a[N];
int n;

void dfs(int left, int u)
{
    if (left == 0)
    {
        cout << n << '=';

        for (int i = 1; i < u - 1; i++) cout << a[i] << '+';
        cout << a[u - 1] << endl;

        return ;
    }

    int t = a[u - 1]; //不能小于前一个拆分的数

    for (int i = t; i <= left; i++)
    {
        if (i < n)
        {
            a[u] = i;
            left -= i;

            dfs(left, u + 1);

            left += i;
        }
    }
}


int main()
{
    cin >> n;

    a[0] = 1;
    dfs(n, 1); //剩余要拆分的数为n，从下标1开始

    return 0;
}

```

这题面，并没有给n的取值范围，用vector<>
比如，上面的写法，当输入n为20的时候，就会有问题了

```cpp
//一本通这题竟然没给n的取值范围..我日
//无力吐槽，例题用数组做的，数组拍多大合适啊
//ybt上，评测环境还没有vector头文件，用万能头才编译过的

//当测试n=1000的时候，笔记本cpu就会开始炸
//输出到文件里，文件要几百MB，而且几十秒也没结束

//ybt上的测试点，最多需要8ms，测试点的n不会大，可能是一个两位数

//那你例题拍1万的数组，是咋回事啊。。真敢拍啊

#include <iostream>
#include <vector>
#include <cstdio>
//#include <bits/stdc++.h>
#include <fstream>

using namespace std;

vector<int> A;
int n;

void dfs(int left)
{   
    if (left == 0)   //left = 0在上一层循环的时候，是被push_back进去的
    {   
        cout << n << '=';
        
        vector<int>::iterator iter;
        for (iter = A.begin() + 1; iter < A.end() - 1; iter++)
            cout << *iter << '+';
        
        cout << *iter << endl;
        
        return ;
    }
    
    vector<int>::iterator iter = A.end() - 1;
    for (int i = *iter; i <= left; i++)
        if (i < n)
        {
            A.push_back(i);
            left -= i;

            dfs(left);

            A.pop_back();
            left += i;
        }
}

int main()
{
    //freopen("chai03.in", "r", stdin);
    //freopen("chai03.out", "w", stdout);

    cin >> n;

    A.push_back(1);  //把1先push进去，占位，防止SE，也是要拆分出来的数要大于等于1
    dfs(n);  //当前要拆分的数为n；

    return 0;
}

```

#### P2089 烤鸡
`枚举`
```cpp
#include<iostream>
using namespace std;
int zuoliao[10000][11];
int ans;

int main()
{
	int n;
	cin>>n;
	if(n>33)
	{
		cout<<"0";
		return 0;
	}
	
	for(int i1=1;i1<=3;i1++)
		for(int i2=1;i2<=3;i2++)
			for(int i3=1;i3<=3;i3++)
	for(int i4=1;i4<=3;i4++)
		for(int i5=1;i5<=3;i5++)
			for(int i6=1;i6<=3;i6++)
	for(int i7=1;i7<=3;i7++)
		for(int i8=1;i8<=3;i8++)
			for(int i9=1;i9<=3;i9++)
				for(int i10=1;i10<=3;i10++)
				{
					if(n==i1+i2+i3+i4+i5+i6+i7+i8+i9+i10)
					{
						ans++;
						zuoliao[ans][1]=i1;
						zuoliao[ans][2]=i2;
						zuoliao[ans][3]=i3;
						zuoliao[ans][4]=i4;
						zuoliao[ans][5]=i5;
						zuoliao[ans][6]=i6;
						zuoliao[ans][7]=i7;
						zuoliao[ans][8]=i8;
						zuoliao[ans][9]=i9;
						zuoliao[ans][10]=i10;
					}
				}
	
	if(0==ans)
		cout<<ans;
	else
	{
		cout<<ans<<endl;
		for(int i=1;i<=ans;i++)
		{
			for(int j=1;j<=10;j++)
				cout<<zuoliao[i][j]<<" ";
			cout<<endl;
		}
	}
	
	return 0;
}

```

`DFS`
```cpp
#include <iostream>
#include <cstdio>
#include <cstring>

using namespace std;

int a[15]; //用来临时存方案
int g[10000][15];  //用来存方案
int ans;   //方案数，判断是否输出0的问题
int n;

void dfs(int u, int s)
{
    if (u > 10)
    {
        if (s == n)  //输出合法方案
        {
            ans++;
            for (int i = 1; i <= 10; i++) g[ans][i] = a[i];
        }

        return ;
    }

    for (int i = 1; i <= 3; i++)
    {
        if (s + i > n) break;

        a[u] = i;
        dfs(u + 1, s + i);  //搜索下一个调料，当前美味程度+i
        //a[u] = 0;    //可写可不写
    }
}

int main()
{
    cin >> n;

    if (n > 30) {cout << 0 << endl; return 0;}

    dfs(1, 0);  //从第1个调料开始，当前美味程度为0

    if (ans == 0) cout << 0 << endl;
    else
    {
        cout << ans << endl;
        for (int i = 1; i <= ans; i++)
        {
            for (int j = 1; j <= 10; j++)
                cout << g[i][j] << ' ';
            puts("");
        }
    }

    return 0;
}


```


