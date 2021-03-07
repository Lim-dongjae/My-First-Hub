#아이폰 개발 시작하기

#주제 - iOS 앱 개발 #패스트 캠퍼스 

---
  
10강 스위프트 __MVVM 디자인 패턴__

## Design Pattern?
### 코딩을 유지보수하기 쉽게 관리하기 위한 패턴.
#### 디자인 패턴의 목적
#### 1) 기술부채 최소화
#### 2) 재사용 및 지속 가능한 코드 작성

## - MVC Pattern : Model - View - Controller (MVVM 패턴 등장 전까지 가장 많이 사용된 패턴)
Model : 데이터 (Struct)
View : UI요소 (UIView)
Controller : 중계자 (UIViewController)

## - MVVM Pattern : Model - View - ViewModel (가장 많이 사용되는 디자인 패턴)
Model : 데이터 (Struct)
View : UI요소 (UIView, UIViewController)
ViewModel : 중계자 (ViewModel)

## MVC 와 MVVM 의 차이점
#### - MVVM은 뷰컨트롤러가 모델에 직접 접근하지 못한다
#### - MVVM은 뷰컨트롤러가 뷰모델이라는 새로운 클래스를 갖게되었다
#### - 뷰컨트롤러 자체가 MVC패턴에서는 컨트롤러 레이어에 있었지만 MVVM패턴에서는 뷰레이어에 있게되었다.

## MVVM 개선점
#### - 기존 MVC에서 뷰컨트롤러가 지나치게 많은 부분을 차지하고 있으니 그 부분을 축소시켰다.
#### - 많은 일을 뷰모델로 위임했기 때문에 각자의 할 일이 명확해졌다.
#### - 클래스는 할 일이 명확할 수록 수정이 용의하고 유지보수에 적은 비용이 든다.

## 리팩터링
#### - 기술부채를 줄이고 재사용 가능하면서 유지보수가 쉽도록 코드를 수정하는 과정
#### - 코드안에 있는 구조를 바꾸는 과정이기 떄문에 유저 입장에서는 차이를 느끼지 못한다.

### 리팩터링의 중요한 원칙들
#### - 중복 제거
#### - 단일 책임 갖기
#### - 10, 200 rule (메서드는 10줄안에, 클래스는 200줄안에 코드 짜기)
#### - 위의 룰은 사실 지키기 힘들다 따라서 30, 400 rule을 최대한 지켜보도록 노력하자


## MVVM 패턴에 맞게 현상금 앱을 리팩터링 해보자.

### 기존 코드
```Swift
// BountyViewController
class BountyViewController: UIViewController , UITableViewDataSource, UITableViewDelegate{
    
    let nameList = ["brook", "chopper", "franky", "luffy", "nami", "robin", "sanji", "zoro"]
    let bountyList = [330000000, 50, 440000000, 30000000, 1600000, 8000000, 7700000, 1200000]

    // 세그웨이 수행하기 직전에 준비하는 메서드
    override func prepare(for segue: UIStoryboardSegue, sender: Any?) {
        // DetailViewController에 데이터를 넘겨보자
        if segue.identifier == "showDetail" {
            let vc = segue.destination as? DetailViewController
            if let index = sender as? Int {
                vc?.name = nameList[index]
                vc?.bounty = bountyList[index]
            }
        }
    }
    
    override func viewDidLoad() {
        super.viewDidLoad()
    }
    
    // UITableViewDataSource

    func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        return bountyList.count
    }
    
    func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
        
        guard let cell = tableView.dequeueReusableCell(withIdentifier: "cell", for: indexPath) as? ListCell else {
            return UITableViewCell()
        }
        let img = UIImage(named: "\(nameList[indexPath.row]).jpg")
        cell.imgView.image = img
        cell.nameLabel.text = nameList[indexPath.row]
        cell.bountyLabel.text = "\(bountyList[indexPath.row])"
        return cell
    }
    
    // UITableViewDelegate
    func tableView(_ tableView: UITableView, didSelectRowAt indexPath: IndexPath) {
        print("--> \(indexPath.row)")
        performSegue(withIdentifier: "showDetail", sender: indexPath.row)
    }
}


// Custom Cell
class ListCell: UITableViewCell {
    @IBOutlet weak var imgView: UIImageView!
    @IBOutlet weak var nameLabel: UILabel!
    @IBOutlet weak var bountyLabel: UILabel!
}


// DetailViewController
class DetailViewController: UIViewController {
    
    @IBOutlet weak var imgView: UIImageView!
    @IBOutlet weak var nameLabel: UILabel!
    @IBOutlet weak var bountyLabel: UILabel!
    
    var name: String?
    var bounty: Int?

    override func viewDidLoad() {
        super.viewDidLoad()
        updateUI()
    }
    
    func updateUI() {
        if let name = self.name, let bounty = self.bounty {
        let img = UIImage(named: "\(name).jpg")
        imgView.image = img
        nameLabel.text = name
        bountyLabel.text = "\(bounty)"
        }
        
    }


    @IBAction func close(_ sender: Any) {
        dismiss(animated: true, completion: nil)
    }
}
```

