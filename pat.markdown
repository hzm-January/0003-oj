# PAT A1002  A+B for Polynomials (25) [模拟]
## 题目
This time, you are supposed to find A+B where A and B are two polynomials.<br/>
Input <br/>
Each input file contains one test case. Each case occupies 2 lines, and each line contains the information of a polynomial: K N1 aN1 N2 aN2 … NK aNK, where K is the number of nonzero terms in the polynomial,Ni and aNi (i=1, 2, …, K) are the exponents and coeficients, respectively. It is given that 1 <= K <= 10，0<= NK < … < N2 < N1 <=1000.
<br/>Output <br/>
For each test case you should output the sum of A and B in one line, with the same format as the input. <br/>
Notice that there must be NO extra space at the end of each line. Please be accurate to 1 decimal place. <br/>
Sample Input<br/>
2 1 2.4 0 3.2<br/>
2 2 1.5 1 0.5<br/>
Sample Output<br/>
3 2 1.5 1 2.9 0 3.2<br/>
## 题目分析
多项式相加，输出加和后-多项式的项数和结果
## 解题思路
用double数组保存每个项的系数和，其下标对应幂次
## 思维盲点
系数有正有负
## 易错点
多项式的项数，不能在输入时统计，因为可能同一幂次会被统计多次。忽略了相等幂次正负抵消的情况
例：
    刚开始为0 ，cnt++，添加一个负数
    添加一个正数，抵消为0
    添加一个负数，因为为0，所以 cnt++
    添加一个正数，抵消为0
    同一个幂次的项，统计了两次。其实结果系数为0，不用被统计到最终结果中，应该为0次			

## Code
```C++
#include <iostream>
using namespace std;
/*
	错误：在输入时统计项数，忽略了相等幂次正负抵消的情况
			刚开始为0 ，cnt++，添加一个负数
			添加一个正数，抵消为0
			添加一个数，因为为0，所以 cnt++
			同一个幂次，加了两次
*/
double c,cs[1010]; //cs为加和结果容器，下标对应多项式幂次，元素对应其系数 
int n,e,cnt; // cnt 最终项数 
int main(int argc,char * argv[]) {
	for(int i=0; i<2; i++) {
		scanf("%d",&n);
		while(n-->0) {
			scanf("%d %lf", &e, &c);
//			if(cs[e]==0)cnt++; //错误，不能再这里统计，有正负相互抵消的可能，同一幂次会统计多次
			cs[e]+=c;
		}
	}
	for(int i=1010-1; i>=0; i--) {
		if(cs[i]!=0) cnt++;
	}
	printf("%d",cnt);
	for(int i=1010-1; i>=0; i--) {
		if(cs[i]!=0) {
			printf(" %d %.1f", i, cs[i]);
		}
	}
	return 0;
}
```

# A1126 Eulerian Path (25分)[dfs]
## 题目
判断无向图是欧拉图，半欧拉图，还是非欧拉图
欧拉环：连通并且所有顶点的度都为偶数
欧拉路径：连通并且仅有两个顶点的度为奇数，其余顶点的度都为偶数
## 分析
1 表存储图，无向图每个顶点的邻接顶点个数即为顶点的度
2 dfs判断图是否连通
## Code
```C++
#include <iostream>
#include <vector>
using namespace std;
/*
	欧拉环：连通并且所有顶点的度都为偶数
	欧拉路径：连通并且仅有两个顶点的度为奇数，其余顶点的度都为偶数
*/
const int maxn = 510;
int n,m,cnt,odd,vis[maxn];
vector<int> g[maxn];
void dfs(int v) {
	cnt++;
	vis[v]=1;
	if(g[v].size()==0)return;
	for(int i=0; i<g[v].size(); i++)
		if(vis[g[v][i]]==0)dfs(g[v][i]);
}
int main(int argc,char * argv[]) {
	// 1 建图
	int v1,v2;
	scanf("%d%d",&n,&m);
	for(int i=0; i<m; i++) {
		scanf("%d%d",&v1,&v2);
		g[v1].push_back(v2);
		g[v2].push_back(v1);
	}
	// 2 dfs判断是否连通
	dfs(1);
	// 3 判断 打印
	for(int i=1; i<=n; i++) {
		if(i!=1)printf(" ");
		printf("%d",g[i].size());
		if(g[i].size()%2!=0) odd++;
	}
	printf("\n");
	if(odd==0&&cnt==n)
		printf("Eulerian");
	else if(odd==2&&cnt==n)
		printf("Semi-Eulerian");
	else
		printf("Non-Eulerian");
	return 0;
}
```
