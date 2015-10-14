```cpp
inline void build() {
	int i, j,
	    for (j = 1; (1 << j) <= N; j++) //区域大小(2^j)没必要超过整个区间
		    for (i = 1; i + (1 << j) - 1 <= N; i++)
			    f[i][j] = f[i][j - 1] >? f[i + (1 << (j - 1)) ][j - 1];
}
inline int get(int b, int e) {
	int j = (int)log2(e - b + 1); //区域长度的j,使得(2^j)大于等于请求区间的一半即可
	return f[b][j] >? f[e - (1 << j) + 1][j]; //f[e-(1<<j)+1][j] 表示以e为中点长为2^j的区间
}
```
