#### T126438 约数 (divisor)
```cpp
#include <iostream>
  
using namespace std;

int main()
{
    int x;
    cin >> x;

    bool flag = false;
    for (int i = 2; i <= x / i; i ++)
        if (x % i == 0)
        {
            cout << x / i << endl;
            flag = true;

            break;
        }

    //x可能是质数，质数要输出1
    if (!flag) cout << 1 << endl;
   
    return 0;
}

```

#### T126439 阶乘(factorial)
`第一种思考`

1、2、3、4、5、6、7、8、9、10、11、...

分析上面的数列可知，每5个数中会出现一个可以产生结果中0的数字。把这些数字抽取出来是：
...、5、...、10、...、15、...、20、...、25、...

这些数字其实是都能满足5*k的数字，是5的倍数。统计一下他们的数量：n1=N/5。比如如果是101，则101之前应该是5,10,15,20,...,95,100 共101/5=20个数字满足要求。

把`...、5、...、10、...、15、...、20、...、25、...`
这些数字化成5*(1、2、3、4、5、...)的形式，内部的1、2、3、4、5、...又满足上面的分析：每5个数字有一个是5的倍数
抽取为...、25、...、50、...、75、...、100、...、125、...

而这些数字都是25的倍数（5的2次幂的倍数），自然也都满足5*k的要求。

这些数字是25、50、75、100、125、...= 5 * (5、10、15、20、25、...)=5 * 5 * (1、2、3、4、5、...)，内部的1、2、3、4、5、...又满足上面的分析，因此后续的操作重复上述步骤即可。

https://img-blog.csdn.net/20160416152912230

其实到这里已经不用再写，规律已经很清楚了。对于例子N=101，只要根据规律进行101/125=((101/5)/5)/5=4/5=0，退出统计。因此最终结果是20+4=24。计算结束



`第二种思考`

5！=120，其末尾所含有的“0”的个数为1；

10！= 3628800，其末尾所含有的“0”的个数为2；

20！= 2432902008176640000，其末尾所含有的“0”的个数为4。

两个大数字相乘，都可以拆分成多个质数相乘，而质数相乘结果尾数为0的，只可能是2*5。如果想到了这一点，那么就可以进一步想到：两个数相乘尾数0的个数其实就是依赖于2和5因子的个数。又因为每两个连续数字就会有一个因子2，个数非常充足，所以此时只需要关心5因子的个数就行了。

对于一个正整数n来说，怎么计算n！中5因子的个数呢？我们可以把5的倍数都挑出来

令n! = (5*K) * (5*(K-1)) * (5*(K-2)) * ... * 5 * A，其中A就是不含5因子的数相乘结果，n = 5*K + r（0<= r <= 4）。假设f(n!)是计算阶乘n!尾数0的个数，而g(n!)是计算n!中5因子的个数，那么就会有如下公式：

f(n!) = g(n!) = g(5^K * K! * A) = K + g(K!) = K + f(K!)，其中K=n / 5（取整数），其中5^k表示5的k次方

很显然，当0 <= n <= 4时，f(n!)=0

f(5!) = 1 + f(1!) = 1

f(10!) = 2 + f(2!) = 2

f(20!) = 4 + f(4!) = 4

f(100!) = 20 + f(20!) = 20 + 4 + f(4!) = 24

f(1000!) = 200 + f(200!) = 200 + 40 + f(40!) = 240 + 8 + f(8!) = 248 + 1 + f(1) =249

```cpp
#include <iostream>
#include <cstdio>

using namespace std;

int fact01(int n)
{
    //5和任意偶数相乘就会产生0，而偶数的个数远比5的个数多，所以每增加5 结尾就会增加一个零
    //每次加 5 到了 5 的次方时，会出现增加多个 0 的现象。也就是5*5*...和多个偶数相乘就会产生多个零
    //有5，5*5, 5*5*5的个数
    int sum = 0;
    while (n)
    {
        n /= 5;
        sum += n;
    }

    return sum;
}

int fact02(int n)
{   
    //当n>50的时候，就会出现5，可以和偶数凑成0
    //如果n<10，是不会凑成0的，最小凑出0的数是10
    
    
    //先把10的倍数先累计进去，有几个10，就肯定会产生很多个0的
    n /= 10;  
    int sum = n;
    
    //除以10之后，就是砍掉了最后一位，然后就变成了前n-1位组成的数当中，有5，5*5, 5*5*5的个数
    while (n)
    {   
        n /= 5;
        sum += n;
    }
   
    return sum;
}

int main()
{
    int n;
    cin >> n;

    //n的阶乘
    cout << fact01(n) << ' ';

    //n的双阶乘
    if (n % 2)
        cout << 0 << endl;
    else
        cout << fact02(n) << endl;

    return 0;
}
```

