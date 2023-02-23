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
- 화면간 데이터 이동. Notification!!!

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

- 페이지뷰컨트롤러의 뷰컨트롤러들이 각각 데이터 갯수가 달라 높이값도 다른데, 페이지뷰컨트롤러에 고정 높이를 설정하게되면 일부 페이지들이 바닥이 잘려보이는 문제가 있었습니다.
  <details>
  <summary><b>개선된 코드</b></summary>
  <div markdown="1">

    - 현재 선택 된 pageViewController의 viewController가 변경되어 화면에 띄울 때(bind)마다 viewController의 높이에 맞게 pageViewController 높이가 변경되도록 했습니다. 
    ~~~Swift
    //MainViewController
      //선언
      private var pageViewHeightConstraint: NSLayoutConstraint?

      //컨트롤 레이아웃 설정
      private func setLayout(){
          dayPageViewController.view.snp.makeConstraints{
              $0.top.equalTo(dayCollectionView.snp.bottom).offset(1)
              $0.leading.trailing.bottom.equalToSuperview()
          }
          pageViewHeightConstraint =    pageViewController.view.heightAnchor.constraint(equalToConstant: 250)
          pageViewHeightConstraint?.isActive = true
    }
      //현재 페이지가 변경될 때 각 컨트롤(페이지뷰, 콜렉션뷰)에 업데이트
      private func bind(pageViewController: UIPageViewController, oldValue: Int, newValue: Int, animated: Bool){
          pageViewHeightConstraint?.constant = CGFloat(3*60 + 210)
    }
    ~~~

  </div>
  </details>


- 스크롤을 일정범위까지 내리지 않으면 내려오던 네비게이션바가 다시 위로 숨겨져야하고, 스크롤을 완전히 내렸을 때 요일컬렉션뷰가 네비게이션바 바로 아래에 고정되어야했습니다.
  <details>
  <summary><b>개선된 코드</b></summary>
  <div markdown="1">

    - 콜렉션뷰를 스크롤을 할때 호출되는 Delegate 메소드를 사용해 scrollView.contentOffset.y값을 기준으로 콜렉션뷰의 y위치값과 네비게이션바의 y위치값을 조정해 구현했습니다.
    ~~~Swift
    //MainViewController
      extension MainViewController: UIScrollViewDelegate{
          //스크롤이 될 때마다 호출되는 메소드
          func scrollViewDidScroll(_ scrollView: UIScrollView) {
              //스크롤 시 상위여백이 없도록 고정
              if scrollView.contentOffset.y <= 0{
                  scrollView.contentOffset.y = 0

                  return
              }

              //스크롤을 일정범위 이상 내렸을 때, dayCollectionView를 네비게이션바 바로 아래에 고정
              if scrollView.contentOffset.y > 200 - view.safeAreaInsets.top{
                  dayCollectionView.frame.origin.y = scrollView.contentOffset.y + view.safeAreaInsets.top

                  let nv = self.navigationController as! MainNavigationView

                  nv.setNavigationViewHidden(hidden: false)
              }
              else{
                  dayCollectionView.frame.origin.y = 200// - statusBarHeight

                  let nv = self.navigationController as! MainNavigationView

                  nv.setNavigationViewHidden(hidden: true)
              }
              //-statusBarMargin) navigationBar를 숨기지 않고 안보이게하는 y 값
              var headerConstant = scrollView.contentOffset.y - statusBarMargin

              headerConstant = headerConstant > view.safeAreaInsets.top - statusBarMargin ? view.safeAreaInsets.top - statusBarMargin : (headerConstant - Const.Size.HeaderMinHeight)

              self.navigationController?.navigationBar.layer.position.y = headerConstant// < -15 ? -22 : headerConstant
          }
      }
    ~~~

  </div>
  </details>

  
### 5.2. 라벨에 강조속성 주기

- 콜렉션뷰의 헤더 라벨에 닉네임이나 강조 부분만 볼드체 or 색상속성을 주어 강조해야했습니다.
  <details>
  <summary><b>개선된 코드</b></summary>
  <div markdown="1">

    - 데이터는 하나인데 라벨을 여러개로 나눠야하는줄 알았으나, NSMutableAttributedString을 적용하여 간단하게 구현했습니다.
    ~~~Swift
    //MainDayPageViewController
      let defaultAttributes: [NSAttributedString.Key: Any] = [
              .foregroundColor: UIColor.label,
              .font: UIFont.systemFont(ofSize: 16, weight: .regular)
          ]
          let boldAttributes: [NSAttributedString.Key: Any] = [
              .foregroundColor: UIColor.label,
              .font: UIFont.systemFont(ofSize: 16, weight: .semibold)
          ]

      let text = NSMutableAttributedString(string: "\(id)님, 이 웹툰들\n최신 이야기를 놓치고 계신 것 같아요!", attributes: defaultAttributes)
      text.addAttributes(boldAttributes, range: NSRange(location: 0, length: id.count))
      text.addAttributes(boldAttributes, range: NSRange(location: text.length - 13,
    ~~~

  </div>
  </details>

  
### 5.3. 서치바의 스코프바 위치 지정

- 서치바컨트롤러의 스코프바를 사용했을 때, 취소버튼이 서치바 옆이 아닌 두 객체 사이의 옆에 위치하는 문제가 있었습니다.
    - 서치바를 커스텀으로 만들었습니다.
    - 스코프바에 해당하는 컨트롤은 UISegmentedControl로 구현했습니다.
    - CustomSearchViewController.swift 코드 확인: [🔗](https://github.com/oneoneoneoneoneoneone/NaverWebtoonCloneCoding/blob/main/NaverWebtoonCloneCoding/Scene/CustomUI/CustomSearchViewController.swift)
  
  
### 5.4. 상세화면 버튼 색깔

- 상세화면에서 +관심버튼의 색깔을 적용해야하는데 배경이되는 이미지별로 어울리는 색깔을 써야했습니다.
  <details>
  <summary><b>개선된 코드</b></summary>
  <div markdown="1">
    
    - 이미지의 특정 위치의 색깔을 따서 버튼의 색깔을 적용하고, 버튼의 라벨은 그를 반전시킨 값을 적용했습니다.
    ~~~Swift
    //DetailViewController
      backgroundView.backgroundColor = imageView.image?.getPixelColor(pos: CGPoint(x: 0, y: 0))
      likeLabel.layer.backgroundColor = imageView.image?.getPixelColor(pos: CGPoint(x: 0, y: 0)).cgColor
    ~~~

    - extension UIImage 코드 확인: [🔗](https://github.com/oneoneoneoneoneoneone/NaverWebtoonCloneCoding/blob/main/NaverWebtoonCloneCoding/Scene/CustomUI/UIImage.swift)

  </div>
  </details>
  
  
</br>

  
## 6. 회고 / 느낀점
>프로젝트 개발 회고 글:
>디자인이 다 나와있는데 왜 안대누..