### MVVM 패턴을 적용한 리팩터링 후 코드
```Swift
// BountyViewController
class BountyViewController: UIViewController , UITableViewDataSource, UITableViewDelegate{
    
    // MVVM
    
    // Model
    // - BountyInfo
    // > BountyInfo 만들기
    
    // View
    // - ListCell
    // > ListCell 필요한 정보를 ViewModel한테서 받기
    // > ListCell은 ViewModel로 부터 받은 정보로 뷰 업데이트 하기
    
    // ViewModel
    // - BountyViewModel
    // > BountyViewModel를 만들고, 뷰레이어에서 필요한 메서드 만들기
    // > 모델 가지고 있기 ,, BountyInfo 들
    
    
    //아래 코드를 대체해보자
//    let nameList = ["brook", "chopper", "franky", "luffy", "nami", "robin", "sanji", "zoro"]
//    let bountyList = [330000000, 50, 440000000, 30000000, 1600000, 8000000, 7700000, 1200000]
    
//    let bountyInfoList: [BountyInfo] = [
//        BountyInfo(name: "brook", bounty: 33000000),
//        BountyInfo(name: "chopper", bounty: 50),
//        BountyInfo(name: "franky", bounty: 44000000),
//        BountyInfo(name: "luffy", bounty: 300000000),
//        BountyInfo(name: "nami", bounty: 16000000),
//        BountyInfo(name: "robin", bounty: 80000000),
//        BountyInfo(name: "sanji", bounty: 77000000),
//        BountyInfo(name: "zoro", bounty: 120000000)
//    ] 이 대체된 코드는 뷰모델 클래스로 이동해준다.
    
    let viewModel = BountyViewModel()

    // 세그웨이 수행하기 직전에 준비하는 메서드
    override func prepare(for segue: UIStoryboardSegue, sender: Any?) {
        // DetailViewController에 데이터를 넘겨보자
        if segue.identifier == "showDetail" {
            let vc = segue.destination as? DetailViewController
            if let index = sender as? Int {
                
                //아래 코드를 대체한다
//                vc?.name = nameList[index]
//                vc?.bounty = bountyList[index]
                
//                let bountyInfo = bountyInfoList[index]
                let bountyInfo = viewModel.bountyInfo(at: index)
                vc?.viewModel.update(model: bountyInfo)
//                vc?.name = bountyInfo.name
//                vc?.bounty = bountyInfo.bounty
            }
        }
    }
    
    override func viewDidLoad() {
        super.viewDidLoad()
    }
    
    // UITableViewDataSource

    func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
//        return bountyList.count 이 코드를 대체한다
//        return bountyInfoList.count
        return viewModel.numOfBountyInfoList
    }
    
    func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
        
        guard let cell = tableView.dequeueReusableCell(withIdentifier: "cell", for: indexPath) as? ListCell else {
            return UITableViewCell()
        }
//        let img = UIImage(named: "\(nameList[indexPath.row]).jpg")
//        cell.imgView.image = img
//        cell.nameLabel.text = nameList[indexPath.row]
//        cell.bountyLabel.text = "\(bountyList[indexPath.row])"
        // 위 코드를 대체한다
        
//        let bountyInfo = bountyInfoList[indexPath.row]
        
        let bountyInfo = viewModel.bountyInfo(at: indexPath.row)
        cell.update(info: bountyInfo)
//        cell.imgView.image = bountyInfo.image
//        cell.nameLabel.text = bountyInfo.name
//        cell.bountyLabel.text = "\(bountyInfo.bounty)"
        // 위 코드는 뷰컨트롤러안에 있을 필요 없이 셀에서 바로 실행하면되기 때문에 IBOutlet 코드 안으로 옮긴다.
        return cell
    }
    
    // UITableViewDelegate
    func tableView(_ tableView: UITableView, didSelectRowAt indexPath: IndexPath) {
        print("--> \(indexPath.row)")
        performSegue(withIdentifier: "showDetail", sender: indexPath.row)
    }
}


// Custom Cell
class ListCell: UITableViewCell {
    @IBOutlet weak var imgView: UIImageView!
    @IBOutlet weak var nameLabel: UILabel!
    @IBOutlet weak var bountyLabel: UILabel!
    
    func update(info: BountyInfo) {
        imgView.image = info.image
        nameLabel.text = info.name
        bountyLabel.text = "\(info.bounty)"
    }
}


struct BountyInfo {
    let name: String
    let bounty: Int
    
    var image: UIImage? {
        return UIImage(named: "\(name).jpg")
    }
    
    init(name: String, bounty: Int) {
        self.name = name
        self.bounty = bounty
    }
}


// viewModel 클래스 만들기
class BountyViewModel {
    let bountyInfoList: [BountyInfo] = [
        BountyInfo(name: "brook", bounty: 33000000),
        BountyInfo(name: "chopper", bounty: 50),
        BountyInfo(name: "franky", bounty: 44000000),
        BountyInfo(name: "luffy", bounty: 300000000),
        BountyInfo(name: "nami", bounty: 16000000),
        BountyInfo(name: "robin", bounty: 80000000),
        BountyInfo(name: "sanji", bounty: 77000000),
        BountyInfo(name: "zoro", bounty: 120000000)
    ]
    
    // 이렇게 옮기고 나면 기존 코드들에서 오류가 발생한다.
    // 그 부분을 뷰모델 클래스에서 메서드로 만들어줘야한다.
    
    var numOfBountyInfoList: Int {
        return bountyInfoList.count
    }
    
    func bountyInfo(at index: Int) -> BountyInfo{
        return bountyInfoList[index]
    }
}


// DetailViewConvtroller
class DetailViewController: UIViewController {
    
    
    // MVVM
    
    // Model
    // - BountyInfo
    // > BountyInfo 만들기
    
    // View
    // - imgView, nameLabel, ListCell
    // > View들은 viewModel를 통해서 구성되기
    
    // ViewModel
    // - DetailViewModel
    // > 뷰레이어에서 필요한 메서드 만들기
    // > 모델 가지고 있기 ,, BountyInfo 들
    
    
    @IBOutlet weak var imgView: UIImageView!
    @IBOutlet weak var nameLabel: UILabel!
    @IBOutlet weak var bountyLabel: UILabel!
    
//    var name: String?
//    var bounty: Int?
    
//    var bountyInfo: BountyInfo? 이 친구는 DetailViewModel 클래스로 이동한다.
    
    //뷰모델 선언
    let viewModel = DetailViewModel()

    override func viewDidLoad() {
        super.viewDidLoad()
        updateUI()
    }
    
    func updateUI() {
        if let bountyInfo = viewModel.bountyInfo {
            imgView.image = bountyInfo.image
            nameLabel.text = bountyInfo.name
            bountyLabel.text = "\(bountyInfo.bounty)"
        }
        
//        if let bountyInfo = self.bountyInfo {
//            imgView.image = bountyInfo.image
//            nameLabel.text = bountyInfo.name
//            bountyLabel.text = "\(bountyInfo.bounty)"
//        } 이 코드를 위와 같이 바꾼다.
        
        // 아래 코드를 위처럼 바꾼다.
//        if let name = self.name, let bounty = self.bounty {
//        let img = UIImage(named: "\(name).jpg")
//            imgView.image = img
//            nameLabel.text = name
//            bountyLabel.text = "\(bounty)"
//        }
    }


    @IBAction func close(_ sender: Any) {
        dismiss(animated: true, completion: nil)
    }
}

class DetailViewModel {
    var bountyInfo: BountyInfo?
    
    func update(model: BountyInfo?) {
        bountyInfo = model
    }
}
```

