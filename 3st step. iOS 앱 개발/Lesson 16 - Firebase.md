#아이폰 개발 시작하기

#주제 - iOS 앱 개발 #패스트 캠퍼스 

---
 
16강 스위프트 __Firebase__


## Firebase
### 서버를 제공해주는 서비스
#### 1. 데이터 저장
#### 2. 실시간 데이터 동기화
#### 3. 사용자 인증
#### 4. 데이터 분석
#### 5. A/B Testing

### Firebase에서 제공하는 3가지 큰 기능
#### 1. Build better apps
- 제공해주는 서비스를 사용하여 굉장히 빠르고 쉽게 개발할 수 있도록 도와준다.

#### 2. Improve app quality
- 앱의 퀄리티를 관리해주는 기능을 제공해준다.

#### 3. Grow your app
- 비지니스 성장을 도모하는 기능을 제공해준다.


## CocoaPods
#### 외부 라이브러리 관리 모듈

### Cocoapods 설치 - Firebase 콘솔로 접속 - Pod init - Pod install - 사용


## 데이터베이스에서 데이터 가져오기
### Firebase 콘솔 - RealtimeDatabase - 데이터베이스 생성 - Swift Pod 파일에 입력 - 터미널에서 Podinstall - 데이터베이스접속 - 생성
### Swift에 inport
```Swift
// 데이터베이스에서 데이터 가져오기
import UIKit
import Firebase

class ViewController: UIViewController {
    
    let db = Database.database().reference()
    @IBOutlet weak var dataLabel: UILabel!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view.
        updateLabel()
    }
    
    func updateLabel() {
        db.child("firstData").observeSingleEvent(of: .value) { snapshot in
            print("--> \(snapshot)")
            
            let value = snapshot.value as? String ?? ""
            
            DispatchQueue.main.async {
                self.dataLabel.text = value
            }
        }
    }
}
```

## 데이터 추가하기
```Swift

```