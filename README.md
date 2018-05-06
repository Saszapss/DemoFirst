# DemoFirst
/*距离矢量路由算法实验模拟  
输入：网络拓扑图，表示网络中结点之间的联系  
输出：一个节点（结点A）从初始到稳定的所有路由表数据以及更新次数。  
*/  
#include<iostream>  
using namespace std;  
#define N 8  
  
  
struct Node  
{  
    int destination;  
    int nextJump;  
    int cost;  
    struct Node* nextNode;  
};  
typedef Node* NodePtr;  
  
int graph[N][N];            //保存网络拓扑图   
NodePtr routeHeaders[N];    //旧路由表  保存路由表头指针   
NodePtr newRouteHeaders[N]; //新的路由表  
int gxcount = 0;            //更新次数   
bool isChange=false;        //更新一次后，路由表是否改变   
  
void InitGraph();           //初始化网络拓扑图   
void InitRouteTable();      //初始化路由表   
void ShowRouteTable(NodePtr head);      //打印某个结点的路由表   
NodePtr CopyRouteTable(NodePtr head);   //复制某个结点的路由表，返回头指针   
void DeleteRouteHeaders(NodePtr head);  //删除以head为头结点的路由表，该实验主要用来删除旧表   
void Update();              //进行更新，由旧表得到新表   
  
  
  
int main()  
{  
    InitGraph();  
	InitRouteTable();  
      
    while(1)  
    {  
        isChange = false;  
        Update();  
        ShowRouteTable(routeHeaders[0]);  
        gxcount++;  
        if(!isChange)   break;  
          
    }   
    cout<<"更新次数为："<<gxcount<<endl;
	cout<<"说明更新"<<(gxcount-1)<<"次后已经稳定"<<endl;  
      
    for(int i=0; i<N; i++)   DeleteRouteHeaders(newRouteHeaders[i]); //删除最后的路由表   
  
    return 0;  
}   
  
  
//初始化网络拓扑图   
void InitGraph()  
{  
    memset(graph,-1,sizeof(graph));       
    graph[0][1]=2;  graph[0][6]=6;  
    graph[1][0]=2;  graph[1][2]=7;  graph[1][4]=2;  
    graph[2][1]=7;  graph[2][3]=3;  graph[2][5]=3;  
    graph[3][2]=3;  graph[3][7]=2;  
    graph[4][1]=2;  graph[4][5]=2;  graph[4][6]=1;  
    graph[5][4]=2;  graph[5][2]=3;  graph[5][7]=2;  
    graph[6][0]=6;  graph[6][4]=1;  graph[6][7]=4;  
    graph[7][3]=2;  graph[7][5]=2;  graph[7][6]=4;  
}  
  
//初始化路由表  
void InitRouteTable()  
{  
    for(int i=0; i<N; i++)   
    {  
        routeHeaders[i] = NULL;  
    }  
      
    for(int i=0;i<N;i++)  
    {  
        for(int j=0;j<N;j++)  
        {  
            if(graph[i][j]>0)  
            {  
                NodePtr newNode = (N
