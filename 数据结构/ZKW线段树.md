```cpp
class ZKW
{
	int len;
	int *data;
	int base;
public:
	ZKW(int len)
	{
		for (base = 1; base < len; base <<= 1);
		data = new int[base * 2];
	}
	~ZKW()
	{
		delete [] data;
	}
	int update(int x, int v)
	{
		x = x + base;
		data[x] = v;
		x >>= 1;
		while (x)
		{
			data[x] = max(data[x << 1], data[(x << 1) + 1]);
			x >>= 1;
		}
		return data[1];
	}
	int max()
	{
		return data[1];
	}
};
```
