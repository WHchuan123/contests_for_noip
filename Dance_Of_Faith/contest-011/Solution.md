# The Solution

题目按CF顺序排序


## #A -Okabe and Future Gadget Laboratory

> 时间限制：  2s
>
> 空间限制：  256MB
>
> 主要算法：  模拟
>
> 时间复杂度：O(N^4)
> 
> 空间复杂度：O(N^2)

### 题意
给定一个N*M的矩阵，对于矩阵中每一个不等于1的数，存在与其同一行的某数加上与其同一列的某数使其之和围该数。
### 题解
模拟其题意，暴力枚举。


## #B Okabe and Banana Trees

> 时间限制：  2s
>
> 空间限制：  256MB
>
> 主要算法：  模拟
> 
> 时间复杂度：O(玄学)
>
> 空间复杂度：O(1)

### 题意
给定一条直线，找到直线上一点，使得由该点与原点组成的矩形中的所有点的权值之和最大（一点的权值为X+Y）。
### 题解
枚举可能的X，计算其Y，其权值之和为(X+1)*(Y+1)*X*Y/2。（可推导）


## #C Okabe and Boxes

> 时间限制：  3s
>
> 空间限制：  256MB
>
> 主要算法：  栈
>
> 时间复杂度：O(N)
>
> 空间复杂度：O(N)

### 题意
给定一个栈，并给出一个放东西（取东西）的顺序，要求弹出物品的顺序为1....N，如果在中间某步不符合要求，请改变栈内东西的顺序使得符合题意。要求改变顺序的次数最少。输出最小次数。
### 题解
可以肯定，在每次需要排序时按降序排列时最优的；但如果每次操作时都要进行一遍排序，那超时无疑。我们假设每次操作时并不真正去实现排序，而是给他一个标记，表示之前的元素已经排好序了。我们用一个栈来模拟其过程，每次要add时就是平常操作，弹出时如果不符合要求，则表示需要排序，此时我们将栈的top赋值为0，可以理解为栈底以下的元素都是有序的。如果在某次操作是top==0，则需要从栈底以下取出一个元素，此时一定是符合要求的（因为题目保证了元素在取出前一定被放入过），于是不需要干其他操作了。



## #D Okabe and City

> 时间限制：  4s
>
> 空间限制：  256MB
>
> 主要算法：  最短路
>
> 时间复杂度：O(N^2)
>
> 空间复杂度：O(N)

### 题意
在一个矩阵中有若干个暗点和亮点，相邻亮点之间走动不需要花费；可以站在亮点（原有的）上创建一排（列）伪列亮点，并可以在上边行走，创建一排（列）伪亮点的花费为1，求从（1，1）到右下角的最小花费，如不能到达，输出-1。
### 题解
显而易见，将每一个亮点当成一个点，如果（n，m）原来没亮，可以创建一个（n+1，m+1）的点（想一想，为什么）。相邻的亮点之间距离为0，当两个亮点有公共格点，或其X或Y的相差为2的时候，两点之间的权值为1，跑一遍dijkstra即可。



## #E Okabe and El Psy Kongroo

> 时间限制：  2s
>
> 空间限制：  256MB
>
> 主要算法：  DP+矩阵快速幂加速
>
> 时间复杂度：O(15^3*N*logK)
> 
> 空间复杂度：O(N)

### 题意
在坐标系的第一象限（包括X轴和Y轴），存在N条线段这些线段在X轴上的投影恰好不重叠的完全覆盖，从（0,0）开始走，对于（i，j）可以走到（i+1，j+1），（i+1，j），（i+1，j-1）三个位置，前题是不能越界（走到X轴以下）或走到线段上。 求从（0,0）到（k，0）有多少种走法。
### 题解
可以马上想到dp方程：dp[i,j]=dp[i-1,j-1]+dp[i-1,j]+dp[i-1,j+1]；

其中dp[i,j]，表示走到（i，j）的位置是的方案数。但由于k非常大，不仅会超时，也会超内存。考虑到j的范围很小（只有15），而且对于每一个i，只会有i-1转移过来，故可以构造矩阵进行快速幂。

考虑对于每一个i，只会有i-1转移过来，则需要一个2行16列的ans矩阵（初始矩阵的[0,0]应为1，表示边界）。

但由于每次最高的线段高度不同，所以每段转移矩阵都可能不一样，对一次构造矩阵时，可以这样定义：假设y为线段的高度（j<=y），则对于每一个j-1>=0，都有F[j-1,j]=1；对于每一个j<y，都有F[j+1,j]=1；对于每个j，都有F[j,j]=1。（考虑矩阵乘法的法则，ans每乘一次F就会被视为做一次DP，同时自动会完成滚动数组的妙处）。

转移矩阵F每次做b[i]-a[i]次幂，此时用快速幂。每次做完后都ans*F（幂后），注意，每次假如存在c[i+1]<c[i]，则需要把ans[j,0]（c[i+1]+1<=j<=c[i]）赋值为0（因为不存在的状态不能被转移）。
