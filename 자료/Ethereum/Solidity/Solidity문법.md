# 문서 소개
최근 수정 시각 : 2025 - 04 - 10

이 문서는 현재 작성 중이며, 내용이 변경될 수 있습니다

소개 : Solidity문법을 학습하는 문서입니다
# Solidity란
![Solidity 로고](/자료/Images/Solidity.png)

공식문서 : https://docs.soliditylang.org/en/v0.8.29/

번역본 : https://solidity-kr.readthedocs.io/ko/latest/

이더리움 블록체인에서 스마트컨트랙트를 제작하기 위한 언어
# 스마트 컨트랙트란 
블록체인에서 특정 조건이 충족되면 자동으로 작동되는 프로그램
# Solidity를 배우기전에 알아야 할 내용
<b>Gas</b> <br>
스마트컨트랙트를 배포 / 사용 할때 지불하는 비용 

<b>Gwei, wei</b> <br>
가스비를 낼때 사용되는 단위 <br>
1 ether = 10^9 Gwei = 10^18 wei

가스는 스마트컨트랙트에서 어떤 행위를 하느냐에 따라 부과되는 가스비가 다르다 자세한 내용은 이더리움 옐로우 페이퍼를 보면 나와있다 <br>
https://ethereum.github.io/yellowpaper/paper.pdf 

이러한 점 때문에 스마트컨트랙트를 작성할때는 가장적게 가스가 나오도록 작성해야한다

가스는 해커들의 DDOS 공격을방지 할수도 있다 => 가스를 발생시킴으로 블록체인 네트워크의 과부화를 방지 (가스 발생으로 인한 비용 부담)

## Gwei와 wei가 있는데 Gas는 또 뭔가?
이더리움에서 Gwei와 wei는 이더(ETH)의 단위이지만, Gas는 연산량을 측정하는 개념이다 즉, 이더는 화폐 자체이며 시장 가격이 존재하지만, 트랜잭션 비용을 직접 ETH 단위로 책정하면 혼란을 야기할 수 있다. 이를 해결하기 위해 Gas라는 별도의 단위를 도입했다 비유를 하자면
<pre>
gas = 자동차의 연료 소비량 (ex: 10L)

gas price = 연료 1L당 가격 (ex: 1,500원/L)

최종 연료비 = 연료 소비량 * 연료 가격 (ex: 10L * 1,500원 = 15,000원)
</pre>

이렇게 비교해볼 수 있다. 즉, Gas는 연산량을 나타내는 단위이고, Gas Price는 그 연산량을 ETH로 변환하는 비용 단위라고 이해할 수 있다.

## Gas Price(가스 가격)는 누가 정하는가?
<b>사용자(트랜잭션 보내는 사람)</b>
* 트랜잭션을 전송할 때 "나는 1 Gas당 20 Gwei를 지불하겠다!"라고 설정할 수 있음.

* 더 높은 Gas Price를 설정하면 더 빨리 처리될 가능성이 큼.

* 너무 낮게 설정하면 네트워크에서 무시될 수도 있음.

<b>네트워크 상태(혼잡도)</b>

* 사용자가 많고 혼잡하면 Gas Price가 올라감.

* 반대로 한산하면 Gas Price가 낮아짐.

<b>채굴자(또는검증자)</b>

* 트랜잭션을 처리하는 사람(검증자)이 높은 Gas Price를 제시한 트랜잭션을 먼저 선택함.

즉, Gas Price가 높을수록 우선 처리될 가능성이 커짐.

### GasLimit과 BlockGasLimit
<b>Gas Limit</b> <br>
특정 트랜잭션이 사용할 수 있는 최대 가스량

<b>BlockGasLimit</b> <br> 
블록이 포함할 수 있는 최대 가스량 

BlockGasLimit은 네트워크에서 지정됨 

# Solidity 문법
## 라이센스 선언
솔리디티는 코드 맨 윗줄에 라이샌스를 명시해줘야지만 오류가 나지 않는다.
<pre>
// SPDX-License-Identifier: GPL-30
</pre>
라이센스의종류는 GPL-30 라이센스 뿐만 아니라 다양하다