### 위와 같이 리팩토링 후에는 기존 잔재들을 지워주는 것이 좋다.
### (지금은 참고를 위하여 잔재를 남겨두었다)
이해가 되지 않는 점.
1) 분명 위에서는 단일 책임을 갖도록 해줘야한다고 했는데
이번 시간에 배운 것은 책임을 분산시키는 일이였다.
이 부분은 다음 시간에 다시 알아보도록하자.


## 보다 깔끔한 코드 정리를 위하여
### 뷰모델에 많은 클래스들이 존재하면 헷갈릴 가능성이 있기 때문에
### 스트럭트를 따로 관리해준다.
```Swift
// 방법
// 새로운 파일 - 코코아 터치 클래스 - NSObject - 코드 복사 - 기존 코드 삭제

struct BountyInfo {
    let name: String
    let bounty: Int
    
    var image: UIImage? {
        return UIImage(named: "\(name).jpg")
    }
    
    init(name: String, bounty: Int) {
        self.name = name
        self.bounty = bounty
    }
}
```

## 현상금 랭킹대로 나열하기
### 아래 코드처럼 간단하게 코드 몇줄 작성과 변경으로
### 앱 전반적인 기능을 수정할 수 있게되었다.
### 디자인 모델을 이용하여 리팩토링하면 이와 같이
### 쉽게 유지관리가 가능하다.

