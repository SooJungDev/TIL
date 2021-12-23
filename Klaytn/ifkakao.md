# if(kakao) 2021 듣고 정리

## klaytn 을 이용하여 NFT 활용하기: 발행부터 판매까지
- 클레이튼 : 블록생성 1초 
- 매 블록당 4000 트랜잭션
- 완결성이 보장되는 BFT 컨센서스
- EVM & 솔리디티 언어 지원
- 저렴한 트랜잭션 수수료
- 독자적인 계정 & 트랜잭션 모델

### 클레이튼 토큰 표준
- KIP-7, 17, 37
- 토큰표준은 왜필요?
- 각계정 사용자가 관리하는 계정, Contract 계정일수도 있음
- ex) KIP-17 컨트랙트 사용자, Contract 계정 쉽게 스마트 컨트랙트 호출할 수 있음


### KIP-17 인터페이스
NFT(Non-Fungible Token)
- 토큰 발행
- 토큰 개수조회
- 토큰의 소유자 조회
- 전송
- 전송권한 승인

### NFT 사용자 시나리오

발행 -> 전송 -> 판매 -> 전시

발행 -> 전송 : KIP-17

판매: 판매자/ 구매자 원하는 가격으로 매매할수 있는 방법 필요
별도의 추가 컨트랙트 개발해야함, 추가적인 서비스 레이어에 개발필요

전시: NFT 정보를 효율적으로 검색, 나의 NFT를 효과적으로 전시

### NFT 마켓플레이스 요구사항
- 추급권(Resale Royalty Right) : 작가, IP 소유권등
- 판매방식: 고정가, 경매
- 결제 수단: KIP-7 , KLAY
- 환불 : 구매후 일주일간 환불 기능 제공

### 추급권 (Resale Royalty Right)
- 대부분 NFT 마켓플레이스에서 제공하는 기능
- 하지만 표준화되지 않아, 서로 호환되지 않음
  - A 거래소에서 추급권을 10%로 설정
  - B 거래소에서 해당 NFT를 판매할 경우 추급권이 동작하지 않음
- 이더리움 2021월 7월 25일 ERC-2981을 통해 Royalty에 대한 표준화 정리
  - 하지만 마켓플레이스까지 동작하는 전체 사용자 시나리오가 완벽하지 않아보임
- 클레이튼에서도 해당 표준에 대한 정립필요
  - 마켓 플레이스까지 동작하는 구조를 만들고 표준을 제시 
  
### ERC-2981의 아쉬운점
- 최소한의 요구사항만 인터페이스로 정의
  - 둘 이상의 추급권자가 있을 경우?
  - 추급권이 적용되지 않았던 NFT는?
- NFT 마켓플레이스에서 현제 제공하는 기능들도 커버 할 수 없음

### 이 발표에서는...
- NFT 마켓 플레이스의 관점에서 필요한 기능들을 설계/구현
- 다수 추급권자 허용
- 추급권을 처리하는 gateway 컨트랙트 설계
- NFT 마켓플레이스에서 추급권까지 동작하는 구조설계
- 해당 기능들을 공개하고 클레이튼 표준(KIP)으로 제안하려함

### 추급권 조회
- 추급권이 적용되어 있는 NFT, 없는 NFT 처리하기 위해서는 gateway 컨트랙트가 필요함
- RoyaltyRouter : Gateway Contract 모든 추급권을 조회할 수 있는 컨트랙트, 추급권 조회를 위해서는 이 컨트랙트만 알면 됨
- RoyaltyInfo : 각 토큰별로 로열티를 세팅할수 있도록 함, KIP-17 컨트랙트 1:1로 매칭됨

