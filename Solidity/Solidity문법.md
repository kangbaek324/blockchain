# 문서 소개
최근 수정 시각 : 2025 - 06 - 15

이 문서는 현재 작성 중이며, 내용이 변경될 수 있습니다

소개 : Solidity문법을 학습하는 문서입니다
# Solidity란
![Solidity 로고](/Images/Solidity.png)

공식문서 : https://docs.soliditylang.org/en/v0.8.29/

번역본 : https://solidity-kr.readthedocs.io/ko/latest/

이더리움 블록체인에서 스마트컨트랙트를 제작하기 위한 언어이다
# 스마트 컨트랙트란 
블록체인 기술을 활용해 제3의 인증기관 없이 개인 간 계약이 이루어질 수 있도록 하는 기술이라고한다 쉽게말해서

블록체인에서 특정 조건이 충족되면 자동으로 작동되는 프로그램

이라고 생각하면 편하다
# Solidity를 배우기전에 알아야 할 내용
### Gas <br>
스마트컨트랙트를 배포 / 사용 할때 지불하는 비용 

### 이더 단위 <br>
1 ether = 10^18 wei
i wei = 10^9 Gwei

Gwei는 보통 가스비를 계산할때 사용된다

가스는 스마트컨트랙트에서 어떤 행위를 하느냐에 따라 부과되는 가스비가 다르다 <br> 자세한 내용은 이더리움 옐로우 페이퍼를 보면 나와있다 <br>
https://ethereum.github.io/yellowpaper/paper.pdf 

이러한 점 때문에 스마트컨트랙트를 작성할때는 가스비를 가장 적게 사용하도록 효율적으로 코드를 작성해야한다

또한 가스는 해커들의 DDOS 공격을방지 할수도 있다 => 가스를 발생시킴으로 블록체인 네트워크의 과부화를 방지 (가스 발생으로 인한 비용 부담)

## Gwei와 wei가 있는데 Gas는 또 뭔가?
이더리움에서 Gwei와 wei는 이더(ETH)의 단위이지만, Gas는 연산량을 측정하는 개념이다 즉, 이더는 화폐 자체이며 시장 가격이 존재하지만, 트랜잭션 비용을 직접 ETH 단위로 책정하면 혼란을 야기할 수 있다. 이를 해결하기 위해 Gas라는 별도의 단위를 도입했다 비유를 하자면
<pre>
gas = 자동차의 연료 소비량 (ex: 10L)

gas price = 연료 1L당 가격 (ex: 1,500원/L)

최종 연료비 = 연료 소비량 * 연료 가격 (ex: 10L * 1,500원 = 15,000원)
</pre>

이렇게 비교해볼 수 있다. 즉, Gas는 연산량을 나타내는 단위이고, Gas Price는 그 연산량을 ETH로 변환하는 비용 단위라고 이해할 수 있다.

하드포크와 같은 일이 발생하면 GasPrice가 아닌 Gas가격 자체가 오르는 경우도 있다 (이스탄불 하드포크)

관련 자료 : https://medium.com/tokamak-network/%EC%9D%B4%EB%8D%94%EB%A6%AC%EC%9B%80-%EC%9D%B4%EC%8A%A4%ED%83%84%EB%B6%88-%ED%95%98%EB%93%9C%ED%8F%AC%ED%81%AC-cd9737f21753
## Gas Price(가스 가격)는 누가 정하는가?
<b>사용자(트랜잭션 보내는 사람)</b>
* 트랜잭션을 전송할 때 "나는 1 Gas당 20 Gwei를 지불하겠다!"라고 설정할 수 있음.

* 더 높은 Gas Price를 설정하면 더 빨리 처리될 가능성이 큼

* 너무 낮게 설정하면 네트워크에서 무시될 수도 있음

<b>네트워크 상태(혼잡도)</b>

* 사용자가 많고 혼잡하면 Gas Price가 올라감

* 반대로 한산하면 Gas Price가 낮아짐

<b>채굴자(또는검증자)</b>

* 트랜잭션을 처리하는 사람(검증자)이 높은 Gas Price를 제시한 트랜잭션을 먼저 선택함.

즉, Gas Price가 높을수록 우선 처리될 가능성이 커짐

## GasLimit과 BlockGasLimit
<b>Gas Limit</b> <br>
특정 트랜잭션이 사용할 수 있는 최대 가스량

<b>BlockGasLimit</b> <br> 
블록이 포함할 수 있는 최대 가스량 

BlockGasLimit은 네트워크에서 지정된다

# Solidity 문법
## 라이센스 선언
솔리디티는 코드 맨 윗줄에 라이센스를 명시해줘야한다 명시하지 않으면 권장하지 않는 방식이라고 경고한다
<pre>
// SPDX-License-Identifier : GPL-30
</pre>
라이센스의종류는 GPL-30 라이센스 뿐만 아니라 다양하다

