# :pushpin: 온라인 강의 실습 프로젝트
>강의 정보: [[패스트캠퍼스]30개 프로젝트로 배우는 iOS 앱 개발 with Swift 초격차 패키지 Online](https://fastcampus.co.kr/dev_online_iosappfinal) <br>
>실습 코드: [git](https://github.com/oneoneoneoneoneoneone/fc)

</br>

## 1. 제작 기간 & 참여 인원
- 2022.03 ~ 2023.02
- 개인 프로젝트


</br>


## 2. 사용 기술
- UIKit / SwiftUI / RxSwift / XCTest
- SnapKit / Alamofire / Firebase


</br>


## 3. 주 프로젝트

### 3.1. 실시간 공지사항 팝업 만들기
- 화면
![Simulator Screen Shot - iPhone 14 - 2023-02-15 at 02 22 46](https://user-images.githubusercontent.com/94464179/218811403-eeace868-3889-4fc7-a9d8-fbbce151f7b9.png)
![Simulator Screen Shot - iPhone 14 - 2023-02-15 at 02 22 53](https://user-images.githubusercontent.com/94464179/218811412-c4a4d5b9-a6e0-4793-8a1b-8487f3fed1ec.png)



- 기능: 단순 공지사항 팝업을 띄우는 기능을 가진 앱으로, Firebase Remote Config와 연결하여 실시간으로 공지 내용을 변경할 수 있습니다.

<details>
<summary>핵심 코드 or 코드 흐름</summary>
<div markdown="1">
  
  - Firebase Remote Config를 추가
  
~~~swift
    let component = Calendar.current.dateComponents([.hour, .minute], from: alert.date)
    let trigger = UNCalendarNotificationTrigger(dateMatching: component, repeats: alert.isOn)
    let request = UNNotificationRequest(identifier: alert.id, content: content, trigger: trigger)
~~~
  
</div>
</details>

<details>
<summary>Firebase A-B Test 적용 코드</summary>

~~~swift
  
~~~
  
</details>


### 3.2. 물마시기 알람 앱 만들기
- 화면

- 핵심 기능: 물마실 시간 알림을 추가하고, NotificationCenter에서 알림을 받아볼 수 있습니다. 

<details>
<summary>코드 흐름</summary>
<div markdown="1">
  
  - 알림추가 화면에서 리스트뷰로 데이터 넘기기
  ~~~swift
  //AddAlertViewController
  
    var pickedDate: ((_ date: Date,_ isRepeat: Bool, _ duration: Double) -> Void)? 

    @IBAction func saveButtonTap(_ sender: UIBarButtonItem) {
      pickedDate?(datePicker.date, isRepeatSwitch.isOn, datePicker.date.timeIntervalSinceNow + timePicker.countDownDuration)

      self.dismiss(animated: true, completion: nil)
    }
  ~~~
  
  - 테이블뷰에서 추가된 알림을 UNUserNotificationCenter에 넘기기
  ~~~swift
  //AlertListViewController
  
    addAlertVC.pickedDate = {[weak self] date, isRepeat, duration in
      guard let self = self else {return}

      let newAlert = Alert(date: date, isOn: true, isRepeat: isRepeat, duration: duration)

      ... //테이블뷰 데이터 업데이트 및 정렬, 내부저장소 저장

      //센터에 알림을 추가하는 메소드 호출
      self.userNotificationCenter.addNotificationRequest(by: newAlert)
    }
  ~~~
  
  - UNUserNotificationCenter에 알림 추가
  ~~~swift
  //UNUserNotificationCenter
  
    let content = UNMutableNotificationContent()
    ... //content 설정

    //UNCalendarNotificationTrigger - 시간 알림
    let component = Calendar.current.dateComponents([.hour, .minute], from: alert.date)
    let trigger = UNCalendarNotificationTrigger(dateMatching: component, repeats: alert.isOn)
    let request = UNNotificationRequest(identifier: alert.id, content: content, trigger: trigger)
    self.add(request, withCompletionHandler: nil)
  
    //UNTimeIntervalNotificationTrigger - 타이머 알림 (다시알림)
    let timeTrigger = UNTimeIntervalNotificationTrigger(timeInterval: alert.duration, repeats: false)
    let timeRequest = UNNotificationRequest(identifier: alert.id, content: content, trigger: timeTrigger)
    self.add(timeRequest, withCompletionHandler: nil)
  ~~~
  
  - 사용자 알림 승인
  ~~~swift 
  //AppDelegate
  
    let authrizationOptions = UNAuthorizationOptions(arrayLiteral: [.alert, .badge, .sound])
    userNotificationCenter.requestAuthorization(options: authrizationOptions){_, error in
      if let error = error{
        print("ERROR: \(error)")
      }
    }
  ~~~
  
</div>
</details>


### 3.3. 재난문자 푸시 알림 구현하기

- 핵심기능)

<details>
<summary><b>FCM(Firebase Cloud Messaging) 설정</b></summary>
  
~~~swift

~~~
  
</details>


### 3.4. 다음 카페/블로그 검색앱 만들기

- 핵심기능)
- API통신 코드
- rx 데이터 처리 코드


### 3.5. 내 근처 편의점 찾기 앱

- 핵심기능)
- DaumMap 처리 메소드
- Rx로 사용시 커스텀한 코드

- 문제해결)
- DaumMap API 오류시 처리 


### 3.6. 도서리뷰 앱 만들기

- 핵심기능)
- MVP 패턴
- XCTest / Unit Test 코드 작성


</br>

  
## 4. 회고 / 느낀점
>프로젝트 개발 회고 글:
>디자인이 다 나와있는데 왜 안대누..