### NFT 마켓플레이스 설계/구현
- FixedPriceExchange 고정 값 판매를 위해 사용
- 판매자가 어떤 토큰으로 구매하기를 원하냐에 따라 호출하는 함수가 달라짐
- Minter : 작가
- 작가를 통해 새로운 NFT 발급됨
- 판매자가 판매 등록을 하기전에 해당 토큰에 전송 권한을 FixedPriceExchange 에게 부여해야 한다
- 승인 절차가 선행되어야 함 approve(exch, tokenID)
- putOnSale(KIP-17. tokenId, KIP-7, salePrice)
- 먼저 구매자가 KIP-7 으로 transferForm(Seller, Buyer, tokenId) 를 호출한다
- 이후 Exchange 컨트랙트는  RoyaltyRouter 를 통해 추급권에 대한 정보 얻어온다 
- 이정보를 바탕으로 정상 정보를 Escrow 컨트랙트에 전달한다. 판매 대금도 같이 전달
- Exchange 컨트랙트는 transfer 함수를 이용하여 구매자에게 전송한다.
- 실제 NFT 는 구매자에게 전달됨
- 일정 시간이 흐른후 판매자가 closeEscrow() 호출하면 판매대금이 판매자에게 전달

### 더 고민해야 되는 부분들
- NFT의 매매 활성화를 위해 필요한 추가 기능에 대한 조사/연구
  - 추급권의 유효기간? 금액한도? 금액별 요율?
  - NFT 마켓플레이스 컨트랙트는 공유하되, 웹서비스가 여러개로 분화한다면?
  - 법률검토(가상자산, 저작권 등 기타 디지털 자산권리)
- NFT 전시를 위해서는 어떤 기능들이 필요한지 조사/연구
  - NFT 소유자들이 원하는 전시형태
  - 전시를 하귀 위해 필요한 추가적인 기능? 자료구조?