라이센스를 작성하는 이유는 크게 두가지로 컨트랙트의 신뢰성, 저작권 문제가 있다
### 알아두기
라이센스는 0.6.8버전 부터 도입 되었기 때문에 0.6.7버전 이하라면 경고가 나지 않는다

### 버전 선언
작성된 소스코드가 몇 버전에서 컴파일해야지 안정적인지 알려주는 키워드이다
사실상 이 범위의 버전에서 컴파일하라는 의미에 더 가깝다

필수는 아니기에 작성하지 않아도 컴파일은 되지만 작성하라는 경고문이 뜬다
<pre>
pragma solidity >= 0.7.0 < 0.9.0; // 0.7.0 ~ 0.9.0 버전을 사용
pragma solidity ^0.8.0; // 0.8.0 이상의 버전을 사용
</pre>
버전은 어떤 버전이든 사용해도 상관없지만 해당문서에서는 0.8.0 버전을 사용하는것을 권장한다

### 컨트랙트 선언하기
컨트랙트는 컨트랙트에서 실행될 코드가 담겨있는곳으로 객체지향의 클래스와 유사하다
<pre>
// SPDX-License-Identifier : GPL-30
pragma solidity ^0.8.0;

contract Hello {

}
</pre>

### 알아두기 
컨트랙트를 배포하게 되면 배포한 사람의 주소와 무관하게 컨트랙트자체에 주소가 부여되며 이더를 주고 받을 수 있다
## 변수
Solidity에서 변수를 선언하는법에 대해서 알아보자
## DataType(자료형)

솔리디티에서는 크게 ValueType(값 타입)과 ReferenceType(참조 타입)으로 나뉜다.

## ValueType(값 타입)

### bool
true false 값을 정의한다 

<pre>
bool b1 = !false; // true
bool b2 = false || true // true
bool b3 = false == true; // false
bool b4 = false && true; // false
</pre>
### bytes 
bytes 형식으로 값을 저장할때 사용된다
<pre>
// bytes 1 ~ 32
bytes4 bt = 0x12345678;
bytes bt2 = "string"; // 0x535452494e47
</pre>
### address 
이더리움의 지갑주소를 저장할때 사용된다
<pre>
// 40자리의 이더리움 주소
address addr = 0x5B38Da6a701c568545dCfcB03FcB875f56beddC4;
</pre>

### int, uint 
정수를 저장할때 사용되며 뒤에 8, 16과 같은 숫자를 붙여 변수의 크기를 지정할수 있다 <br> (uint는 unsigned integer의 약자이며 양의 점수만 저장한다) <br> <br> 보통은 음의정수는 사용할 일이 없기 때문에 uint를
많이 사용한다
<pre>
// 뒤에 숫자를 생략하게 되면 자동으로 256으로 선언된다
uint number = 3;
uint8 number = 3;

### 상수

uint public constant z = 3; // 상수
</pre>
## ReferenceType(참조형 타입)
### string
문자열을 저장할때 사용한다
<pre>
string 변수명 = 값;
</pre>
<pre>
string name = "BaekHo";
</pre>

### mapping
key : value 형식의 데이터를 저장 해야될때 사용한다
길이 제한이 없다

<pre>
mapping(key타입 => value타입) 접근제한자 변수명;
</pre>
<pre>
// 조회하기
변수명[key];

// 값 넣기
변수명[key] = value;

mapping (address => bool) voted;
voted[0x5B38Da6a701c568545dCfcB03FcB875f56beddC4];
voted[0x5B38Da6a701c568545dCfcB03FcB875f56beddC4] = true;

// 삭제하기
delete(voted[0x5B38Da6a701c568545dCfcB03FcB875f56beddC4]);
// voted[0x5B38Da6a701c568545dCfcB03FcB875f56beddC4] = 0;
// delete도 삭제되면 0으로 치환하기 때문에 0으로 치환해도 됨
</pre>

### array
관련있는 여러 데이터를 하나의 변수로 묶을때 사용한다
mapping과 다르게 길이가 있다
<pre>
배열타입[크기] 접근제한자 변수명;
</pre>
크기를 지정해주지 않으면 동적으로 사용할 수 있다
<pre>
uint[] public ageArray;
uint[50] public ageArray;

// 값 넣기
ageArray.push();

// 배열길이 구하기
ageArray.length;

// 가장 마지막 인덱스 값 삭제
ageArray.pop();

