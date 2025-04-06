# Solidity

## Solidity란
![Solidity 로고](/자료/Images/Solidity.png)

이더리움 블록체인에서 스마트컨트랙트를 제작하기 위한 언어
## 스마트 컨트랙트란 
블록체인에서 특정 조건이 충족되면 자동으로 작동되는 프로그램
## Solidity를 배우기전에 알아야 할 내용
<b>Gas</b> <br>
스마트컨트랙트를 배포 / 사용 할때 지불하는 비용

<b>Gwei, wei</b> <br>
가스비를 낼때 사용되는 단위 <br>
1 ether = 10^9 Gwei = 10^18 wei

가스는 스마트컨트랙트에서 어떤 행위를 하느냐에 따라 부과되는 가스비가 다르다 자세한 내용은 이더리움 옐로우 페이퍼를 보면 나와있다 <br>
https://ethereum.github.io/yellowpaper/paper.pdf 

가스는 해커들의 DDOS 공격을방지 할수도 있다 => 가스를 발생시킴으로 블록체인 네트워크의 과부화를 방지 (가스 발생으로 인한 비용 부담)

### Gwei와 wei가 있는데 Gas는 또 뭔가?
이더리움에서 Gwei와 wei는 이더(ETH)의 단위이지만, Gas는 연산량을 측정하는 개념이다 즉, 이더는 화폐 자체이며 시장 가격이 존재하지만, 트랜잭션 비용을 직접 ETH 단위로 책정하면 혼란을 야기할 수 있다. 이를 해결하기 위해 Gas라는 별도의 단위를 도입했다 비유를 하자면
<pre>
gas = 자동차의 연료 소비량 (ex: 10L)

gas price = 연료 1L당 가격 (ex: 1,500원/L)

최종 연료비 = 연료 소비량 * 연료 가격 (ex: 10L * 1,500원 = 15,000원)
</pre>

이렇게 비교해볼 수 있다. 즉, Gas는 연산량을 나타내는 단위이고, Gas Price는 그 연산량을 ETH로 변환하는 비용 단위라고 이해할 수 있다.

### Gas Price(가스 가격)는 누가 정하는가?
<b>사용자(트랜잭션 보내는 사람)</b>

* 트랜잭션을 전송할 때 "나는 1 Gas당 20 Gwei를 지불하겠다!"라고 설정할 수 있음.

* 더 높은 Gas Price를 설정하면 더 빨리 처리될 가능성이 큼.

* 너무 낮게 설정하면 네트워크에서 무시될 수도 있음.

<b>네트워크 상태(혼잡도)</b>

* 사용자가 많고 혼잡하면 Gas Price가 올라감.

* 반대로 한산하면 Gas Price가 낮아짐.

* 마치 택시 요금이 교통 체증에 따라 변하는 것과 비슷함.

<b>채굴자(또는검증자)</b>

* 트랜잭션을 처리하는 사람(검증자)이 높은 Gas Price를 제시한 트랜잭션을 먼저 선택함.

즉, Gas Price가 높을수록 우선 처리될 가능성이 커짐.

### GasLimit과 BlockGasLimit
<b>Gas Limit</b>

<b>BlockGasLimit</b>

## Solidity 문법
### 라이센스 선언
솔리디티는 코드 맨 윗줄에 라이샌스를 명시해줘야지만 오류가 나지 않는다.
<pre>
// SPDX-License-Identifier : GPL-30
</pre>
라이센스의종류는 위 라이센스를 제외하고 다양하다

### 버전 선언
솔리디티는 컴파일러에게 몇 버전의 컴파일러를 사용하여 컴파일 할건지 알려줘야하기 떄문에 pragma를 이용하여 컴파일 버전을 명시해줘야한다.
<pre>
pragma solidity >= 0.7.0 < 0.9.0; // 0.7.0 ~ 0.9.0 버전을 사용
pragma solidity ^0.8.0; // 0.8.0 이상의 버전을 사용용
</pre>
버전은 어떤 버전이던 사용해도 상관없지만 0.8.0 버전을 사용하는것이 현재 기준으로는 가장안정적이다

### 컨트랙트 선언하기
컨트랙트는 컨트랙트에서 실행될 코드가 담겨있는곳으로 객체지향의 클래스와 유사하다
<pre>
// SPDX-License-Identifier : GPL-30
pragma solidity ^0.8.0;

contract Hello {

}
</pre>

### 변수
Solidity는 data type, reference type, mapping type으로 3가지 타입으로 나뉜다
### data type
data type에는 bool, bytes, address, uint 타입이 존재한다 <br> <br>
<b>bool</b> <br> true false를 정의 <br> <br>
<b>bytes</b> <br>bytes 형식으로 값을 저장할때 사용된다 <br> <br> 
<b>address</b> <br> 이더리움의 지갑주소를 저장할때 사용됨 <br> <br>
<b>int, uint</b> <br>정수를 저장할때 사용되며 뒤에 8, 16과 같은 숫자를 붙여 변수의 크기를 지정할수 있다 <br> (uint는 unsigned integer의 약자이며 양의 점수만 저장한다) <br> <br> 보통은 음의정수는 사용할 일이 없기 때문에 uint를
많이 사용한다
<pre>
bool b1 = !false; // true
bool b2 = false || true // true
bool b3 = false == true; // false
bool b4 = false && true; // false

// bytes 1 ~ 32
bytes4 bt = 0x12345678;
bytes bt2 = "string"; // 0x535452494e47

//40자리의 이더리움 주소
address addr = 0x5B38Da6a701c568545dCfcB03FcB875f56beddC4;

uint number = 3;
uint8 number = 3;
</pre>
### reference type
<pre>
</pre>
### mapping type
<pre>
</pre>
### 조건문
### 반복문
### 함수
### 인스턴스
### 상속
### 이벤트
### 맵핑 & 배열 & 구조체
### 에러핸들러
### 모디파이어 & 