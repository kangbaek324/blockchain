# RemixIDE 사용방법

### RemixIDE란?
![Solidity 로고](/Images/RemixLogo.png)
브라우저에서 솔리디티(Solidity)를 이용하여 스마트컨트랙트를 개발하고 배포하기 위한 웹 기반 통합개발환경(IDE)이다

### 데스크톱에 설치하기

Remix 다운로드링크

 https://github.com/remix-project-org/remix-desktop-insiders/releases/tag/v1.0.8-insiders

![remix_1.png](/Images/remix_1.png)
windows는 확장자가 exe파일, Mac은 확장자가 dmg인 파일을 다운로드한다

![remix_2.png](/Images/remix_2.png)
![remix_3.png](/Images/remix_3.png)

설치완료
## 사용방법
### 컴파일
![useRemix_1](/Images/useRemix_1.png)
솔리디티 로고 버튼을 클릭한다
컴파일 버튼을 누른다

컨트롤 + S 단축키로 바로 컴파일 할 수도있다
### 배포 
![useRemix_2](/Images/useRemix_2.png)
이더리움 로고 버튼을 클릭한다

배포할 컨트랙트와 배포를 할 계좌, Gaslimit등 다양한 옵션을 만져준뒤 Deploy 버튼을 눌러준다

### 디버깅
![useRemix_3](/Images/useRemix_3.png)
왼쪽하단 DeployContracts를 보면 내가 배포한 컨트랙트와 그 컨트랙트의 함수를 호출해볼 수 있다

솔리디티에는 로그를 출력하는법이 존재하지 않는다

대신 Debug 기능을 이용하여 실제 변수에 무슨 값이 들어가고 어떻게 프로그램이 흘러가는지 디버깅 해볼 수 있다

오른쪽 Debug 버튼을 눌러보자

![useRemix_4](/Images/useRemix_4.png)

왼쪽 위에 파란색 바를 조정해 순서대로 변수들의 값 등이 어떻게 변화하는지 디버깅해볼 수 있다