// 원하는 인덱스 삭제
delete ageArray[index];
</pre>
#### 알아두기
pop을 사용하게되면 배열의 길이가 줄어들지만 delete 함수를 사용하여 삭제하게되면 길이가 줄어들지 않는다
### struct
구조체란 말 그대로 구조형식의 데이터를 만들때 사용한다 사용자지정 타입을 만든다고 생각하면된다 <br> 솔리디티에서의 구조체 선언 방법은 다음과 같다.
<pre>
struct 구조체명 {
    자료형 변수명;
    자료형 변수명;
    자료형 변수명;
}
</pre>
<pre>
struct User {
    string name;
    uint age;
}

function createUser(string memory name, uint256 _age) pure public returns(User memory) {
    return User("BaekHo", 18);
}

</pre>

### enum
여러 값들 중 하나를 선택할 수 있도록 미리 정해둔 집합형 타입이다
사용할때는 enumName.hello와 같이 접근할 수 있지만 내부적으로는 uint(0 1 2 3 ...)의 형태로 저장되어 관리된다

원래 타입이 uint8이여서 uint8로 형변환이 가능하다 그리고 uint8의 최대범위가 0-255이기때문에 enum안에서 277개 이상을 선언하게 되면 컴파일 오류가 발생한다 

<pre>
enum 변수명 {  }
</pre>

<pre>
enum Light { 
    TurnOn,
    TurnOff
}

Light public lightStatus;
lightStatus = Light.TurnOn;

event currentStatus(Light _lightStatus, uint _lightStatus);
 
</pre>

## mapping VS array
솔리디티에서 보통 배열보다 맵핑 선호도가 높다 왜냐하면 배열의 장점은 배열을 순환할 수 있는것인데 배열을 순환하기 위해서는 반복문이 사용되고 이점 때문에 DDOS 공격과 같은 공격으로 이더리움 네트워크가 과부화 될 수도있기 때문이다


## 변수 네이밍 규칙
private, 함수의 인자 변수는  변수앞에 _를 붙여 선언한다
<pre>
uint256 private _age = 1;

fucntion what(uint256 _age) {

}
</pre>

## 접근 제한자
여기서 접근 제힌자란 말 그대로 접근을 어떻게 제한할 것인지 선택하는 옵션이다 <br>
함수, 변수등 다양하게 적용할수 있다 생략할경우 public으로 선언된다.

<b>public</b>

모든곳에서 접근이가능하다
( 변수 적용시 함수 생성 )

<b>external</b> 

public 처럼 모든 곳에서 접근가능하나 external이 선언된 컨트랙트에서는 접근이 불가능하다<br>
( 변수 적용 불가, this(현재 컨트랙트를 나타냄) 키워드 사용시 내부 접근 가능 )

<b>private</b>

오직 private이 정의된 컨트랙트에서만 사용가능하다

<b>internal</b>

private와 비슷하지만 상속한 컨트랙트에서 접근할수 있다는 차이점이 있다.

<pre>
uint256 public a1 = 2;
uint256 private a2 = 5;
</pre>

## 저장방식
솔리디티에서는 변수가 저장되는 데이터 영역으로 크게 4가지 있다<br>


### storage <br>
영속적인 읽고 / 쓰기가 가능한 저장공간이다 함수의 외부 변수, 함수를 저장한다

영속적이기때문에 가스비용이 비싸다
<br>
<pre>
contract ex {
    uint256 public PrettyNumber;
}
</pre>
### memory <br>
함수가 실행되는 동안에만 사용할 수 있는 가상메모리이다 

영역 함수가 실행될동안만 동적으로 할당되며 함수내에서 임시로 사용되는 데이터(함수의 내부에 정의된 변수, 매개변수, 반환값등)를 저장하기 위해 사용된다

임시공간이기 때문에 함수의 실행이 종료되면 이 공간은 삭제된다
<br>
<pre>
contract ex {
    function temp() {
        uint 256 temp = 0;
    }
}
</pre>
### calldata <br>
솔리디티에서 함수 호출시 인자들이 저장되는 영역이고 오직 읽기 전용 영역이다 
calldata 영역에 저장되는 데이터는 변하지 않으며 일반적으로 내부에서 읽기만 할때 사용되고 함수호출이 끝날때 자동으로 삭제된다

<pre>
contract ex {
    function wow(uint256 a, calldata c) {
        
    }
}
</pre>

### stack
EVM에서 stack data를 관리할떄 쓰는 영역 1024MB로 제한

storage, memory, calldata 순으로 가스비가 비싸다 
## memory VS calldata
<b>memory</b>
* 함수의 실행 중에 생성되고 함수의 실행이 종료되면 사라짐
* 함수 실행시 일시적으로 저장되는 변수와 배열, 구조체 등의 데이터를 저장하는 임시공간
* 함수의 인자 중 함수 내부에서 수정이 가능

