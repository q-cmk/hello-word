# hello-word#include <stdio.h>
#define MaxSize 100
#define M 8
#define N 8
int mg[M+2][N+2]=
{
    {1,1,1,1,1,1,1,1,1,1},
    {1,0,0,1,0,0,0,1,0,1},
    {1,0,0,1,0,0,0,1,0,1},
    {1,0,0,0,0,1,1,0,0,1},
    {1,0,1,1,1,0,0,0,0,1},
    {1,0,0,0,1,0,0,0,0,1},
    {1,0,1,0,0,0,1,0,0,1},
    {1,0,1,1,1,0,1,1,0,1},
    {1,1,0,0,0,0,0,0,0,1},
    {1,1,1,1,1,1,1,1,1,1}
};//构造地图
typedef struct
{
    int i;              //当前方块的横坐标 
    int j;              //当前方块的纵坐标 
    int di;             //di是下一可走相邻方位的方位号---上下左右的标记
} Box;
typedef struct
{
    Box data[MaxSize]; //是用来存储位置坐标的，当MaxSize<位置坐标的数目时，则程序出错，停止运行
    int top;            //栈顶指针
} StType;               //定义栈类型
//该栈中:每一个位置用于存储Box，该栈对应了一个int型的top作为指针，依次移动，方便对栈中每一个位置进行操作，而Box中，存储了该迷宫位置的横坐标，纵坐标，和从上一位置移动到该位置所对应的方向数值

int mgpath(int xi,int yi,int xe,int ye){
	int i,j,di,k,find;
	StType st;//创建栈 
	st.top=-1;//初始化空栈 
	st.top++;   //初始方块进站
	st.data[st.top].i=xi;
	st.data[st.top].j=yi;
	st.data[st.top].di=-1;
	mg[xi][yi]=-1;
	while(st.top>-1)
	{   printf(	"#");
		i=st.data[st.top].i;
		j=st.data[st.top].j;
		di=st.data[st.top].di;
		if(i==xe&&j==ye){
			printf("\n迷宫出口如下\n");
			for(k=0;k<=st.top;k++){
				printf("\t(%d,%d)",st.data[k].i,st.data[k].j);
				if((k+1)%5==0)
				printf("\n");
			} 
			printf("\n");
			return 0;
		}
		find=0;
		while(di<4 && find==0){
			di++;
		
			switch(di)
			{
			case 0:
				i=st.data[st.top].i-1;
				j=st.data[st.top].j;
				break;
			case 1:
				i=st.data[st.top].i;
				j=st.data[st.top].j+1;
				break;
			case 2:
				i=st.data[st.top].i+1;
				j=st.data[st.top].j;
				break;
			case 3:
				i=st.data[st.top].i;
				j=st.data[st.top].j-1;
				break; 
			}
			if(mg[i][j]==0) find=1;		
		}
		if(find==1){
			st.data[st.top].di=di;
			st.top++;
			st.data[st.top].i=i;
			st.data[st.top].j=j;
			st.data[st.top].di=-1;
			mg[i][j]=-1;
		}
		else{
			printf("O");
			mg[st.data[st.top].i][st.data[st.top].j]=0;
			st.top--;
		}
	} 
	   printf("该初始位置没有出口"); 
    return(0);
} 
int main(){
	int x,y,k,t;
	printf("终点坐标（8，8）");
	while(1){
    printf("请输入起点坐标:\n");
    printf("(横坐标范围:0~9,纵坐标范围:0~9)\n");
    scanf("%d %d",&x,&y);
    if(mg[x][y]!=0){
	printf("该位置不是通路，不可作为初始位置"); 
	} else{
		break;
	} 
	}
	mgpath(x,y,M,N);
	 printf("\n\n图像表示:\n");
	 for(k=0;k<M+2;k++)
	 {
	 	for(t=0;t<N+2;t++){
	 		if(mg[k][t]==-1){
	 			printf("O");
			 }
			 else if(mg[k][t]==0){
			 	printf(" ");
			 }else{
			 	printf("#"); 
			 }
			 
		 }
		 if(t==10) {
		 	printf("\n");
		 }
	 }
	  printf("\n此时，o所代表的图标为迷宫行走路径!!!\n");
    return 0;
}