#### T126440 序列 (sequence)
```cpp
//0分程序
#include <iostream>
#include <algorithm>

using namespace std;

string s;

int main()
{
    int n;
    cin >> n;

    for (int i = 1; i <= n; i++)
    {   
        char c;
        cin >> c;
        
        s += c;
        reverse(s.begin(), s.end());
    }
    
    cout << s << endl;

    return 0;
}
```

```cpp
#include <iostream>
#include <cstdio>

using namespace std;

const int N = 1e8 + 10;

int a[N];

int main()
{
    int n;
    cin >> n;

    for (int i = 1; i <= n; i++)
        scanf("%d", &a[i]);

    for (int i = n; i > 0; i -= 2)
        printf("%d ", a[i]);

    //1234 4213
    //12345 53124
    for (int i = n % 2 + 1; i <= n; i += 2)
        printf("%d ", a[i]);
    puts("");

    return 0;
}
```

#### T126443 糖果(candy)
```cpp
#include <iostream>
#include <cstdio>

using namespace std;

const int N = 1e7 + 10;

int minn = 27; //一个区间内，最多26个英文字母不重复
string s;
int st[N];

int main()
{
    cin >> s;

    bool flag = false;
    int start = 0;
    for (int i = 0, len = s.size(); i < len; i++)
    {
        int u = s[i] - 'A';
        st[u]++;
   
        if (st[u] > 1)
        {
            //这个区间内找到重复的字母
            flag = true;

            for (int j = start; j < i; j++)
            {
                if (s[j] == s[i]) //发现重复的字母
                {
                    int t = i - j;
                    minn = min(minn, t);
   
                    start = j + 1; //下次再校验重复的时候，从j的下一位开始找
                    break;
                }
            }
            st[u] = 1; //恢复成1
        }
    }

    if (!flag)
        puts("-1");
    else
        cout << minn << endl;

    return 0;
}
```

#### T126444 迷宫 (maze)
```cpp
#include <iostream>

using namespace std;

const int N = 210;

char g[N][N];
bool st[N][N];
int n, m, ans;

void dfs(int x, int y)
{
    int dx[] = {0, 1, 0, -1}, dy[] = {1, 0, -1, 0};
    for (int i = 0; i < 4; i++)
    {
        int nx = x + dx[i], ny = y + dy[i];

        if (nx < 1 || nx > n || ny < 1 || ny > m) continue;
        if (st[nx][ny] || g[nx][ny] == '#') continue;

        st[nx][ny] = true;
        if (g[nx][ny] == '*') ans++;

        dfs(nx, ny);
    }
}

int main()
{
    int sx, sy;
    cin >> n >> m;

    for (int i = 1; i <= n; i++)
        for (int j = 1; j <= m; j++)
        {
            cin >> g[i][j];
            if (g[i][j] == 'S')
                sx = i, sy = j;
        }

    st[sx][sy] = true;
    dfs(sx, sy);

    cout << ans << endl;

    return 0;
}
```

#### T126445 盒子（box）
思路：序列sort之后，从中看看能分离出多少个符合条件的子序列

用开放链的思路，需要开新的链就新开，能插进前面的链尾部，就插上去

```cpp
#include <iostream>
#include <vector>
#include <cstdio>
#include <algorithm>
#include <fstream>

using namespace std;

const int N = 5e5 + 10;

vector<int> h[N];
int q[N];
int idx;

int main()
{
    int n;
    cin >> n;

    for (int i = 0; i < n; i++) scanf("%d", &q[i]);
    sort(q, q + n);

    h[idx++].push_back(q[0]);
    
    for (int i = 1; i < n; i++)
    {   
        bool flag = false;
        for (int j = 0; j < idx; j++)
        {   
            if (q[i] >= h[j].size())
            {   
                h[j].push_back(q[i]); //插入当前堆里面
                flag = true;
                
                break;
            }
        }

        if (!flag) //新开一个堆
            h[idx++].push_back(q[i]);
    }

    cout << idx << endl;

    return 0;
}
```
