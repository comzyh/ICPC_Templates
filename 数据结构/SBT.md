```cpp
/*
Program:Size Balanced Tree
*/
#include <cstdio>
#include <cstring>
struct node
{
	int s;
	int  left, right;
	int key;
	void pri() {printf("s=%4d,left=%4d,right=%4d,key=%4d\n", s, left, right, key);}
	void init(int k)
	{
		key = k;
		s = 1;
		left = right = 0;
	}
} SBT[100000];
int tail = 0;
inline int free_node()
{return ++tail;}
node &left_rotate(int &t)
{
	int k = SBT[t].right;
	SBT[t].right = SBT[k].left;
	SBT[k].left = t;
	SBT[k].s = SBT[t].s;
	SBT[t].s = SBT[SBT[t].left].s + SBT[SBT[t].right].s + 1;
	t = k;
	return SBT[k];
}
node &right_rotate(int &t)
{
	int k = SBT[t].left;
	SBT[t].left = SBT[k].right;
	SBT[k].right = t;
	SBT[k].s = SBT[t].s;
	SBT[t].s = SBT[t].s = SBT[SBT[t].left].s + SBT[SBT[t].right].s + 1;
	t = k;
	return SBT[k];
}
int maintain(int &t, bool flag)
{
	if (flag == 0) //×ó±ß´óÁË
	{
		if (SBT[SBT[SBT[t].left].left].s > SBT[SBT[t].right].s)
			right_rotate(t);
		else if ((SBT[SBT[SBT[t].left].right].s > SBT[SBT[t].right].s))
		{
			left_rotate(SBT[t].left);
			right_rotate(t);
		}
		else return t;
	}
	else
	{
		if (SBT[SBT[SBT[t].right].right].s > SBT[SBT[t].left].s)
			left_rotate(t);
		else if ((SBT[SBT[SBT[t].right].left].s > SBT[SBT[t].left].s))
		{
			right_rotate(SBT[t].right);
			left_rotate(t);
		}
		else return t;
	}
	maintain(SBT[t].left, 0);
	maintain(SBT[t].right, 1);
	maintain(t, 0);
	maintain(t, 1);
	return t;
}
int insert(int &t, int v)
{

	if (t == 0)
	{
		t = free_node();
		SBT[t].init(v);
		return t;
	}
	else
	{
		SBT[t].s++;
		if (v < SBT[t].key)
			insert(SBT[t].left, v);
		else
			insert(SBT[t].right, v);
		maintain(t, v >= SBT[t].key);
		return t;
	}
}
node &getk(const int t, int k)
{
	//printf("t=%4d,",t);SBT[t].pri();getchar();
	if (k <= SBT[SBT[t].left].s  )return getk(SBT[t].left, k);
	if (k >= SBT[SBT[t].left].s + 2)return getk(SBT[t].right, k - SBT[SBT[t].left].s - 1);
	return SBT[t];
}
int head;
int main()
{
	int i, j;
	int a, b;
	char c;
	head = 0;
	SBT[0].s = 0;
	while (scanf("%c %d", &c, &a) != EOF)
	{
		switch (c)
		{
		case 'I':
			insert(head, a);
			break;
		case 'Q':
			printf("%d\n", getk(head, a).key);
			break;
		}
		getchar();
	}
}
```
