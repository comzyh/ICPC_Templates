```cpp
int tarjan(int n)
{
    LOW[n]=DFN[n]=++Time;
    instack[n]=1;
    stack[++st]=n;
    int i,j;
    for (i=head[n];i!=0;i=es[i].next)
    {
        int t=es[i].t;
        if (!DFN[t])
            tarjan(t);      
        if (instack[t] && LOW[t]<LOW[n])LOW[n]=LOW[t];
    }
    if (DFN[n]==LOW[n])
    {
        BR++;
        do
        {
            j=stack[st--];
            instack[j]=0;
            belong[j]=BR;
        }while (j!=n);
    }
    return 0;   
}
int main()
{
    for (i=1;i<=N;i++)
        if (!DFNN[i])
            tarjan(i);
}
```
