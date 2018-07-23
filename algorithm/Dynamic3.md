## 동적 계획법
1. 일반적으로 최적화문제 (optimisation problem) 혹은 카운팅(counting)문제에 적용됨
2. 주어진 문제에 대한 순환식(recurrence equation)을 정의한다.
3. 순환식은 memoization 혹은 bottom-up방식으로 푼다.
   
- 순환식 그문제를 풀기위해서 어떤 부수적인 문제를 풀어야하고 그것을 조합하는것
- subproblem들을 풀어서 원래 문제를 푸는방식, 그런 의미에서 분할정복법과 공통성이 있음
- 분할정복법에서는 분할된 문제들이 서로 disjoint하지만 동적계획법에서는 그렇지 않음
- 즉 서로 overlapping 하는 subproblem들을 해결함으로써 원래 문제를 해결

## 분할정복법 vs 동적계획법
 QuickSort의 경우 pivot을 기준으로 분할된 두 subproblem은 서로 disjoint하다.
 
## Optimal Substructure
  - 어떤 문제의 최적해가 그것의 subproblem들의 최적해로부터 효율적으로 구해 질 수 있을때 
   그 문제는 optimal substructure를 가진다고 말한다.
  - 분할정복법, 탐욕적기법, 동적계획법은 모두 문제가 가진 이런 특성을 이용한다.
    
## Optimal Substructure를 확인하는 질문
- 최적해의 일부분이 그부분에 대한 치적해인가?
- 최단경로 문제 (shortest-path)
   - 이 경로가 s에서 u까지 최단경로라면 이경로는 s에서 v까지의 최단경로이다.
-  순환식은 Optimal subStructure를 표현한다
   - u까지 가는 최단경로의 길이는 u에 인접한 모든 정점 v에 대해서 v까지 가는 최단경로의 길이 더하기 에지(v,u)의 가중치의 합의 최소값이다. 
  
  차근차근하는법
  1. 순환식 세우는것이 중요함 (동적계획법에서 핵심)
  2. memoization, bottom-up 방식으로 푼다

## Optimal Substructure을 확인하는 질문 
  - 최장경로 (Longest-Path)문제
    - 노드를 중복 방문하지 않고 가는 가장 긴 경로
    - Optimal substructure을 가지는가?
    - u까지 가는 최장경로가 v를 지난다고 하더라도 그 경로상에서 v까지 가는 경로가 반드시 
      v까지 가는 최장경로가 아닐수도 있으므로 이런순환식은 성립하지 않는다
    - 최장 경로 문제는 optimal substructure가 성립하지 않는것은 아니고 복잡한 형태로 세울수 있음 

## 행렬의 곱셈 
- p * q 행렬 A와 q * r 행렬 B 곱하기
~~~
 void product(int A[][], int B[][], int C[][]){
    for(int i=0; i<p; i++){
      for(int j=0; j<r; j++){
         C[i][j] =0;
         for(int k=0;k<q; k++)
          C[i][j] +=A[i][k]*B[k][j];
        }
    }
 }

~~~

## Matrix-Chain 곱하기
- 행렬 A는 10*100, B는 100*5, C는 5*50
- 세 행렬의 곱은 ABC는 두가지 방법으로 계산가능(결합법칙이 성립)
  - (AB)C: 7500번의 곱셈이 필요(10*100*5+ 10*5*50)
  - A(BC): 75000번의 곱셍이 필요(100*5*50+ 10*100*50)
-  즉 곱하는 순서에 따라 연산량이 다름
-  n개의 행렬의 곱 A1A2A3 ...An을 계산하는 최적의 순서?
-  여기서 Ai는 Pk-1*Pk 행렬이다.

## 순환식
m[i,j]: AiAi+1 ...Aj를 곱하는 최소 곱셈횟수
 
 m[i,j] = 0 (if i=j)
 m[i,j] = min i<=k<= j-1 (m[i,k]+m[k+1,j]+Pi-1PkPj) (if i<j )

 ## 동적계획법
 ~~~
 int matrixChain(int n){
  for(int i=1; i<=n; i++)
    m[i][i]=0;
    for(int r=1; r<=n-1; r++){ //대각선의 갯수 
      for(int i=1; i<=n-r; i++){ // 각 대각선의 값의 개수 
        int j= i+r;  ///r 번재의 대각선의 i번째의 값
         m[i][j]=m[i+1][j] + p[i-1]*p[i]*p[j];
         for(int k=i+1; k<=j-1; k++){
           if(m[i][j]> m[i][k]+m[k+1][j]+ p[i-1]*p[k]p[j])//최소값
              m[i][j]=m[i][k]+m[k+1][j]+p[i-1]*p[k]p[j]
         }
      }
    }
    return m[1][n];
 }
 ~~~
 시간 복잡도 O(n^3)

