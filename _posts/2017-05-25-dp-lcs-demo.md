---
layout: post
title: "DP LCS"
description: ""
categories: [ALG]
tags: [dp, lcs]
redirect_from:
  - /2017/05/25/
---

{:toc .toc}
## 给定两个序列 X 和 Y，求 X 和Y 的最长公共子序列 (longest common subsequence)。

```cpp
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#define MAX 200
#define max(a,b) ((a)>(b)?(a):(b))
int lcs(const char *X,const int n,const char *Y,const int m)
{
    int i,j;
    /**f[i][j]表示X(1..i)和Y(1..j)的LCS长度*/
    int f[MAX+1][MAX+1];
    memset(f,0,sizeof(f));
    for(i=1; i<=n; i++)
    {
        for(j=1; j<=m; j++)
        {
            if(X[i-1]==Y[j-1])
                f[i][j]=f[i-1][j-1]+1;
            else
                f[i][j]=max(f[i-1][j],f[i][j-1]);
        }
    }
    return f[n][m];
}
int p[MAX+1][MAX+1];
int lcs_extend(const char *X,const int n,const char *Y,const int m)
{
    int i,j;
    /**f[i][j]表示X(1..i)和Y(1..j)的LCS长度*/
    int f[MAX+1][MAX+1];
    memset(f,0,sizeof(f));
    memset(p,0,sizeof(p));
    for(i=1; i<=n; i++)
    {
        for(j=1; j<=m; j++)
        {
            if(X[i-1]==Y[j-1])
            {
                f[i][j]=f[i-1][j-1]+1;
                p[i][j]=1;
            }
            else
            {
                if(f[i-1][j]>=f[i][j-1])
                {
                    f[i][j]=f[i-1][j];
                    p[i][j]=2;
                }
                else
                {
                    f[i][j]=f[i][j-1];
                    p[i][j]=3;
                }
            }
        }
    }
    return f[n][m];
}
void lcs_print(const char *X,const int n,const char *Y,const int m)
{
    if(m==0||n==0)
        return ;
    if(p[n][m]==1)
    {
        lcs_print(X,n-1,Y,m-1);
        printf("%c",X[n-1]);
    }
    else if(p[n][m]==2)
        lcs_print(X,n-1,Y,m);
    else
        lcs_print(X,n,Y,m-1);
}
int main()
{
    char X[MAX+1];
    char Y[MAX+1];
    FILE *filein=fopen("lcs.txt","rb");
    while(fscanf(filein,"%s %s",X,Y)!=EOF)
    {
        int n=strlen(X);
        int m=strlen(Y);
//        printf("%s,%s 的最长公共子序列长度是：%d\n",X,Y,lcs(X,n,Y,m));
        printf("%s,%s 的最长公共子序列长度是：%d\n",X,Y,lcs_extend(X,n,Y,m));
        lcs_print(X,n,Y,m);
        printf("\n");
    }
    return 0;
}
```
