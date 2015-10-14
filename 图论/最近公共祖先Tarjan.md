```cpp
//POJ 1986
#include <iostream>
#include <cstring>
#include <cstdio>
using namespace std;
struct link { //链表结构体;目标,长度 ,下一个
    int t, l, next;
};
struct node_request { //node request
    int ind, t;
    int next;
};
//Map
int lhead[40005];//邻接表头指针
link lin[100000];//链表(无向图)
//Tarjan
int rhead[40005];
node_request nqs[100000];//链表(请求)
int sum[40005], level[40005]; //类似栈的部分和(被DFS更新),深度
int f[40005];//并查集
int dis[40005];//Distance about Root
int ans[40005];
int visited[40005];
//Other
int N, M, K;
//Sub
int getf(int x);
inline void add(int x, int y); //Add X to Y
void DFS(int x, int k); //节点,层数
int main() {
    int i, j;
    int fr, t, l;
    int R = 0; //链表尾指针
    char rubbish;
    scanf ("%d%d", &N, &M);
    lin[0].next = 0;
    memset(lhead, 0, sizeof(lhead));
    for (i = 1; i <= M; i++) { //读入
        scanf ("%d%d%d %c", &fr, &t, &l, &rubbish);
        R++;
        lin[R].next = lhead[fr];
        lin[R].t = t;
        lin[R].l = l;
        lhead[fr] = R;
        R++;
        lin[R].next = lhead[t];
        lin[R].t = fr;
        lin[R].l = l;
        lhead[t] = R;
    }
    //Finish Build Map ...
    //Begin Read Request ...
    scanf("%d", &K);
    R = 0;
    memset(rhead, 0, sizeof(rhead));
    for (i = 1; i <= K; i++) {
        scanf("%d%d", &fr, &t);
        if (fr == t) {
            ans[i] = 0;
            continue;
        }
        R++;
        nqs[R].ind = i;
        nqs[R].t = t;
        nqs[R].next = rhead[fr];
        rhead[fr] = R;
        R++;
        nqs[R].ind = i;
        nqs[R].t = fr;
        nqs[R].next = rhead[t];
        rhead[t] = R;
    }
    //End reading...
    for (i = 1; i <= N; i++) {
        f[i] = i;
    }
    memset(dis, 0, sizeof(dis));
    memset(visited, 0, sizeof(visited));
    //Begin
    DFS(1, 1);
    for (i = 1; i <= K; i++) //按顺序输出结果
        printf("%d\n", ans[i]);
    system("pause");
}
int getf(int x) {
    if (f[x] == x)return x;
    int ret = getf(f[x]); //最终祖先
    dis[x] += dis[f[x]]; //x到祖先的距离=x到f[x]的距离+f[x]到祖先的距离
    return f[x] = ret;
}
inline void add(int x, int y) {
    //这里的并查集并不需要getf
    if (getf(x) == getf(y))return ;
    f[x] = y;
}
void DFS(int x, int k) { //Tarjan 算法主函数
    level[x] = k;
    int i;
    //处理请求
    i = rhead[x]; //从链表里读请求
    while (i != 0) {
        if (visited[nqs[i].t]) {
            //printf("Get visited node %4d \n",nqs[i].t);
            getf(nqs[i].t);//做路径压缩； 然后就可以开心的使f[x]而不是getf(x)了
            //printf("Request to %d ,target is %4d away from its Root(%4d )\n",nqs[i].t,dis[nqs[i].t],f[nqs[i].t]);
            ans[nqs[i].ind] = dis[nqs[i].t] + sum[k] - sum[level[getf(nqs[i].t)]];
        }
        i = nqs[i].next;
    }
    visited[x] = 1;
    //扩展
    i = lhead[x];
    while (i != 0) {
        if (!visited[lin[i].t]) {
            sum[k + 1] = sum[k] + lin[i].l;
            DFS(lin[i].t, k + 1); //DFS
            f[lin[i].t] = x; //归入并查集
            dis[lin[i].t] = lin[i].l;
        }
        i = lin[i].next;
    }
}
```
