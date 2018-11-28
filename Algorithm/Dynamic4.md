# Codility NumberSolitaire

## 문제 
~~~
A game for one player is played on a board consisting of N consecutive squares, numbered from 0 to N − 1. There is a number written on each square. A non-empty array A of N integers contains the numbers written on the squares. Moreover, some squares can be marked during the game.

At the beginning of the game, there is a pebble on square number 0 and this is the only square on the board which is marked. The goal of the game is to move the pebble to square number N − 1.

During each turn we throw a six-sided die, with numbers from 1 to 6 on its faces, and consider the number K, which shows on the upper face after the die comes to rest. Then we move the pebble standing on square number I to square number I + K, providing that square number I + K exists. If square number I + K does not exist, we throw the die again until we obtain a valid move. Finally, we mark square number I + K.

After the game finishes (when the pebble is standing on square number N − 1), we calculate the result. The result of the game is the sum of the numbers written on all marked squares.

For example, given the following array:

    A[0] = 1
    A[1] = -2
    A[2] = 0
    A[3] = 9
    A[4] = -1
    A[5] = -2
one possible game could be as follows:

the pebble is on square number 0, which is marked;
we throw 3; the pebble moves from square number 0 to square number 3; we mark square number 3;
we throw 5; the pebble does not move, since there is no square number 8 on the board;
we throw 2; the pebble moves to square number 5; we mark this square and the game ends.
The marked squares are 0, 3 and 5, so the result of the game is 1 + 9 + (−2) = 8. This is the maximal possible result that can be achieved on this board.

Write a function:

class Solution { public int solution(int[] A); }

that, given a non-empty array A of N integers, returns the maximal result that can be achieved on the board represented by array A.

For example, given the array

    A[0] = 1
    A[1] = -2
    A[2] = 0
    A[3] = 9
    A[4] = -1
    A[5] = -2
the function should return 8, as explained above.

Assume that:

N is an integer within the range [2..100,000];
each element of array A is an integer within the range [−10,000..10,000].
Complexity:

expected worst-case time complexity is O(N);
expected worst-case space complexity is O(N) (not counting the storage required for input arguments).
~~~

## 문제해석
- 조약돌은 행렬에 0번째에 존재함
- 주사위를 던졌을때 3이나오면 행렬의 3번째 즉 A[3]에 조약돌이 가진다.
- 그다음에 5를 던지면 A[8]로 가야되는데 없으므로 움직이지 않음 A[3]에 존재
- 그다음에 2를 던지면 A[5]로 가지고 게임이 끝남.
- 조약돌은 0,3,5 에 존재하게되고 게임 결과는 1+9+(-2) = 8 이 결과에서 얻을 수있는 최대값이다.
- N개의 보드판이 있으며 A[N-1] 배열로 되어있으며 각 판에는 숫자가 맵핑되어있고 주사위를 던져서 N-1까지 움직여서 도착하면 게임이 끝남

## 접근방법
- 받은 A[n-1] 배열의 길이만큼 result[n-1] 값을 받을 배열을 선언해줌
- 첫번째 값은 A[0] = result[0] 는 같음
- 두번째도 선택의 여지가 없으므로 첫번째랑 자기자신을 더한값 result[1] = A[0]+A[1]
- 세번째 부터 고를수 있음 앞에 숫자들 중에서 큰값인것을 고른다 자기 자신+ 큰값
-  ex) result[2] = A[2]+Max(A[0],A[1])
-  주사위 숫자가 6이므로 고려해줘야함
-  결국 result[n-1] 값이 이 문제에 최대값임 

|   |  0 | 1  | 2  | 3  |  4 |  5 |
|---|---|---|---|---|---|---|
| A[]| 1 |-2 | 0|9|-1 |-2|
|result[]| 1 | -1|1| 10 | 9|8|


## 풀이방법

~~~java
class Solution{
    public int solution(int[] A){
        // 받은 A[N-1]길이만큼 결과값을 받을 배열을 선언해준다
        int [] result = new int[A.length];
        
        result[0]=A[0];
        for(int i=1; i<A.length; i++){
            result[i] =result[i-1]; //ex)i=2 이면 result[2] = result[1]
            for(int minus =2; minus<=6; minus++){
                if(i>=minus){  
                    result[i]= Math.max(result[i],result[i-minus]);
                    // ex)i =2 result[2] = max(result[1],result[0]);
                    //result[2] = 큰값은 여기서 result[0]들어가짐
                }else{
                    break;
                }
            }
            result[i] +=A[i];// result[2]=result[0]+A[2]
        }
        //결국 result[n-1] 값이 이문제의 최대값
        return result[A.length-1];
    }
}


~~~