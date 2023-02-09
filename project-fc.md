# :pushpin: 온라인 강의 실습 프로젝트
>강의정보: [패스트캠퍼스]30개 프로젝트로 배우는 iOS 앱 개발 with Swift 초격차 패키지 Online
>https://github.com/oneoneoneoneoneoneone/fc

</br>

## 1. 제작 기간 & 참여 인원
- 2022.03 ~ 2023.02
- 개인 프로젝트

</br>

## 2. 사용 기술
- UIKit / SnapKit


</br>

## 4. 핵심 기능
이 서비스의 핵심 기능은 네이버 웹툰을 사용자 편의에 맞게 조회시키는 것..?임
구현할 수 있는 뷰이벤트를 최대한 구현하였음

<details>
<summary><b>펼치기</b></summary>
<div markdown="1">

### 4.1. 화면 흐름
![](스토리보드 이미지 첨부)

### 4.2. API 조회
- **앱 최초 로드 시 네이버 책&영화 OPEN API를 조회하여 데이터를 구성했습니다.**
  - 책 링크
  - 북 링크
  
- **URL 통신** :pushpin: [코드 확인](링크)
  - Entities 형식에 맞게 파싱

- **User Default로 저장** :pushpin: [코드 확인](링크)
  - 로컬데이터 사용. 두번째 사용부터 로딩 없음

### 4.3. ViewController

- **오토 레이아웃 - SnapKit** :pushpin: [코드 확인]()
  - 스넵킷을 이용하여 화면 레이아웃 구성

- **적합한, 다양한 뷰컨트롤러 이용?** :pushpin: [코드 확인]()
  - ㅎㅎ.... 콜렉션뷰.. 테이블뷰.. 어쩌고
  - 재사용을 고려하여..

### 4.4. Service


### 4.5. Repository


</div>
</details>

</br>

## 5. 핵심 트러블 슈팅
### 5.1. 메인 레이아웃 구성 문제
- 메인 화면에 한 요일당 여러개의 콜렉션뷰가 들어감. 이를 요일별로 데이터만 바꾸어 레이아웃을 재사용해야함(화면 구성을 보고 어떤게 필요하겠다 생각)

- 콜렉션뷰+콜렉션뷰+콜렉션뷰 ... 등 복잡하게 콜렉션뷰를 구성

- 콜렉션뷰(컴포지션)+페이지뷰컨트롤러 조합하여 구성

<details>
<summary><b>기존 코드</b></summary>
<div markdown="1">

~~~java

~~~
  
</div>
</details>
- 설명
- 설명

<details>
<summary><b>개선된 코드</b></summary>
<div markdown="1">
  
~~~java

~~~
  
</div>
</details>

</br>

## 6. 그 외 트러블 슈팅
<details>
<summary>스트키 메뉴</summary>
<div markdown="1">
- 조가태요
</div>
</details>

<details>
<summary>ElementUI input 박스에서 `v-on:keyup.enter="메소드명"`이 정상 작동 안하는 문제</summary>
<div markdown="1">
  - `v-on:keyup.enter.native=""` 와 같이 .native 추가로 해결
</div>
</details>

<details>
<summary>써치바 커스텀</summary>
<div markdown="1">
  
  ```jsx
    $ npm run dev
    npm ERR! path C:\Users\integer\IdeaProjects\pilot\package.json
    npm ERR! code ENOENT
    npm ERR! errno -4058
    npm ERR! syscall open
    npm ERR! enoent ENOENT: no such file or directory, open 'C:\Users\integer\IdeaProjects\pilot\package.json'
    npm ERR! enoent This is related to npm not being able to find a file.
    npm ERR! enoent
    npm ERR! A complete log of this run can be found in:
    npm ERR!     C:\Users\integer\AppData\Roaming\npm-cache\_logs\2019-02-25T01_23_19_131Z-debug.log
  ```
  
  - 단순히 npm run dev/build 명령을 입력한 경로가 문제였다.

</details> 
  
<details>
<summary> 구글 로그인으로 로그인한 사용자의 정보를 가져오는 방법이 스프링 2.0대 버전에서 달라진 것</summary>
<div markdown="1">
  
  - 1.5대 버전에서는 Controller의 인자로 Principal을 넘기면 principal.getName(0에서 바로 꺼내서 쓸 수 있었는데, 2.0대 버전에서는 principal.getName()의 경우 principal 객체.toString()을 반환한다.
    - 1.5대 버전에서 principal을 사용하는 경우
    - 아래와 같이 사용했다면,
    ```jsx
    @RequestMapping("/sso/user")
    @SuppressWarnings("unchecked")
    public Map<String, String> user(Principal principal) {
        if (principal != null) {
            OAuth2Authentication oAuth2Authentication = (OAuth2Authentication) principal;
            Authentication authentication = oAuth2Authentication.getUserAuthentication();
            Map<String, String> details = new LinkedHashMap<>();
            details = (Map<String, String>) authentication.getDetails();
            logger.info("details = " + details);  // id, email, name, link etc.
            Map<String, String> map = new LinkedHashMap<>();
            map.put("email", details.get("email"));
            return map;
        }
        return null;
    }
    ```
    - 2.0대 버전에서는
    - 아래와 같이 principal 객체의 내용을 꺼내 쓸 수 있다.
    ```jsx
    UsernamePasswordAuthenticationToken token =
                    (UsernamePasswordAuthenticationToken) SecurityContextHolder
                            .getContext().getAuthentication();
            Map<String, Object> map = (Map<String, Object>) token.getPrincipal();
            String email = String.valueOf(map.get("email"));
            post.setMember(memberRepository.findByEmail(email));
    ```
        
</div>
</details> 
    
</br>
  
## 6. 회고 / 느낀점
>프로젝트 개발 회고 글:
>디자인이 다 나와있는데 왜 안대누..