<b>calldata</b>
* 함수를 호출할 때 생성되고 함수의 호출이 끝나면 사라짐
* 읽기 전용
* 외부에서 함수로 전달되는 인자들의 값 저장
<br>

## 조건문
if 안에 있는 조건이 발동되었을때 조건문안에 정의된 코드를 실행시켜주는 구조이다
<pre>
if (발동조건) {
    // 특정코드
}
else if (앞전 if문이 아니고 이 if문을 만족한다면) {
    // 특정코드
}
else if (앞전 if문이 아니고 이 if문을 만족한다면) {
    // 특정코드
}
else {
    // 위 if문들을 모두 만족하지 않는다면
    // 특정코드
}
</pre>
## 반복문
반복문은 특정 조건이 참(true)인 동안 코드 블록을 반복 실행하는 구조이다
### for
<pre>
for(초기값; 조건식; 증감식) {
    // 실행될 내용
}
</pre>

<pre>
for(uint i = 0; i < 5; i ++) {
    emit importantMassage(i);
}
</pre>
### while
<pre>
while(조건문) {
    // 실행될 내용
} 
</pre>

<pre>
while(true) {
    emit importantMassage("메롱");
}
</pre>
### do-while
while문과 비슷한 구조를 가졌지만 while과 다르게 먼저 실행한뒤 검사를 하는 구조를 가졌다
<pre>
do {

} while (조건문)
</pre>

## break, continue
### break
반복문내에서 만나면 현재 반복문을 빠져나온다
<pre>
while(true) {
    break;
}
</pre>
### continue
반복분내에서 만나면 다음 반복문으로 이동한다
<pre>
for(uint i = 0; i < 100; i++) {
    if (i % 2 == 0) {
        i++;
    }
    else {
        continue;
    }
}
</pre>
## 함수
함수는 다음과 같은 형식으로 선언할 수 있다
<pre>
function 변수명() (view또는pure.. 생략도가능) 접근제한자 returns(반환타입) {
    // 함수 내용
}
</pre>
( returns()는 옵션이다 )

returns(uint256 total)와 같이 리턴 변수를 지정해줄수도 있다

같은 컨트랙트에서  함수를 호출할떄는 this라는 키워드를 앞에 붙인뒤 호출한다

## 함수에서 인자값으로 레퍼런스타입이 들어올때
함수에서 인자값으로 레퍼런스타입이 들어오게되면 저장방식을 명시해줘야한다

<pre>
function test(string memory name) {
    return name;
}
</pre>

## view, pure
솔리디티는 함수를 정의할때 함수 옆애 붙어서 제약을 걸어주는 키워드들이 존재한다 이 키워드를 잘 사용하면 컨트랙트의 가스비를 줄이거나 간편히 제약을 걸 수 있다.
### view <br>
단순히 값을 읽을때 사용된다 <br>
<pre>
uint256 public a = 1;

function read_a() public view returns(uint256) {
    return a;
}
</pre>
### pure <br>
view보다 더 엄격한 조건을 가지며 블록체인에서 상태변화를 이르키지 않고 순수하게 함수안에있는 변수만을 사용할때 사용한다
<pre>
uint256 public a = 1;

function read_a2(uint8 a, uint8 b) public pure returns(uint256) {
    return a + b
}
</pre>
### 명시를 안한 경우 <br>
function 밖의 함수를 읽어서 변경할 수 있다

<pre>
uint256 public a = 1;

function read_a3() public returns(uint256) {
    uint256 a = 13;
    return a;
}
</pre>

## 생성자
- 스마트 컨트랙트가 배포될때 제일 먼저 작동하는 함수
- 스마트 컨트랙트를 배포할때 마다 특정한 값을 세팅해줌
<pre>
contract lec {
    constructor() {
        
    }
}
</pre>

## 상속
솔리디티는 is를 사용하여 컨트랙트끼리 상속을 받을 수 있다
<pre>
 contract Son is Father {

 }
</pre>
만약 Father 컨트랙트에 생성자에 값을 넣고 싶다면 다음과 같이 넣어주면 된다 
<pre>
 contract Son is Father("BaekHo") {

 }

 또는 

  contract Son is Father {
    constructor() Father("BaekHo")
 }
</pre>

## 오버라이딩

먼저 오버라이딩할 함수에 virtual 이라는 키워드를 입력한다
<pre>
function getMoney view 여기 또는 public 여기 returns(uint256) {
    return money;
}
</pre>

그리고 상속받은 컨트랙트에서는 같은 자리에 override 라고 입력한다  

그다음 함수안의 내용을 원하는 내용으로 변경해주면 된다.

