## Huffma coding
- 무손실 압축(lossless)

## Prefix code
- 어떤 codeword도 다른 codeword의 prefix가 되지 않는 코드(여기서 codeword란 하나의 문자에 부여된 이진코드를 말함)
- 모호함이 없이 decode가 가능함
- prefix code는 하나의 이진트리로 표현이 가능함
  
 ## Run-Length Encoding
- 런(run)은 동일한 문자가 하나 혹은 그이상 연속해서 나오는것을 의미한다. 예를 들어 스트링 s="aaabba"는 다음과 같은 3개의 run으로 구성된다 "aaa","bb",a
- run-length encoding 에서는 각각의 run을 그 "run을 구성하는 문자"와 "run의 길이"의 순서쌍(n,ch)로 encoding 한다. 여기서 ch가 문자이고 n은 길이이다. 가령 위의 문자열 s는 다음과같이 코딩된다: 3a2b1a.
- Run-length encoding은 길이가 긴 run들이 많을 경우에 효과적이다 ex)이미지 압축

## Huffman method with Run-Length Encoding
- 파일을 구성하는 각각의 run들을 하나의 super-symbol로 본다. 이 super-symbol들에 대해서 huffma coding을 적용한다.
- 예를들어 문자열 AAABAACCAABA는 5개의 super-symbol들 AAA,B AA, CC,A로 구성되며 각 super-symbol의 등장 횟수는 다음과같음 
  
| symbol  | A  | C  | A  | B  | A  |
|---|---|---|---|---|---|
| run length  | 3  | 2  | 1  | 1  | 2  |
|  frequency |  1 | 1  | 1  |  2 |  2 |

## 1단계 Run과 frequency찾기
- 압축할 파일을 처음부터 끝까지 읽어서 파일을 구성하는 run들과 각 run들의 등장횟수를 구한다.
- 먼저 각 run들을 표현할 하나의 클래스 class Run을 정의한다. 클래스 run은 적어도 세개의 데이터 멤버 
  symbol,runLen, freq를 가져야 한다. 여기서 symbol은 byte 타입이고, 나머지는 정수들이다.
- 인식한 run들은 하나의 ArrayList에 저장한다.
- 적절한 생성자와 equals 메서드를 구현한다.
  
  ~~~java
  class Run {
      public byte symbol;
      public int runLen;
      public int freq;

      /*적절한 생성자와 equal 메소드 필요 비교할때는 symbol, runLen만 비교*/
  }
  ~~~
 - 데이터 파일을 적어도 두 번 읽어야 한다. 한번은 run들을 찾기 위해서, 그리고 다음은 실제로 압축을 수행하기 위해서.
 - 여기서는 RandomAccessFile을 이용하여 데이터 파일을 읽어본다
 ~~~java
 /*읽을 데이터 파일을 연다*/
 RandomAccessFile fIn=new RandomAccessFile(fileName,"r");

 /* 한 byte를 읽어온다. 읽어온 byte믐 0~255사이의 정수로 반환된다 */
 /* 파일의 끝에 도달하면 -1을 반환한다.*/
 int ch=fIn.read();

 /* 필요하다면 byte로 casting해서 저장한다*/
 byte symbol=(byte)ch;
 ~~~

 ## Run 인식하기
 1. 파일의 첫 byte를 읽고 이것을 start_symbol이라고 한다.
 2. 파일의 끝에 도달하거나 혹은 start_symbol와 다른 byte가 나올 때 까지 연속해서 읽는다. 현재까지 읽은 byte수를 count
 3. (start_symbol, count-1)인 run이 하나 인식되었다. 이 run을 저장하고 가장 마지막에 읽은 byte를 start_symbol로
   count=1로 reset하고 다시 반복한다.

## Run과 frequency찾기

~~~java
 public class HuffmanCoding{
     /*인식한 run들을 지정할 ArrayList를 만든다.*/
     private ArrayList<Run> runs =new ArrayList<Run>();

     privae void CollectRuns(RandomAccessFile fIn) throws IOException{
         /*데이터 파일 fIn에 등장하는 모든 run들과 각각의 등장횟수를 count하여
          ArrayList에 저장한다.*/
     }

     static public void main(String arg[]){
         HuffmanCoding app=new HuffmaCoding();
         RandomAccessFile fin;
         try{
             fIn -new RandomAccessFile('sample.txt',"r");
             app.collecionRun(fIn);
             fIn.close();

         }catch(IOException io){
             System.err.println("cannot open"+fileName);
         }
     }
 }


~~~