```Swift
// viewModel 클래스 만들기
class BountyViewModel {
    let bountyInfoList: [BountyInfo] = [
        BountyInfo(name: "brook", bounty: 33000000),
        BountyInfo(name: "chopper", bounty: 50),
        BountyInfo(name: "franky", bounty: 44000000),
        BountyInfo(name: "luffy", bounty: 300000000),
        BountyInfo(name: "nami", bounty: 16000000),
        BountyInfo(name: "robin", bounty: 80000000),
        BountyInfo(name: "sanji", bounty: 77000000),
        BountyInfo(name: "zoro", bounty: 120000000)
    ]
    // 이렇게 옮기고 나면 기존 코드들에서 오류가 발생한다.
    // 그 부분을 뷰모델 클래스에서 메서드로 만들어줘야한다.
    
    // 랭킹 나열을 위한 리팩토링
    var sortedList: [BountyInfo] {
        let sortedList = bountyInfoList.sorted { prev, next in
            return prev.bounty > next.bounty
            // 프리뷰 아이템에 앞에있는 바운티가 뒤에있는 바운티의 현상금보다 커야한다.
        }
        return sortedList
    }
    
    var numOfBountyInfoList: Int {
        return bountyInfoList.count
    }
    
    func bountyInfo(at index: Int) -> BountyInfo{
//        return bountyInfoList[index]
        // 랭킹 나열을 위한 리팩토링
        return sortedList[index]
    }
}
```