## 두개 이상 상속하기
컨트랙트를 두개 이상 상속할경우에는 , 로 구분한다

<pre>
contract Son is Father, Mother {

}
</pre>

이떄 두 컨트랙에 동일한 함수가 존재할경우 오버라이딩을 해야된다

<pre>
contract Son is Father, Mother {
    function getMoney() public view override(Father, Mother) returns(uint256) {
        return father + motherMoney
    }
}
</pre>

## super
super는 상속받은 컨트랙트에 존재하는 함수를 호출하는데 사용되는 키워드이다 
<pre>
super.상속된컨트랙트의 함수명()
</pre>

<pre>
contract Parent {
    function sayHello() public pure virtual returns (string memory) {
        return "Hello from Parent";
    }
}

contract Child is Parent {
    function sayHello() public pure override returns (string memory) {
        return super.sayHello();  // Parent의 sayHello() 호출
    }
}
</pre>

두개 이상을 상속하였을때 상속한 컨트랙트에 동일한 함수가 있는경우 super로 불러오게되면 가장 오른쪽에 있는 즉 가장 최근에 선언되는 컨트랙트에 함수를 불러온다

만약 동일한 이름의 함수를 둘다 사용해야된다면
<pre>
상속받은컨트랙트이름.함수명()
</pre>
이렇게 명시적으로 불러올 수 있다

## 인스턴스롸
인스턴스화란 특정 스마트 컨트랙트를 개별적으로 만드는것이다
<pre>
스마트컨트랙트이름 변수명 = new 스마트컨트랙트명();

// 호출하기
변수명.함수명();
</pre>

<pre>
fatherWallet = wallet = new fatherWallet(); 

// 호출하기
wellet.addMoney();
</pre>

## 이벤트
이벤트란 블록체인에 로그를 생성하는 기능이다 주로 프론트엔드에서 스마트컨트랙트에서 이벤트를 구독하여 컨트랙트의 상태변화를 감지하기 위해 사용된다

블록체인의 특정 블록에 값을 저장하고 함수 내부에서 emit이라는 키워드를 사용해 사용할 수 있다
<pre>
event 이벤트명(자료형 변수이름) => 출력하고 싶은 변수를 넣으면 된다
</pre>

<pre>
event info(string name, uint256 money);
</pre>

### 이벤트 발생시키기
<pre>
emit info("BaekHo", 1000); 
</pre>

## indexed
indexed란 이벤트에서 사용하는 키워드이다 

특정한 이벤트에 대해 가져올수 있다 numbertracker같은경우 프론트엔드에서 인덱스를 넣어 요청하더라도 전체 로그가 나오지만 numbertracker2 같은 경우는 indexed를 사용했기때문에 인덱스를 넣어 특정 로그를 조회할수 있다 

필터 기능이라고 생각하면 편한다

<pre>
contract ex {
    event numberTracker(uint256 num, string str);
    event numberTracker2(uint256 indexed num, string str);

    uint256 num = 0;
    function pushEvent(s)
}
</pre>

## 에러핸들러
솔리디티에서는 올바르지 않은 요청을 걸러내기 위해 에러핸들러를 사용한다


## assert, revert, require 0.4.22 ~ 0.7x 버전
assert, revert, require는 0.4.22
### require
다음과 같이 선언하고 괄호안의 값이 false라면 오류를 발생시킨다

assert: 가스를 다 소비한후 ,특정한 조건에 부합하지 않으면 (false) 에러를 발생시킨다
<pre>
function assertNow() public pure {
    assert(false);
}
</pre>

revert: 조건없이 에러를 발생시키고 gas를 환불 시켜준다
<pre>
function revertNow() public pure {
    revert("errorMessage");
}
</pre>

require: 특정한 조건에 부합하지 얺으면(false) 에러를 발생시키고, gas를 환불해준다 revert + if라고 생각해도된다
<pre>
function requireNow() {
    require(false, "errorMessage") // 에러메세지 생략가능
}
</pre>

보통 개발환경에서는 assert를 사용하고 실제 컨트랙트를 작성할때는 revert + if 또는 require을 사용한다
## assert, revert, require 0.8 ~ 버전
assert: 오직 내부 테스트와 불변성 체크 용도로 사용된다 그리고 assert 에러를 발생시키면 Panic(uint256) 이라는 에러타입을 발생시킨다

또한 0.7X 버전과 다른점은 0.8.0 이전 버전은 실패하면 invalid opcode를 발생시켜 가스를 전부소모 하였지만 0.8.0 버전 이상부터는 Panic(uint256)revert를 발생시켜 가스를 환불해준다

#### 알아두기
함수자체를 실행하는 가스비는 환불해주지 않는다

