## Dynamic bottom-up 방식
~~~
int fib(int n)
{
  f[1]=f[2] =1;
  for(int i=3; i<=n; i++)
    f[n]=f[n-1]+f[n-2];
  return f[n];
}
~~~
- 중복 계산을 피함

## 이항계수(binomial coefficeint)
- n개 중에서 k를 선택 경우의 수 
 1. recursion 으로 풀 경우 아래 코드 중복이 많이 됨 
~~~
int binomial(int n, int k){
   if(n== k || k==0) // base case 
        return 1;
   else
       return binomial(n-1, k)+ binomial(n-1, k-1);     
}
~~~
 2. Memoization 으로 풀 경우 
 ~~~
int binomial(int n, int k)
{
  if(n==k || k ==0)
    return 1;
  else if(binom[n][k] > -1) /*배열 binom이 -1로 초기화 되어 있다고 가정 */
    return binom[n][k]
  else {
    binom[n][k] = binomial(n-1,k)+binomial(n-1,k-1);
      return binom[n][k];
  }     
} 
~~~
3. bottom-up 방식으로 풀 경우
~~~
int binomial(int n, int k){
    for(int i=0; i<=n; i++){
       for(int j=0; j<=k && j<=i; j++){
        if(k==0 || n==k)
            binom[i][j] =1;
        else{
            binom[i][j] = binom[i-1][j-1]+ binom[i-1][j];
        }    
       }
   return binom[n][k];
}
~~~

## Memoization vs Dynamic Programming
- 순환식의 값을 계산하는 기법
- 둘다 동적계획법의 일종으로 보기도 함
- Memoization은 top-down 방식 실제로 필요한 subproblem만을 
- 동적계획법은 bottom-up 방식이며 , recursion에 수반되는 overhead 가 없음 

-----------------------------------------------
## 행렬 경로 문제 
- 정수들이 저장된 nxn 행렬의 좌상단에서 우하단까지 이동한다. 단 오른쪽이나 아래쪽 방향으로만 이동
- 방문한 칸에 있는 정수들의 합이 최소화 되도록 해라 
- (i,j)에 도달하기 위해서는 (i,j-1) 또는 (i-1, j) 거쳐야함
- 또한 (i,j-1) 혹은 (i-1, j) 최소의 방법으로 다가가야함

 순환식  
 L[i,j] : (1,1)에서 (i,j)까지 이르는 최소합
 - mij (if i=1 and j=1) (출발점)
 - L[i-1, j] + mij   (if j=1) (아래쪽으로 내려오는 방법밖에없음 외길)
 - L[i, j-1]+mij  (if i=1) (오른쪽으로 가는 방법밖에없음 외길)
 - min(L[i-1, j], L[i,j-1])+min (otherwise) (나머지 경우)
 
 - 순환식으로 푼 알고리즘
 ~~~
 int mat(int i, int j)
 {
    if(i ==1 && j==1){
        return m[i][j]
    }else if(i ==1){
        return mat(1, j-1)+ m[i][j];
    }else if(j ==1){
        retunr mat(i-1, 1)+m[i][j];
    }else{
        return Math.min(mat(i-1,j),mat(i,j-1))+m[i][j];
    }
 }
 ~~~
 
 - Memoization
 int mat(int i, int j){
    if(L[i][j] !=-1) return L[i][j];
    if(i==1 && j==1)
        L[i][j] =m[i][j];
    else if (i==1)
        L[i][j]= mat(1,j-1)+m[i][j];
    else if(j==1)
        L[i][j]=mat(i-1, j)+m[i][j];
    else{
        L[i][j] = Math.min(mat(i-1, j),mat(i,j-1))+m[i][j]
    }    
    return L[i][j];
 }
 
 - Bottom-up : 순서로 계싼하면 필요한 값이 항상 먼저 계산됨 (시간복잡도: O(n^2))
 ~~~
 int mat(){
    for(int i=1; i<=n; i++){
        for(int j=1; j<=n; j++){
            if(i ==1 & j==1){
                L[i][j]= m[1][1];
            }
            else if(i==1){
                L[i][j]=L[i][j-1]+m[i][j];
            }else if(j==1){
                L[i][j]=L[i-1][j]+m[i][j];
            }else{
              L[i][j]=Math.min(L[i-1][j],L[i][j-1)+m[i][j];
            }
        }
    }
   return L[n][n];
 }
 ~~~
 
 - Common Trick 시간복잡도 O(n^2)
 ~~~
 /*
    initialize L with L[0][j]=L[i][0]= 무한대 for all i and j
 */
 int mat(){
    for(int i=1; i<=n; i++){
      for(int j=1; j<=n; j++{
        if(i==1 && j==1)
          L[i][j]= m[1][1];
        else{
          L[i][j]= m[i][j]+Math.min(L[i-1][j],L[i][j-1]);
        }  
      }
    }
    return[n][n];
 }
 ~~~
 
 ## 경로 구하기 (목적지부터시작해서 돌아감)
 ~~~
 void printPath(){
    int i=n, j=n;
    while(p[i][j] != "-"){
       print(i+""+j);
       if(p[i][j] == "<-")
         j=j-1;
       else
         i=i-1;   
    }
    print(i+""+j);
 }
 
 void printPathRecursive(i,j){
   if(p[i][j  =="-")
     print(i+""+j);
   else{
     if(p[i][j] =="<-")
       printPathRecursive(i,j-1);
     else
      printPathRecursive(i-1,j);
     print(i+""+j);   
   }
 }
 ~~~
 -----------------------------------------------
 
 