## 행렬 경로 문제
- 정수들이 저장된 nxn 행렬의 좌상단에서 우하단까지 이동한다. 단 오른쪽이나 아래쪽 방향으로만 이동할 수 있다.
- 방문한 칸에 있는 정수들의 합이 최소화되도록 하라.

## key Observation
(i,j)에 도달하기 위해서는 (i,j-1) 혹은 (i-1,j)를 거쳐야 한다.

## Recursive Algorithm
~~~
int mat(int i, int j){
    if(i ==1 && j ==1)
        return m[i][j];
    else if(i ==1)
        return mat(1, j-1) + m[i][j];
    else if(j==1)
        return mat(i-1, 1)+ m[i][j];
    else
        retrun Math.min(mat(i-1,j),mat(i,j-1))+m[i][j];
~~~

## Memoization
int mat (int i, int j){
    if(L[i][j] !=-1) return L[i][j];  // 있으면 바로 리턴 
    if( i==1 && j ==1)
        L[i][j] = m[i][j];
    else if(i==1)
        L[i][j] = mat(1, j-1)+m[i][j];
    else if(j==1)
        L[i][j] = mat(i-1,1)+m[i][j];
    else
       L[i][j]=Math.min(mat(i-1,j), mat(i),k-1)+m[i][j];
    return L[i][j];
}

위와 둘이 다른점은 저장하여서 리턴해주기때문에 새로 계산되지 않고 중복된 계산을하지않는다.

## Bottom-up
 ~~~
 int mat()
 {
     for(int i=1; i<=n; i++){
         for(int j=1; j<=n j++){
             if(i==1 && j==1)
                L[i][j] =m[1][1];
             else if(i==1)
                L[i][j] =m[i][j]+ L[i][j-1];
             else if(j==1)
                L[i][j]= m[i][j]+ L[i-1][j];
             else
                L[i][j] = m[i][j]+Math.min(L[i-1][j],L[i][j-1]);
         }
     }
     return L[n][n];
 }
 ~~~

 ## Common Trick
 ~~~
 // L[0][j]=[i][0] =무한대로 초기화 해줄 경우 자동으로 작은값을 고를떄 무시함으로
 //간단하게 코드 작성할수있음 2가지를 생략할수있다 i=1, j=1경우
  int mat(){
      for(int i=1; i<=n; i++){
          for(int j=1; j<=n; j++){
              if(i==1 & j==1)
               L[i][j]=m[1][1];
              else
                L[i][j]=m[i][j]+Math.min(L[i-1][j],L[i][j-1]);
          }
      }
      return L[n][n];
  }
 ~~~

 ## 경로 구하기
 ~~~
   int mat()
   {
       for(int i=1; i<=n; i++){
           for(int j=1; j<=n; j++){
               if(i==1 && j==1){
                   L[i][j] =m[1][1];
                   P[1][j] ='-';
                else{
                    if(L[i-1][j]<L[i][j-1]){
                        L[i][j] = m[i][j] + L[i-1][j];
                        P[i][j] ="<";  // 왼쪽에서왔다 
                    }
                    else{
                        L[i][j]=m[i][j]+L[i][j-1];
                        P[i][j] ="^"; //위쪽에서왔다
                    }
                }
               }
           }
       }
       return L[n][n];
   }
 ~~~
 시간복잡도 O(n^2)

 ## 경로 구하기1 (목적지에서 출발지로 출력)
 ~~~
  void printPath(){
      int i=n, j=n;
      while(P[i][j] !='-'){
          print(i + " "+j);
          if(p[i][j] == "<")
            j = j-1;
           else
            i = i-1;
      }
     print(i + " "+ j);

  }
~~~

## 경로구하기 (출발지에서 목적지로 출력)
~~~
// (1,1) -> (i,j)까지 출력
void printPathRecursive(i,j){
    if(P[i][j] =='-')
     print(i+" "+j);
    else{
        if(P[i][j] =="<")
            printPathRecursive(i,j-1); 
            //옆에서 왔으므로 그전에 경로를 출력해야함
        else 
            printPathRecursive(i-1,j);
            //위에서 왔다면 i-1,j 경로에서 왔으므로 그전 경로를 출력해야함
        print(i+" "+j);
    }
}
~~~