## try / catch
try-catch 문은 코드 실행 중 발생할 수 있는 예외를 처리하기 위해 사용하는 구조이다

## catch의 종류
### catch Error(string memory reason) { ... }
revert 나 require을 통해 생성된 에러용도
### catch Panic(uint errorCode) { ... }
catch Panic은 0.8.1 버전 부터 사용가능하다

assert를 통해 생성된 에러가 날때
errorCode는 솔리디티에서 정의 Panic 에러 별로 나온다
### catch(bytesmemoryLowLevelData) { ... }
보통 로우 레벨에러를 잡을때 사용한다

<pre>
try {
    // 실행시킬 구문
} catch Error(...) {
    // 오류가 났을경우
} catch Panic(...) {
    // 오류가 났을경우
} catch(...) {
    // 오류가 났을경우
} catch {

}
</pre>

그냥 catch만 써줘도된다

<pre>
try this.simple() returns(uint256 _value) {
    
} catach {

}
</pre>

### 언제 사용되는가?
외부 스마트컨트랙트를 부를때 => 인스터화 생성하여 함수 호출

외부 스마트컨트랙트를 생성 할 때 => 인스턴스화 생성 할 때 사용

스마트컨트랙트 내에서 함수를 부를때

### 알아두기
try문안에서 발생한 assert, revert, require는 의도된 오류로 인식하여 catch에서 잡지 못한다 

Panic(uint256)은 0.8.0에 추가된 내장오류 타입이지만 try-catch 문에서 사용할 수 있는 버전은 0.8.1 버전부터이다

## 여러가지 문법의 버전을 알아야하는 이유
개발을 하다 보면 여러 라이브러리를 사용하게되는데 라이브러리의 내용들이 전부 다르기 때문에 여러버전의 문법을 알아두면 좋다


## modifier 
modifier란 미들웨어 같은 존재이다 여러 함수에 적용해야 될 부분있을때 사용되고 보통 require, assert, revert를 포함한다

<pre>
// 매개 변수가 있는 모디파이어
modifier 모디파이어 이름(자료형 매게변수이름) {
    // 로직
    _; // 나머지 함수를 마저 실행한다
}

갳
// 매게 변수가 없는 모디파이어
modifier 모디파이어 이름 {
    // 로직
    _; // 나머지 함수를 마저 실행한다
}
</pre>
<pre>
modifier onlyAdults(uint256 _age) {
    require(20 > Age, no kides);
    _; 
}

function functionName(_age) public onlyAdults(uint256 _age) returns(uint256) {
    // 어쩌구 저쩌구
}
</pre>
### 응용하기
modifier를 응용하면 특정권한을 가진 사람만 실행할 수 있도록 만들 수 있다
<pre>
modifier onlyOwner {
    require(owner == msg.sender, "only owner");
    _;
}
</pre>

## Solidity에서 쓸수 있는 전역변수
## 주소.balance
특정주소가 가지고 있는 이더의 잔액
## msg 
메세지 호출과 관련된 정보를 저장
### msg.sender
스마트 컨트랙트를 실행 / 요청한 주체
### msg.value
송금보낸 코인의 양 (송금액)
## tx
트랜잭션 자체에 대한 정보
## block 
블록에 대한 정보를 대한 정보

더 많은 변수는 공식문서에 보면 나와있다 

https://docs.soliditylang.org/en/latest/units-and-global-variables.html


## 이더를 보내는 방법
### payable
전송할, 받을 이더리움의 주소, 함수, 생성자에 붙이는 키워드이다
<pre>
// 이렇게 생성자에 payble을 붙이는 경우는 초기에 바로 컨트랙트에 이더를 넣고 싶을때 사용한다
constructor() payble {} 

function sendNow(address payble _to) public payble {
    ...
}
</pre>

이러한 키워드를 붙이는 이유는 이더라는 자산을 다른 지갑으로 옮기는 행위이기때문에 보안을 위해서 Solidity에게 옮기는것을 승인받아야하기 때문에 사용한다

### send
2300가스를 소비하고 성공여부를 true, false로 반환한다 <br> 단점으로는 에러를 보내지 못한다

<pre>
function sendEther(address payable recipient) public payable returns (bool) {
    require(msg.value > 0, "You must send some Ether");

    // 송금: recipient에게 이더 전송 (send 사용)
    bool sent = recipient.send(msg.value);

    require(sent, "Failed to send Ether");

    return sent;
}
</pre>
### call
send와 다르게 가변적인 가스 소비가 가능하다 (가스 지정가능) <br>
send와 동일하게 성공여부를 true, false로 반환한다 가스를 가변적으로 사용하기 때문에 재진입 공격에 취약하다  

call은 송금하는것뿐만 아니라 외부 스마트컨트랙트 함수를 부를 수 도있다
<pre>
// 송금하기

