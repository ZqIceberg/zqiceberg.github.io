### mp
3440-统计数字
3450-统计货物

### vector
3460-战士

3480-priority_queue(堆)
3243-离散化   lower_bound()

```cpp
map<string, int> mp;

mp["abc"] = 10;

for (map<string, int>::iterator it = mp.begin(); it != mp.end(); it++)
	cout << it->first << " " << it->second << endl;

if (mp.find(s) == mp.end())  //字符串之前没有出现过

(int)mp.size(); //访问容器的大小加上(int)
返回unsigned int类型
```

.c_str()  printf()输出string
char[]   string

```cpp
char s[10];
scanf("%s", s);
if (strcmp(s, "insert") == 0)
{

}
```

```cpp
set<int> s;
st.insert(10);
st.insert(15);
st.insert(20);

//找到第一个小于15的数
//这个用法也很nice
set<int>::iterator it = st.lower_bound(15);
if (it != st.begin())
{
	it--;
	cout << *it << endl;
}
```