### 버전 선언
솔리디티는 컴파일러에게 몇 버전의 컴파일러를 사용하여 컴파일 할건지 알려줘야하기 떄문에 pragma를 이용하여 컴파일 버전을 명시해줘야한다.
<pre>
pragma solidity >= 0.7.0 < 0.9.0; // 0.7.0 ~ 0.9.0 버전을 사용
pragma solidity ^0.8.0; // 0.8.0 이상의 버전을 사용
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

## 변수
## DataType(자료형)

솔리디티에서는 크게 ValueType(값 타입)과 ReferenceType(참조 타입)으로 나뉜다.

## ValueType(값 타입)

### bool
true false를 정의 
### bytes 
bytes 형식으로 값을 저장할때 사용된다
### address 
이더리움의 지갑주소를 저장할때 사용된다

### int, uint 
정수를 저장할때 사용되며 뒤에 8, 16과 같은 숫자를 붙여 변수의 크기를 지정할수 있다 <br> (uint는 unsigned integer의 약자이며 양의 점수만 저장한다) <br> <br> 보통은 음의정수는 사용할 일이 없기 때문에 uint를
많이 사용한다
<pre>
bool b1 = !false; // true
bool b2 = false || true // true
bool b3 = false == true; // false
bool b4 = false && true; // false

// bytes 1 ~ 32
bytes4 bt = 0x12345678;
bytes bt2 = "string"; // 0x535452494e47

// 40자리의 이더리움 주소
address addr = 0x5B38Da6a701c568545dCfcB03FcB875f56beddC4;

// 뒤에 숫자를 생략하게 되면 자동으로 256으로 선언된다
uint number = 3;
uint8 number = 3;
</pre>
## ReferenceType(참조형 타입)
### string
문자열을 저장할때 사용한다
<pre>
string 변수이름 = 값;

// 예시
string name = "BaekHo";
</pre>

### mapping
key : value 형식의 데이터를 저장 해야될때 사용한다

<pre>
// 맵핑 정의하기
mapping(key타입 => value타입) 접근제한자 이름;

// 조회하기
이름[key];

// value 값 넣기
이름[key] = value;

// 예시
mapping (address => bool) voted;
voted[0x5B38Da6a701c568545dCfcB03FcB875f56beddC4];
voted[0x5B38Da6a701c568545dCfcB03FcB875f56beddC4] = true;
</pre>

### array
솔리디티에서 배열 선언 / 사용 방법은 다음과같다
<pre>
타입[크기] 접근제한자 배열이름;

// 예시
uint[] public ageArray;
uint[50] public ageArray;

// 배열 사용하기

// 값 넣기
ageArray.push();

// 배열길이 구하기
ageArray.length;

// 값 삭제

// 가장 최근값 삭제
ageArray.pop();
// 특정 값 삭제
delete ageArray[index];
</pre>
### struct
구조체란 말 그대로 구조형식의 데이터를 만들때 사용한다 사용자지정 타입을 만든다고 생각하면된다 <br> 솔리디티에서의 구조체 선언 방법은 다음과 같다.
<pre>
struct 구조체이름 {
    자료형 타입 변수명;
    ...
}

// 예시
struct User {
    string name;
    uint age;
}

function createUser(string memory name, uint256 _age) pure public returns(User memory) {
    return User("BaekHo", 18);
}

</pre>


## mapping VS array
솔리디티에서 보통 배열보다 맵핑 선호도가 높다 왜냐하면 배열의 장점은 배열을 순환할수 있는것인데 배열을 순환하기 위해서는 반복문이 사용되고 이점 때문에 DDOS 공격과 같은 공격으로 이더리움 네트워크가 과부화 될수도있다


## 변수 네이밍 규칙
private, 함수의 인자 변수는  변수앞에 _를 붙여 선언한다
<pre>
uint256 private _age = 1;