## Longest Common Subsequence(LCS문제)
 - <bcdb>는 문자열 <abcbdab>의 subsequence이다.
 - <bca>는 문자열 <abcdbab>와 <bdcaba>의 common subsequence이다.
 - Longest common subsequence(LCS)
    - common subsequence들 중 가장 긴것
    - <bcba>는 <abcbab>와 <bdcaba>의 LCS이다 

## Brute Force
- 문자열 x의 모든 subsequencse에 대해서 그것이 y의 subsequence가 되는지 검사한다.
- |x|=m,|y|=n
- x의 subsequence의 개수 =2^m
- 각각의 y의 subsequence인지 검사 : O(n)시간
- 시간복잡도 O(n2^m)

## 순환식
L[i,j]:문자열 X=<x1x2...xi>와 Y=<y1y2...yj>의 LCS의 길이
 - 경우1 xi=yj
     L[i,j] =L[i-1,j-1]+1
 - 경우2 xi != yj
     L[i,j] = max(L[i-1,j],L[i,j-1])

## 동적 계획법
~~~
//시간 복잡도 O(mn)
int lcs(int m, int n)
{
   for (int i=0; i<=m; i++)
      c[i][0]=0;
   for (int j=0; j<=n; j++)
      c[0][j]=0;
   for (int i=0; i<=m; i++){ //행
     for(int j=0; j<= n; j++){ //열 
       if(x[i]==y[j])
           c[i][j] = c[i-1][j-1]+1;
       else
           c[i][j]= Math.max(c[i-1][j],c[i][j-1]);
     }
   }
    return c[m][n];
}
~~~

## Knapsack Problem
- n개의 아이템과 배낭
- 각각의 아이템은 무게 Wi와 가격 Vi를 가짐
- 배낭의 용량 W
- 목적: 배낭의 용량을 초과하지 않으면서 가격이 최대가 되는 부분집합

## 탐욕
- 가격이 높은것 부터 선택
- 무게가 가벼운것 부터 선택
- 단위 무게당 가격이 높은것 부터 선택
  
## 순환식
- OPT(i): 아이템 1,2,....i로 얻을 수 있는 최대 이득
- 경우 1: 아이템 i를 선택하지 않는 경우
   - OPT(i)=OPT(i-1)
- 경우 2: 아이템 i를 선택하는 경우
   - OPT(i)=? 
- OPT(i,w): 배낭 용량이 w일때 아이템 1,2,3,...,i로 얻을 수 있는 최대이득
- 경우 1: 아이템 i를 선택하지 않는 경우 
    - OPT(i,w) = OPT(i-1,w)
- 경우 2: 아이템 i를 선택하는 경우
   - OPT(i-1, w-wi) 


~~~java
public class Bag{
    static int N; // 아이템의 수
    static int W;// 배낭무게 (용량)
    static int []Wi; // 아이템 무게
    static int[]Pi; // 아이템의 가치
    static int[][] D;// 다이나믹 배열

    static int max(int a, int b){
      return (a>b) ? a : b;
    }

public static void main(String[] args){
  Scanner sc= new Scanner(System.in);

  N=sc.nextInt();
  W=sc.nextInt();

  Wi=new int[N+1];
  Pi=new int[N+1];
  D = new int [N+1][W+1];

  for(int i=1; i<=N; i++){
    Wi[i]=sc.nextInt();
    Pi[i]=Sc.nextInt();
  }
  //i는 아이템의 종류
  for(int i=1; i<=N; i++){
    //j는 아이템의 무게
    // 내무게보다 작은 배낭은 이전에 구한값을 참고한다.
    for(int j=1; j<=w; j++){
      if(j-Wi[i]<0){
        D[i][j]=D[i-1][j];
      }else{
        /*
        현재 넣으려고 하는 아이템을 배낭에서 무게를 빼면 이전에 넣은 보석의 가치를 가지고 올수있음 
        여기다가 가치를 더한값과 이전 배낭에서 구한값을 비교하여 더 좋은 값을 취함
        */
        D[i][j]=max(D[i-1][j-Wi[i]])+Pi[i]  D[i-1][j]);
      }
    }
  }
  System.out.println(D[N][W]);
  sc.close();
}
}

~~~

## 시간복잡도
 - 시간복잡도 O(nW)
 - 다항시간인가? 