function transferEther(address payable _to) public payable {
    // ~ 0.7
    // (bool sucess) = _to.call.gas(1000).value(msg.value)("");
    // require(sucess, "fail"); 

    // 0.7 ~
    (bool success,) = _to.call{value: msg.value}("");
    require(success, "fail");
}

</pre>

<pre>
// 외부 컨트랙트 호출하기
function callMethod(address _ccontratAddr, uint256 _num1, uint256 _nume2) public {
    // _contractAddr는 외부 스마트컨트랙트 주소임
    (bool success, bytes memory outputFromCalledFunction) = _contractAddr.call(
        abi.encodeWithSignature("addNumber(uint256, uint256)", _num1, _num2);
    )

    require(success, "fail");
}


/*

_contractAddr.call이 부분을 _contractAddr.call{value: ...} 으로 변경하면 이더를 보내는 동시에 호출도 할 수 있다

*/

</pre>

#### 알아두기
send, transfer, call은 주소 타입의 내장 함수이다
### delegate call
delegate call은 기존의 call과 비슷하지만 다른 특징을 가지고 있다

### 1.msg.sender가 스마트컨트랙트 사용자를 가르킨다

call은 A가 B컨트랙트를 통해 C컨트랙트를 호출하게되면 C컨트랙트에서 msg.sender은 B컨트랙트주소를 나타내지만 delegate call은 본래의 컨트랙트 사용자인 A를 가르킨다

### 2.delegate call의 정의된 컨트랙트 (caller)가 외부 컨트랙의 함수들을 마치 자신의 것 처럼 사용한다 (실질적인 값도 caller에 저장)

B컨트랙트에서 C컨트랙트에 changeC()를 호출한다고 했을떄 call같은 경우에는 C컨트랙트에 있는 num이 바뀌지만 delegate call은 A컨트랙트에 있는 num이 변경된다

### 근데 delegate call은 왜 써야하는 걸까 <br>

A와 B 컨트랙트가 있고 사용자는 물건을 구매한뒤 A컨트랙트를 거쳐 B컨트랙트에 addPoint함수를 실행시켜 적립을 한다고 해보자

실제로 작동하는데에는 아무 문제가 없지만 회사방침이 변경되어 포인트가 5점에서 3점으로 바뀐다고 해보자 그렇게 된다면 컨트랙트를 수정해야하는데 이미 블록체인 네트워크상에 배포되었기때문에 컨트랙트를 재배포를 해야된다 <br><br> 재배포를 하게된다면 안에있는 값들이 모두 없어지기때문에 원래 네트워크에서 데이터를 가져와야된다 또한 사용자들에게 새로운 컨트랙트의 주소를 알려줘야한다 하지만 이렇게 하게되면 비용이 만만치 않게 된다

여기서 delegate call을 사용하면 어떻게 될까 <br>
delegate call을 사용하게된다면 A컨트랙트에 실질적인 값을 저장하고 B컨트랙트는 주요로직만 저장하게된다 만약 지급해야되는 포인트를 바꿔야된다면 A컨트랙트에 함수를 이용하여 B컨트랙트의 주소를 변경하고 B컨트랙트를 재배포하면 간단하게 해결할 수 있다

<pre>
 _contractAddr.call // 이거를
 _contractAddr.delegatecall // 이렇게 바꾸면 된다
</pre>

이러한 방식을 Upgradable Smart Contract 패턴이라고한다


### transfer
send와 call의 단점들을 보안하기 위해서 0.4.13버전 부터 등장하였다 <br>
2300가스를 소비하고 실패시 에러를 발생시킨다
<pre>
function sendEther(address payable recipient) public payable {
    require(msg.value > 0, "You must send some Ether");
    
    // 송금: recipient에게 이더 전송
    recipient.transfer(msg.value);  // msg.value는 송금된 이더의 양
}
</pre>

### 알아두기
#### ABI란
- ABI는 이더리움환경안에서 스마트컨트랙트를 상호작용하는 표준방법이다
- ABI는 컨트랙트 함수를 JSON 형태로 표현한 정보로 EVM이 컨트랙트 함수를 실행할 때 필요
- 컨트랙트 함수를 실행하려는 사람은 ABI 정보를 노드에 제공

이스탄불 하드포크이후 가스 가격이 올라 2019년 12월 이후부터는 call사용을 권장하고 있다 (이후에 가스가격이 오르게 되면 요청에 실패 할 수 도 있기때문)


## fallback
fallback 함수란 말 그대로 대비책함수이다 
이름이 없는 무기명 함수라고도 한다
### 기능
1. 스마트컨트랙트가 이더를 받을 수 있도록 한다