발표 링크
- [Klaytn을 이용하여 NFT 활용하기: 발행부터 판매까지](https://if.kakao.com/session/95)

## Klaytn Blockchain API의 수평적 확장
- Blockchain Service <-----> Klaytn Node
-  Blockchain Service <----->  Klaytn API Service <----->  Klaytn Node

### 데이터베이스 복제의 어려움
- 4.9 TB Storage 복제해야함..
개선 아이디어
- Node에서 데이터베이스를 분리
- 데이터베이스를 여러 Node들이 공유

요구사항
- 작은 데이터를 빠르게 Read/Write 가능
- 평균 크기:400Bytes
- Wirte 성능: 10000 RPS 이상
- Read 성능: 50000 RPS 이상
- 균일하고 낮은 Latency

균일한 Latency의 중요성 -> 안정적인 서비스가 어려움
- Klaytn의 기본데이터베이스(LevelDB)
- 구조적 특성으로 Read 성능 편차가 큼
- 블록체인 은 데이터를 연속 읽는 경우가 많음   


### 최신 블록 동기화 어려움
- 개선방안
1. Node 역할 분리 (Fetcher/ Service Node)
2. Service Node에 확장성 부여  

### 스트리밍 데이터 처리
- 짧은 간격으로 pull or push 를 해야함
- Redis pub/sub 방식을 택함
- Fetcher publish 하면 Service Node Subscribe 방식

### 개선방안 요악
문제점
- 데이터베이스 복제의 어려움 : 데이터베이스 분리와 공유
- 최신 블록 동기화의 어려움 : Node 역할 분리 & 확장성부여
- 스트리밍 데이터 처리 : Pub/Sub을 이용한 스트리밍 데이터 전파

### 향후발전 사항
- Service Node 사이의 최신 블록번호 동기화 수준 강화
  - Redis Pub/Sub 방식에서 동기화 시간 오차 < 10ms
- Transaction Pool 데이터 동기화
  - Pending Transaction 으로 인한 동기화 문제 발생
  - 현재 일부 API에 대해서는 Static Routing 적용
- 데이터베이스 비용절가
  - DynamoDB는 요청수에 비례하여 과금
  - 작은 크기의 데이터를 빈번하게 요청하여 비용과다
  
발표 링크
- [Klaytn Blockchain API의 수평적 확장](https://if.kakao.com/session/98)

## 한줄로 구현하는 블록체인 어플리케이션

### Blockchain Overview
- 블록체인 2.0 이더리움 등장, 이더리움 가치 전송 + 스마트 컨트랙트 코드블럭 블럭체인 위에 올림
- DeFi, NFT

### klip & sdk
- Klaytn 지갑 까다로운 키 관리없이 자산을 안전하게 보관
- klip API 제공 , 맵핑에서 편안하게 사용 할 수 있도록 sdk 제공

### 후원하기 앱 만들기

발표 링크
- [한줄로 구현하는 블록체인 어플리케이션](https://if.kakao.com/session/90)

## 고성능 블록체인 지갑 서비스 Klip 개발기
- 지갑의 키를 서버에서 관리함
- 출시 초기 파리 날려도 버틸려면
  - 자동확장 가능한 인프라 설정은 최소화
  - EC2는 전사 c5.2xlarge 18개중 2개의 pod을 사용 (2만 RPS 까지 pod 늘지 않음)
  - Aurora, Elasticache 넉넉하게 자동으로 늘어나지 않음
- 갑자기 부하 늘어나면?
  - pod 늘어남, 공유데이터
- 성능에 가장 크리티컬한 부분은 캐시임, 가능하면 많은 곳에 Redis 반영 권장


발표 링크
- [고성능 블록체인 지갑 서비스 Klip 개발기](https://if.kakao.com/session/90)

## Klip Drops Smart Contract - 수억 원 작품을 거래하는 플랫폼을 블록체인 컨트랙트로 간단하게 구현해보자
### Klip Drops
- 한정판 디지털 아트 판매
- 판매 방법 두개 옥션과 에디션
- 참여하기 위해서는 클레이 확보해야함

- 개발 요구사항 - Contract 로 해결

### 블록체인 & 컨트랙트로 해결하기
블록체인 특성
- User -> 요청(트랜잭션) -> EndpointNode -> TransactionPool -> 블록체인 기록

- 실시간 입찰
  - 실시간으로 최고 응찰가가 보여져야됨
  - 실시간으로 환불
  - 이러한 입찰과정 컨트랙트로 되도록함
  - 입찰자 및 입찰가 저장
  - 신규 입찰가가 더 높은 경우: 입찰자 및 입찰가 저장 기존 입찰 금액 반환
  - 신규 입찰가가 같거나 낮은 경우
  - 신규 입찰 금액 반환
  
- 응찰내역
  - KAS 사용
  - EOA 로 토큰 송/수신 기록 조회
  - 전송이력 한번에 받을 수 있음
  
- 허가된 사용자
  - 옥션의 경우 Klip Drops 사용자만 되게끔
  - Signing KlipDrops signing 트랜잭션이 있는지
  - 트랜잭션 만들때 수수료 지급해야함, 사용자가 클레이가 없어도 서비스가 트랜잭션 비용을 내줌
  - 트랜잭션을 전달받은 컨트랙트에서는 클립 드롭스 백엔드 계정에서 관리하는 동일한 계정만 되도록 거래하도록 함
  
  
발표 링크
- [Klip Drops Smart Contract - 수억 원 작품을 거래하는 플랫폼을 블록체인 컨트랙트로 간단하게 구현해보자](https://if.kakao.com/session/92)

## 블록체인도 영수증이 있다니! Receipt로 TX 결과를 확인해 보자
### Receipt 이란?
- 거래에 대한 증명서
- 언제 어디서 무엇을 얼마에 구입했는지 정보가 나타남
 
Blockchain Transaction
- 유저간 송금
- Smart Contract 실행
- 각각 고유한 hash 값을 가짐
- 블록 반영 후 Receipt 확인 가능

### TX Receipt
- Receipt 얻는법
  - Klaytn EN or KAS 를 통해 klay_getTransactionReceipt rpc call

### Blockchain Receipt 의 정보
- 누가 : From To
- 언제 : BlockNumber, BlockHash
- 금액: value
- TxType : 어떻게
- 결과: Status , Logs


### Klip Receipt 을 확인하는 방법
Klip : 카카오톡과 연결된 블록체인 지갑 서비스

Klip TX 생성과정
유저 -> Tx 전송요청 -> Klip -> TX 생성후 전달 -> Klaytn 블록체인 
- TX가 블록에 반영되려면 약 1-2초 소요
- 비동기 처리가 필요

이를 해결하기 위해 Receipt Checker 구현
- Message Queue, Works 
- Receipt 과 관련된 비동기 처리를 수행함

### Contract Event 확인 방법
- Contract 에서 발생한 event 는 logs 배열에 표시
- Topic의 0번 index 값이 event signature
- 그 외에 topic 은 indexed 된 Event 매개변수

### Event Signature 구하는 방법
event Transfer(address indexedfrom, address indexedto, unit256 value);
- 이벤트 이름, 매개변수 type 만남기고 삭제
- Trasnfer(address,address,unit256)
- Keccak256 hash 사용 

### 다른 환경 BE 가 같은 Message Queue 를 사용
- Prod, Stage 에 같은 매세지큐 사용해서 마치 유실되는것처럼 보였음
스테이지 워커에서 실행될줄 알았는데  prod worker 에서 수행해서 마치 유실되는것처럼 보임
- 다른 환경에서 메세지큐가 공유되서는 안됨
- 메세지큐 변경하고 사라졌음

### Graceful shutdown
- 그냥 종료시 유실됨
- 종료시그널시 worker 가 현재의 작업까지만 마무리하도록 변경


### 현구조의 문제점
- API 요청이 몰렸을경우 Workers 와 함께 증가됨
- API, Worker 분리 따로 따로 증가되도록 변경할 예정


발표 링크
- [블록체인도 영수증이 있다니! Receipt로 TX 결과를 확인해 보자](https://if.kakao.com/session/91)

## Klaytn 블록체인 데이터 추출하기
- Block : 블록에 기록되는 데이터
- Trace : 트랜잭션 실행과정에서 얻는 데이터
- 블록내 트랜잭션 만으로는 컨트랙트에서 전송내역을 전부다 확인할 수 없음
- 한 계좌에 클레이 전송내역을 추적하기 위해서는 블록내 트랜잭션에서만은 확인할 수는 없고 트랜잭션을 수행하면서 만들어진 데이터를 수집해야함

- 내부적으로 발생하는 클레이 전송은 trace 데이터에 속함 
- Alice 가 Bob 에게 lKLAY 전송
- 외부에서는 발생할수 없고 컨트랙트를 실행하면서 발생한 트랜잭션을 인터널 트랜잭션이라고 부른다


### 초기 구현 방식
API를 통한 데이터 추출
- transaction 실행 직전 State 계산하기
- transaction 실행하기
- internal transaction 데이터 추출하기
- 수신자가 contract 일 경우 contract code 실행
- Internal transaction : Contract -> bob 1kay 전송

### 불필요한 State 계산
State 계산하기
- transaction 실행 직전 State 계산하기
- transaction 실행하기
- internal transaction 데이터 추출하

실시간 데이터 추출 방식
기존 (비효율)
1. 과거 블록에 대한 Trace 데이터 요청
2. Node 에서 트랜잭션 실행
3. 과거 블록에 대한 Trace 데이터 반환
4. KAS

개선
1. Node 새로운 블록 처리
2. 데이터 Produce
3. Message Queue(kafka) 지속적으로 들어오는 데이터 Consume
4. KAS

### 데이터 크기에 따른 성능저하
데이터 분절 및 조합
- kafka는 크기가 작은 메세지를 많이 처리하는데 효과적임
- kafka에서는 메세지 크기가 클수록 처리량이 급격히 줄어듬
- 대략적인 데이터 최대 데이터크기:128MB = 32KB*4000TPS(tx 최대크기 * 네트워크 최대성능)

기존
1. Node 블록처리
2. 데이터 Produce
3. Message Queue(Kafaka) 데이터 Consume
4. Kas

개선
1. Node 블록처리
2. 데이터 분절 (헤더를 만듬: Segment 번호, 총 Segment 개수 데이터조각)
3. 데이터 Produce
4. Message Queue(Kafaka) 데이터 Consume
5. 데이터 조합
6. Kas

데이터 분절 및 조합의 장단점
- 장점
  - 메세지 크기로 인해 Kafka의 성능저하를 막음
  - 큰 데이터를 처리하기 위해 kafka cluster 이외에 다른 리소스가 필요하지 않음
- 단점
  - 메시지 조립 구현의 복잡성이 높음
  - Consumer Buffer 메모리 사용량 증가

### klaytn 의 Tracer란
- KLVM Contract code 실행 Tracer 주입
- 일련의 operation 실행
- 매 operation 마다 VM의 상태 기록 

Go-native Tracer
- Javascript Tracer JSVM 추가적으로 생기는 동작에 오버헤드 굉장히 컸음
- Javascript Tracer와 데이터 추출하는 방식은 같으나 JSVM 사용하지 않아 훨씬 빠르다.
- 기존 대비 8배 성능 향상


발표 링크
- [Klaytn 블록체인 데이터 추출하기](https://if.kakao.com/session/133)

## 블록체인 비호환변경 a.k.a 하드 포크
### 블록체인 비호환변경, 하드포크
트랜잭션의 실행과 블록 생성
- 블록체인이란 일련의 블록이 연결된 구조를 말하며, 각 블록은 트랜잭션 묶음과 그 실행 결과를 담고 있음
- Receipt: 블록을 생성한 노드가 트랜잭션을 실행한 결과
- 블록 생성 노들르 선출: 매 블록마다, 네트워크 노드간의 합의를 통해 이루어짐

1. 트랜잭션이 블록체인 노드로 들어옴
2. 노드는 실행한 트랜잭션들과 그 결과를 receipt 에 담음
3. 블록 생성
4. 네트워크에 전파

블록의 전파와 검증
- 블록이 전파되면, 각 노드는 자신이 가지고 있는 현재 잔고 정볼르 가지고 트랜잭션을 실행
- 실행결과와 블록의 receipt hash 가 일치하면 체인에 새로운 블록으로 추가 -> 검증과정
  - Receipt hash: 트랜잭션 receipt trie 의 root hash

만약 어느 시점부터 서로 다른블록이 생성되고 이를 받아들인다면? 포크
 - 두그룹은 서로 호환 X

호환되지 않는 변경, 비호환 변경
- 그룹1: 수수료가 10인 코드버젼 사용 -> 때문에 수수료가 10인 블록만 생성하고, 받아 들일수 있음
- 그룹2: 수수료가 20인 코드버젼 사용 -> 때문에 수수료가 20인 블록만 생성하고, 받아 들일수 있음
- 수수료가 10인 코드버젼: old version, 수수료가 20인 코드버젼: new version
- new version을 비호환변경이라고 하고, 하드포크라고도 부름

### 하드포크를 하는이유
- 일반적인 변경에서 하지 못하는 변경이 가능
- 예) 가스 가격 변경, 합의 엔진 수정등
- 이를 통해 블록체인 발전 도모

- 런던 하드포크 eip 1559
  - 수수료 가격인 gasPrice 개념이 삭제되고, 네트워크 혼잡도에 따라 결정되는 BaseFee 로 대체
  - 생태계에 시사하는 바 -> 변동성 감소
  - 블록체인 발전 -> 수수료 체계의 안정화

하드포크 작성은 어떻게 하는것일까? 활성화 블록이 통일되도록
- 하드포크 활성화 시점이 통일 될 수 있도록 특정 블록에서 하드포크를 활성화하겠다고 네트워크에서 약속
- 이때의 블록번호를 하드포크 블록번호라고함
- 새로운 버젼의 코드에서 하드포크 이전에는 legacy code 도 사용 할 수 있도록 코드를 남겨

하드포크 블록번호 어떻게 저장? - public network 방식
- 블록번호를 임의로 변경 할 수 없도록 강제 할 수 있음
- 신규코드 업데이트르 통해 자동으로 저장되는 방식
- param/config.go 에 네트워크 별로 정의

- private network 방식
- 제네시스 블록 및 네트워크 설정파일인 genesis.json 을 활용하여 노드가 직접 저장하는 방식
- step1. 아래와 같이 genesis.json 에 하드포크 블록번호를 직접추가
- step2. ken init--datadir data genesis.json

### klaytn 비호환변경
- Ethereum 에서 포크한 프로젝트
- Governance, feePayer, service-chain, multiSig 등 독자적인 블록체인 플랫폼 기술 또한 구축
- 블록 생성 노드 선출 방식인 합의 알고리즘은 Pow 가 아닌 BFT합의 방식을 채택하여 사용중(IBFT)

왜 klaytn은 비호환변경인가요? 변경을 통해 포그가 날수 없는 구죠
- PoW 합의 방식(예, Ethereum)
  1. 블록생성
  2. 해시퍼즐 (해시퍼즐만 풀면 유효 블록으로 인정됨 -> 여러 유효블록이 존재가능)
  3. 유효블록들 중 버전에 따라 호환가능한 블록만 수용
  
- BFT 합의방식(예, Klaytn) 
  1. 블록생성
  2. BFT 합의 (66프로 이상이 동의하는 블록 선정 -> 단 하나의 블록마이 존재가능)
  3. 합의를 통해 선정된 블록만 수용

Opcode 변경점 - opcode 란
- Opcode 란 머신(컴퓨터,가상머신)에게 내리는 명령어 중 최소단위로서 operation code라고 함
- Klaytn 등의 블록체인에서는 지원하는 opcode set이 미리 코드에 정의되어 있음
- 아래 과정은 add opcode를 예로 들어 한 opcode 가 실행되기까지의 과정을 나타냄
- 비호환변경을 통해 새로운 opcode 추가 혹은 동작/가스 변경가능


Precompied contract 변경점 - precompiled contract 란
- Precompiled contract란
  - 높은 연산성능을 필요로 하는 해시 함수들을 특정 컨트랙트 주소에 미리 정의
  - 해당 컨트랙트 주소를 호출하면, 자동으로 그와 매칭되는 해시 함수가 호출됨
- opcode 와 마찬가지로 비호환변경을 통해 새로운 컨트랙트 추가 혹은 동작/가스 변경가능
- 비호환변경 활성화 이전에 배포된 컨트랙트는 old address를 따르도록함

컨트랙트가 배포된 시점을 알수 있는 방법이 없을까?
- 문제: 컨트랙트 어카운트의 배포된 시점을 알수 있는 정보 X
- 해결: 스마트 컨트랙트 어카운트 필드에 배포된 비호환변경 시점을 나타내는 필드 vmVersion을 배포시에 저장
- vmVersion이 OxO인 컨트랙트는 old precompiled contract list 를 사용하도록 함

향후목표..
- 독자성
- 호환성

발표 링크
- [블록체인 비호환변경 a.k.a 하드 포크](https://if.kakao.com/session/96)

## Nonce 그게뭔데?
- Transaction : 사용자의 요청 송금을 한다거나 블록체인 위에서 동작하는 스마트컨트랙트에서 사용되는것
- 논스는? 대기표 작업을 처리하는 순서를 정해줌

### 논스는 왜 필요할까?
- 논스가 없다면 어떻게 될까?
  - 어떤 노드에서 블럭을 생성할줄 모름
  - 노드는 TX 가 반영됬는지 아닌지 알수 없음
  - 논스는 순서를 정해주고 이미 처리된 트랜잭션인지 아닌지 알려줌
  
### 논스법칙
- 1,2,3 순서대로 해야됨 1,2,4 면 4번은 영원히 처리 X
- unique rule 1,2,2,3 논스가 중복 둘중에 한 논스는 처리 X

### 어떻게 구현?
- 쉬운방법
- 논스 묻기
- TX 보내기
- 반영 확인, 트랜잭션이 반영되지 않은 상태에서 논스를 하면 유니크 룰에 위배

좀더 빠른방법
- TX를 읽고 전송
- 다른 요청을 처리하기위해 논스를 업데이트 다시 반복 TX를 읽고 전송


스케일아웃
- LB : 다른 서버에서 각각 논스를 가지고 있으면 유니크 룰에 위반됨
- 공용 저장소가 필요함!!!
- 해당 공용저장소에 논스를 저장하고 읽고 전송 룰위반하지 않는다

데이터 레이스
- 공용저장소로 부터 논스를 읽게되면 유니크룰 위배
- TX가 실패함 
- Redis INCR 명령
~~~
INCR key
~~~
- 레디스는 순차적으로 키를 처리해서 각서버에게 다른 값을 줌

TX 처리에 실패한다면?
네트워크 유실로 실패한다면
1 실패
2 를 처리 도미노룰에 위반

실패한 논스를 재활용함
- 우선순위 큐 를 추가함
- 만약 1번 nonce 실패하면 우선순위 큐에 추가함
- 우선순위에 있는 1번 논스값을 사용
- 우선순위 큐가 비어있으면 레디스에서 INCR 명령

우선순위 큐 쉽게 구현
- Redis Sorted Sets 사용
- {Score, Value} => {Nonce, Nonce}

### 더쉬운 방법
- Klip 에 위임 app to app 

발표 링크
- [Nonce 그게뭔데?](https://if.kakao.com/session/45)
