## 문제풀이
- Arrays.copyOfRange(array[], first, last) 를 사용하면 for 문을 두번 돌지 않고 풀 수 있음

~~~ java
package programmers.alignment_1;

import java.util.Arrays;

class Solution {
    public int[] solution(int[] array, int[][] commands) {
        int[] answer = new int[commands.length];

        for (int i = 0; i < commands.length; i++) {
            int[] command = commands[i];
            int first = command[0] - 1;
            int last = command[1];
            int index = command[2] - 1;

            int[] cut = Arrays.copyOfRange(array, first, last);
            Arrays.sort(cut);
            answer[i] = cut[index];
        }

        return answer;
    }

    public static void main(String[] args) {
        Solution solution = new Solution();
        int[] array = {1, 5, 2, 6, 3, 7, 4};
        int[][] commands = {{1, 7, 3}, {2, 5, 3}, {4, 4, 1}};
        int[] answer = solution.solution(array, commands);

        for (int i = 0; i < answer.length; i++) {
            System.out.println(answer[i]);
        }

    }
}
~~~

## Arrays.copyOfRange() 메소드
- 첫 번째 매개 변수로 복사의 대상을 원본배열에 넣어줌
- 두 번째 매개변수로는 원본 배열에서 복사할 시작 인덱스를 전달 받고
- 세 번째 매개변수로는 마지막으로 복사될 배열 요소의 바로 다음 인덱스를 전달받음
  - 3을 넣으면 2까지만 복사됨!!!!!!
  
~~~java
int[] arr1 = {1, 2, 3, 4, 5};

int[] arr2 = Arrays.copyOfRange(arr1, 2, 4);

for (int i = 0; i < arr2.length; i++) {
    System.out.print(arr2[i] + " ");

}
~~~

## 참고사이트 
- [Arrays 클래스](http://tcpschool.com/java/java_api_arrays)
