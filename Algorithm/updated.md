## 정렬

정렬 알고리즘
- Bubble sort
- Insertion sort
- Selection sort
    - 단순하고 이해하기 쉽지만 느림
   
- Quicksort
- Merge sort
- Heap sort
    - 빠름

- Radix sort

## 선택 정렬 (Selection sort)
- 각 루프마다
    - 최대 원소를 찾는다.
    - 최대 원소와 맨 오른쪽 원소를 교환한다.
    - 맨 오른쪽 원소를 제외한다
- 하나의 원소만 남을 때 까지 위의 루프를 반복
- 시간 복잡도: O(n^2) 최악,최선 동일


## 버블 정렬 (Bubble sort)
~~~
bubblesort(A[],n){ // 배열 A[1...n]을 정렬한다
{
    for last <- n down to{
      for i <-1 to last -1
        if(A[i]> A[i+1])
          then A[i]<-> A[i+1]; // 교환
        
    }
}
~~~
- 실행시간 (n-1)+(n-2)+...+2+1=O(n^2), 최악 최선 동일

## 삽입정렬(Insertion sort)
~~~
insertionSort(A[],n) 
{
    for i<-2 to n {
        A[1...i]의 적당한 자리에 A[i]를 삽입한다.
    }
}

~~~
- 최악의 경우 O(n^2)
- 최선의 경우(다 정렬되어있을 경우) n-1번 O(n)
- 뒤에서 부터 검사하면 더빠르게 검사 가능 

## 합병정렬(Merge sort)
- 분할 정복법을 사용함
    - 분할: 해결하고자 하는 문제를 작은 크기의 동일한 문제들로 분할
    - 정복: 각각의 작은 문제를 순환적으로 해결
    - 합병: 작은 문제의 해를 합하여(merge) 원래 문제에 대한 해를 구함