fucntion what(uint256 _age) {

}
</pre>

## 저장방식
솔리디티에서는 변수가 저장되는 데이터 영역으로 크게 3가지 있다<br>


### storage <br>
블록체인에 영구적인 상태를 저장하는 영역이다, 상태변경 함수가 호출될 때마다 해당상태의 변경 사항이 블록체인의 storage에 저장된다 블록체인에 영구저장되는 만큼 접근하는것에 대한 가스비가 비싸다
<br>
<pre>
contract ex {
    uint256 public PrettyNumber;
}
</pre>
### memory <br>
함수가 실행되는 동안에만 사용할 수 있는 가상메모리 영역 함수가 실행될동안만 동적으로 할당되며 함수내에서 임시로 사용되는 데이터를 저장하기 위해 사용됨
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
솔리디티에서 함수 호출시 인자들이 저장되는 영역이고 오직 읽기 전용 영역이다 <br>
calldata 영역에 저장되는 데이터는 변하지 않으며 일반적으로 내부에서 읽기만 할때 사용되고 함수호출이 끝날때 자동으로 삭제된다

<pre>
contract ex {
    function wow(uint256 a, calldata c) {
        
    }
}
</pre>
## memory VS calldata
<b>memory</b>
* 함수의 실행 중에 생성되고 함수의 실행이 종료되면 사라짐
* 함수 실행시 일시적으로 저장되는 변수와 배열, 구조체 등의 데이터를 저장하는 임시공간
* 함수의 인자 중 함수 내부에서 수정 가능

<b>calldata</b>
* 함수를 호출할 때 생성되고 함수의 호출이 끝나면 사라짐
* 읽기 전용
* 외부에서 함수로 전달되는 인자들의 값 저장
<br>

## 조건문
if 안에 있는 조건이 발동되었을때 코드를 실행시켜주는 구조이다
<pre>
if (발동조건) {

}
else if (앞전 if문이 아니고 이 if문을 만족한다면) {

}
else if (앞전 if문이 아니고 이 if문을 만족한다면) {

}
else {
    // 위 if문들을 모두 만족하지 않는 다면
}
</pre>
## 반복문
반복문은 특정 조건이 참(true)인 동안 코드 블록을 반복 실행하는 구조이다
### for
<pre>
for(초기값; 값이 얼마나 돌아야하는지; 한번돌때마다 변화) {
    // 실행될 내용
}

// 예시
for(uint i = 0; i < 5; i ++) {
    emit importantMassage(i);
}
</pre>
### while
<pre>
while(반복할 조건 (조건이 참일때 발동)) {
    // 실행될 내용
} 

// 예시
while(true) {
    emit importantMassage("메롱");
}
</pre>
### do-while
while문과 비슷한 구조를 가졌지만 while과 다르게 먼저 실행한뒤 검사를 하는 구조를 가졌다
<pre>
do {

} while ()
</pre>

## break, continue
### break
현재 반복문을 빠져나옴
<pre>
while(true) {
    break;
}
</pre>
### continue
다음 반복문으로 이동함
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
function 함수이름() (view또는pure.. 생략도가능) 접근제한자 returns(반환타입) {
    // 함수 내용
}
</pre>
returns()는 옵션이다

## 함수에서 인자값으로 레퍼런스타입이 들어올때
함수에서 인자값으로 레퍼런스타입이 들어오게되면 저장방식을 명시해줘야한다

<pre>
function test(string memory name) {
    return name;
}
</pre>

## 접근 제한자
여기서 접근 제힌자란 말 그대로 접근을 어떻게 제한할 것인지 선택하는 옵션이다 <br>
함수 & 변수등 다양하게 적용할수 있다 생략할경우 public으로 선언된다.

<b>public</b>: <br>
모든곳에서 접근가능

<b>external</b>: <br>
public 처럼 모든 곳에서 접근가능하나 external이 선언된 컨트랙트에서는 접근이 불가능함

<b>private</b>: <br>
오직 private이 정의된 컨트랙트에서만 사용가능

