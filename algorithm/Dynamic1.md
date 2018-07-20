## Dynamic Programming

## Fibonacci Number

- 재귀 함수로 푸는방법 (계산이 너무 중복됨 비효율적이다). 
  새로 계산을 계속 되풀이 해줘야함..

~~~
 int fib(int n)
 {
 	if(n ==1 || n ==2)
 		return 1;
 	else
 		return fib(n-2)+fib(n-1);

 }
~~~


- Memoization 
  - 중간 계산 결과를 caching 함으로써 중복 계산을 피함

~~~
int fib(int n)
{
if(n ==1 || n==2)
    return 1;
else if(f[n] > -1)   //배열 f가 -1으로 초기화 되어 있다고 가정
    return f[n];      // 즉 이미 계산된 값이라는 의미
else{
    f[n]= fib(n-2)+fib(n-1); // 중복 계산 결과를 caching
    return f[n]
}
}
~~~

- bottom-up 방식
  - bottom up 방식으로 중복계산을 피함

 ~~~
 	int fib(int n)
 	{
 		f[1]= f[2] =1;
 		for(int i=3; i<=n; i++)
 		  f[n]= f[n-1]+f[n-2];
 		 return f[n];
 	}
 ~~~

 ## 이항계수(Binomial Coefficient)

 - 재귀로 푸는 방법 (많은 계산이 중복됨)

 ~~~
   int binomial(int n, int k)
   {
    if( n== k || k ==0)
        return 1;
    else
        return binomial(n-1, k)+ binomial(n-1, k-1);	

   }
 ~~~

 - Memoization
~~~
int binomial(int n, int k){
    if(n==k || k==0)
        return 1;
    else if(binom[n][k]>-1) // -1로 초기화 되어있다고 가정
        return binom[n][k];
    else{
        binom[n][k] = binomial(n-1,k)+binomial(n-1,k-1);
        return binom[n][k];
    }
}
~~~

- bottom-up
~~~
  int binomial(int n, int k){
      for(int i=0; i<=n; i++){
          for(int j=0; j<=k && j<=i; j++){
              if(k==0 || n==k)
                binom[i][j]=1;
              else
                 binom[i][j] =binom[i-1][j-1]+binom[i-1][j];
          }

      }
    return binom[n][k];
  }
~~~

## Memoization vs Dynamic Programming
- 순환식의 값을 계산하는 기법들이다.
- 둘 다 동적 계획법의 일종으로 보기도함
- Momoization은 top-down 방식이며, 실제 필요한 subproblem만 푼다
- 동적계획법은 bottom-up 방식이며, recursion 에 수반되는 overhead가 없다.
 