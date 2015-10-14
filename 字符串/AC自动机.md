
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <queue>
using namespace std;
struct Trie
{
	Trie *next[26], *fail;
	int val;
} ns[270000];
int T, N, R, ANS;
char str[1000009];
inline void build(char *w)
{
	Trie *node = &ns[1];
	while (*w >= 'a' && *w <= 'z')
	{
		if (node->next[*w - 'a'] == 0)
			node->next[*w - 'a'] = &ns[++R];
		node = node->next[*w - 'a'];
		w++;
	}
	node->val++;
}
inline void build_ac()
{
	queue<Trie*>q;
	ns[1].fail = &ns[1];
	q.push(&ns[1]);
	while (!q.empty())
	{
		Trie *h = q.front(), *f;
		q.pop();
		for (int i = 0; i < 26; i++)
			if (h->next[i])
			{
				f = h->fail;
				while (f->next[i] == 0 && f != &ns[1])
					f = f->fail;
				if (f->next[i] && h != &ns[1])
					h->next[i]->fail = f->next[i];
				else
					h->next[i]->fail = &ns[1];
				q.push(h->next[i]);
			}
	}
}
void find()
{
	Trie *node = &ns[1];
	for (int i = 0; str[i] >= 'a' && str[i] <= 'z'; i++)
	{
		char &c = str[i];
		while (node->next[c - 'a'] == 0 && node != &ns[1])
			node = node->fail;
		if (node->next[c - 'a'])
			node = node->next[c - 'a'];
		for (Trie *f = node; f != &ns[1]; f = f->fail)
		{
			ANS += f->val;
			f->val = 0;
		}
	}
}
int main()
{
	scanf("%d", &T);
	while (T--)
	{
		scanf("%d", &N);
		memset(ns, 0, sizeof(ns));
		char word[100];
		R = 1;
		for (int i = 1; i <= N; i++)
		{
			scanf("%s", word);
			build(word);
		}
		build_ac();
		scanf("%s", str);
		ANS = 0;
		find();
		printf("%d\n", ANS);
	}
	return 0;
}
```