<b>internal</b>: <br>
private와 비슷하지만 상속한 컨트랙트에서 접근할수 있다는 차이점이 있다.

<pre>
uint256 public a1 = 2;
uint256 private a2 = 5;
</pre>

## view, pure
참고 : https://jerryjerryjerry.tistory.com/147 <br><br>
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
view보다 더 엄격한 조건을 가지며 블록체인에서 상태변화를 일으키지 않고 오로지 계산만 수행한다
<pre>
uint256 public a = 1;

function read_a2(uint8 a, uint8 b) public pure returns(uint256) {
    return a + b
}
</pre>
### 명시를 안한 경우 <br>
function 밖의 함수를 읽어서 변경할수 있음

<pre>
uint256 public a = 1;

function read_a3() public returns(uint256) {
    uint256 a = 13;
    return a;
}
</pre>


## 인스턴스
## 상속
솔리디티는 is를 사용하여 컨트랙트끼리 상속을 받을수있다
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
super.상속된컨트랙트의 함수

// 예시

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

## 이벤트
이벤트란 블록체인에 로그를 생성하는 기능이다 주로 프론트엔드에서 스마트컨트랙트에서 이벤트를 구독하여 컨트랙트의 상태변화를 감지하기 위해 사용된다
<pre>
event 이벤트명(자료형 변수이름) => 출력하고 싶은 변수를 넣으면 된다

// 예시
event info(string name, uint256 money);
</pre>

## 이벤트 발생시키기
<pre>
emit info("BaekHo", 1000); 
</pre>

## indexed
indexed란 이벤트에서 사용하는 키워드이다 

특정한 이벤트에 대해 가져올수 있다 numbertracker같은경우 프론트엔드에서 인덱스를 넣어 요청하더라도 전체 로그가 나오지만 numbertracker2 같은 경우는 indexed를 사용했기때문에 인덱스를 넣어 특정 로그를 조회할수 있다 

필터 기능이라고 생각하면 편한다

<pre>
contract ex [
    event numberTracker(uint256 num, string str);
    event numberTracker2(uint256 indexed num, string str);

    uint256 num = 0;
    function pushEvent(s)
]
</pre>

## 에러핸들러
솔리디티에서는 올바르지 않은 요청을 걸러내기 위해 에러핸들러를 사용한다


## assert, revert, require 0.4.22 ~ 0.7x 버전
assert, revert, require는 0.4.22
### require
다음과 같이 선언하고 괄호안의 값이 false라면 오류를 발생시킨다

assert: 가스를 다 소비한후 ,특정한 조건에 부합하지 않으면 (false) 에러를 발생시킨다

revert: 조건없이 에러를 발생시키고 gas를 환불 시켜준다

require 특정한 조건에 부합하지 얺으면(false) 에러를 발생시키고, gas를 환불해준다


### assert
<pre>
function assertNow() public pure {
    assert(false);
}
</pre>

### revert
<pre>
function revertNow() public pure {
    revert("~에러~");
}
</pre>

### require
<pre>
</pre>

<pre>
// 사용방법
require(검사할 내용)


// 예시
function wow() {
    require(prettyNumber == prettyNumber2);
    //....
}
</pre>
## assert, revert, require 0.8 버전
## 여러가지 문법의 버전을 알아야하는 이유
개발을 하다 보면 여러 라이브러리를 사용하게되는데 라이브러리의 내용들이 전부 다르기 때문에 여러버전의 문법을 알아두면 좋다

## modifier & SPDX
## 이더 보내기
## import 
## interface
## 라이브러리
## 참고한 자료
all <br>
https://www.inflearn.com/course/%EC%86%94%EB%A6%AC%EB%94%94%ED%8B%B0-%EC%8A%A4%EB%A7%88%ED%8A%B8-%EC%BB%A8%ED%8A%B8%EB%9E%99/dashboard <br>view, pure <br>
https://jerryjerryjerry.tistory.com/147 <br>
저장방식 <br>
https://jerryjerryjerry.tistory.com/148