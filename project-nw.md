# :pushpin: 클론코딩
>네이버웹툰 클론코딩  
>전체 코드: [🔗](https://github.com/oneoneoneoneoneoneone/NaverWebtoonCloneCoding)

</br>

## 1. 제작 기간 & 참여 인원
- 2022.07 ~ 2022.12
- 개인 프로젝트

</br>

## 2. 사용 기술
- UIKit / SnapKit

</br>


## 3. 핵심 기능
이 서비스의 핵심 기능은 네이버 웹툰을 사용자 편의에 맞게 조회시키는 것..?임
구현할 수 있는 뷰이벤트를 최대한 구현하였음

<details>
<summary><b>펼치기</b></summary>
<div markdown="1">

  
### 3.1. 화면
  
|<img src="https://user-images.githubusercontent.com/94464179/220561401-e56a2766-7b70-478c-bbf8-3a6bcd63c37e.png" width="90%" height="90%" alt>|<img src="https://user-images.githubusercontent.com/94464179/220561408-29ae03e1-9fcc-4c8e-b8d3-44d9be4c4730.png" width="90%" height="90%" alt>|<img src="https://user-images.githubusercontent.com/94464179/220561412-e0ddcb22-991f-46d3-aa17-d6b9050ad111.png" width="90%" height="90%" alt>|<img src="https://user-images.githubusercontent.com/94464179/220561422-91dc1c58-e4ab-4963-bc9a-b16ca549e0d6.png" width="90%" height="90%" alt>|
|:--:|:--:|:--:|:--:|
| *웹툰(메인)* | *추천완결* | *베스트도전* | *My* |
|<img src="https://user-images.githubusercontent.com/94464179/220561419-8d1abd98-730b-4ff6-a096-c770cf7ef22f.png" width="90%" height="90%" alt>|<img src="https://user-images.githubusercontent.com/94464179/220561427-cd08f77e-cb39-4b7b-abfa-879b33011928.png" width="90%" height="90%" alt>|<img src="https://user-images.githubusercontent.com/94464179/220561429-4782190d-d739-467b-b482-449d8c211b02.png" width="90%" height="90%" alt>|
| *더보기* | *검색* | *상세* |

  
### 3.2. View

- **오토 레이아웃**
  - SnapKit 라이브러리를 사용하여 화면 레이아웃을 구성했습니다.
  
- **다양한 뷰컨트롤러 사용**
  - 웹툰(메인)화면은 요일별로 같은 콜렉션뷰를 재사용하여 다른 데이터를 보여주기위해 UIPageViewController를 사용했습니다.
  - 웹툰(메인)화면은 여러 형태의 콜렉션뷰가 한 화면에 필요한 형태로, 콜렉션뷰 레이아웃으로 UICollectionViewCompositionalLayout을 사용하여 다양한 레이아웃을 적용했습니다.
  
- **화면 이동**
  - 기본 제스쳐 사용과 화면간 이동을 자연스럽게 보여주도록 NavigationViewController의 push, pop 메소드를 사용했습니다.

  
### 3.3. API 조회
  
- 앱 최초 로드 시 네이버 검색 API로 책 정보를 조회하여 데이터를 구성했습니다.
  
- **API 통신** 
  - 서버통신은 URLSession을 사용
  ~~~Swift
    //리퀘스트 생성
    var request = URLRequest(url: url)
    request.httpMethod = "GET"
    request.setValue(APIKey().X_Naver_Client_Id, forHTTPHeaderField: "X-Naver-Client-Id")
    request.setValue(APIKey().X_Naver_Client_Secret, forHTTPHeaderField: "X-Naver-Client-Secret")

    sleep(1)
    //태스크 생성
    URLSession.shared.dataTask(with: request){[weak self] data, response, error in
      ...
    }
    .resume()
  ~~~
  
  - 테스크 내부에서 Response data에 맞춘 Books 구조체로 파싱
  ~~~Swift
    let books = try JSONDecoder().decode(Books.self, from: data)
  ~~~
  
  - LocalNetwork.swift 코드 확인: [🔗](https://github.com/oneoneoneoneoneoneone/NaverWebtoonCloneCoding/blob/main/NaverWebtoonCloneCoding/Network/LocalNetwork.swift)

  
### 3.4. Service


### 3.5. Repository

- **User Default로 저장** 
  - Repository.swift 코드 확인: [🔗](https://github.com/oneoneoneoneoneoneone/NaverWebtoonCloneCoding/blob/main/NaverWebtoonCloneCoding/Util/Repository.swift)
  
- 로컬 디비(UserDefaults)에 저장하는 데이터 목록입니다.
  - Item - 앱 최초 실행시 저장하는 작품(책) 정보
  - User - 관심작품 추가를 위한 기본 사용자 정보
  - LikeItem - 관심작품 추가 정보
  - searchLog - 검색어 기록
  <img src="https://user-images.githubusercontent.com/94464179/220568828-228645e4-0024-4978-a867-721ffb86488e.png"  width="50%" height="50%" alt>

  
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

~~~Swift

~~~
  
</div>
</details>
- 설명
- 설명

<details>
<summary><b>개선된 코드</b></summary>
<div markdown="1">
  
~~~Swift

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
  
  
  - 단순히 npm run dev/build 명령을 입력한 경로가 문제였다.

</details> 
  
<details>
<summary> 구글 로그인으로 로그인한 사용자의 정보를 가져오는 방법이 스프링 2.0대 버전에서 달라진 것</summary>
<div markdown="1">
  
  - 1.5대 버전에서는 Controller의 인자로 Principal을 넘기면 principal.getName(0에서 바로 꺼내서 쓸 수 있었는데, 2.0대 버전에서는 principal.getName()의 경우 principal 객체.toString()을 반환한다.
    - 1.5대 버전에서 principal을 사용하는 경우
    - 아래와 같이 사용했다면,
    ~~~Swift
  
    ~~~
  
    - 2.0대 버전에서는
    - 아래와 같이 principal 객체의 내용을 꺼내 쓸 수 있다.
    ~~~Swift
  
    ~~~
        
</div>
</details> 
    
</br>
  
## 6. 회고 / 느낀점
>프로젝트 개발 회고 글:
>디자인이 다 나와있는데 왜 안대누..