2. 이더를 받고 난 후 무언가의 행동을 할 수 있도록 한다

3. call함수로 없는 함수가 불려질때, 이떠한 행동을 취할 수 있도록 한다
## 0.6 버전 이전 fallback
<pre>
function external payable {
    
}
</pre>
## 0.6 버전 이후 fallback
0.6이후부터는 receive와 fallback으로 두가지 형태로 나뉘게 되었다
### receive 
순수하게 이더를 받을때만 작동한다
### fallback 
함수를 실행하면서 이더를 보낼때, 불려질 함수가 없을떄 작동한다
<pre>
// 불려진 함수가 컨트랙트에 없을때 작동
fallback() external {

}

// 이더를 받고 난뒤 특정행동
fallback() external payable {

}

// 이더를 받고 난뒤 특정행동
receive() external payable {
    
}
</pre>

두개의 함수로 나뉘었다고 기존의 fallback기능이 사라진것은 아니다

## interface
인터페이스란 스마트컨트랙트내에서 정의되여야 할 것을 나타낸다
쉽게 말해서 설명서 같은거라고 생격하면 편하다

interface 정의 규칙은 다음과 같다
1. 함수를 사용할경우는에는 external로 표기한다

2. enum, structs는 사용가능하지만 변수

3. 생성자는 사용할 수 없다


is를 사용하여 컨트랙트에 적용할 수 있다

<pre>
interface ItemInfo {
    struct item {
        string name;
        uint256 price;
    }
    function addItemInfo(string memory _name, uint256 _price) external;
    function getItemInfo(uint256 _index) external view returns(item memory);
}

contract test is ItemInfo {
    item[] public itemList;
    uint256[] public a;

    function addItemInfo(string memory _name, uint256 _price) override public {
        itemList.push(item(_name, _price));
    }

    function getItmeInfo(uint256 _index) override public view returns(item memory) {
        return itemList[_index];
    }
}
</pre>
## 라이브러리
재사용 가능한 코드 블럭을 모듈화하여 작성할 수 있도록 해주는 특수한 형태의 스마트 컨트랙트이다

가스는 코드의 사이즈 / 길이에따라서 가스비를 부과하기때문에 라이브러리를 사용하게된다면 재사용성을 높이고 가스비를 절감할 수 있다.

또한 데이터 타입에 적용한다면 좀 더 유연하게 사용할 수도 있다

이러한 라이브러리에는 3가지 제한사항이 존재한다
1. fallback 함수 사용불가   

2. 상속불가

3. payable 함수 정의 불가

### 라이브러리 작성 및 사용하기
라이브러리를 작성할떄는 contract 대신 library를 적어주면된다
<pre>
libary SafeMath {
    function add(uint 8, uint8b) internal pure returns (uint8) {
        require(a + b >= a, "SafeMath: addition overflow");
        return a + b;
    } 
}

contract lec {
    // using SafeMath; // 이 라이브러리를 사용한다
    using SafeMath for uint8; // uint8 변수타입에 적용한다
    uint public a;

    function libaryTest(uint8 _num1, uint8 _num2) public pure {
        a = SafeMath.add(_num1, _num2);
    }
}
</pre>
### 서로 다른 라이브러리안에 같은 이름의 함수가 있을경우
서로 다른 라이브러리안에 같은 이릉믱 함수가 있을경우 호출 할떄 명시적으로 호출 해줘야한다
<pre>
using LibA;
using LibB;

uint resultA = LibA.doSomething(5);
uint resultB = LibB.doSomething(5);
</pre>
## import
파일을 분리해서 컨트랙트를 작성했을때 다른 파일에 있는 컨트랙트를 그냥 가져오면 Solidity는 어디에 있는 컨트랙트를 말하는지 모른다

그렇기 떄문에 import를 통해서 어디에 있는지 명시를 해줘야한다

<pre>
import "경로"
</pre>

<pre>
import "./lec.sol"
</pre>
### 오픈소스 라이브러리 사용하기
<pre>
import "github .sol file link"
</pre>
## ETC
### Solidity Overflow
0.8.0이상 버전 부터는 Overflow시 자동으로 revert된다
## 참고한 자료
전체 <br>
https://www.inflearn.com/course/%EC%86%94%EB%A6%AC%EB%94%94%ED%8B%B0-%EC%8A%A4%EB%A7%88%ED%8A%B8-%EC%BB%A8%ED%8A%B8%EB%9E%99/dashboard
https://solidity-kr.readthedocs.io/ko/latest/ <br>
https://soliditylang.org/ <br>

view, pure <br>
https://jerryjerryjerry.tistory.com/147 <br>

저장방식 <br>
https://jerryjerryjerry.tistory.com/148 