#아이폰 개발 시작하기

#주제 - iOS 앱 개발 #패스트 캠퍼스 

---
 
9강 스위프트 __원피스 현상금 랭킹 앱 만들기__

## 테이블뷰 기초개념

1. 테이블 뷰를 사용하는 이유
- 여러 아이템을 리스트 형태로 보여주고 싶을 때 사용한다.

2. 테이블 뷰가 질문함
- 1) 테이블 뷰의 셀은 몇개인가?
- 2) 테이블 뷰 어떻게 보여주나?
- 3) 테이블 뷰 클릭하면 어떻게 반응하나?
- ...

-> 우리는 UITableView를 통해 데이터를 보여줄 때
테이블 뷰에서 질문한 질문중에 최소한 1,2번은 답해줘야한다.

### 테이블 뷰가 질문한 내용을 답하는 방법
```Swift

// 아래 클래스에서 UITableViewDataSource 이부분이
// 테이블 뷰의 셀은 몇개인지?
// 테이블 뷰를 어떻게 보여줄 것인지에 대한 답변을 위한 코드이다.
class BountyViewController: UIViewConvtroller, UITableViewDataSource, UITableViewDelegate {
// 또한 UITableViewDataSource 와 UITableViewDelegate는
// 테이블 뷰를 클릭하면 어떻게 반응할 것인지를 물어보는 질문에 답변을 하기 위한 코드이다.
    ....
}
```

## 테이블 뷰 만들어보기
```Swift
class BountyViewController: UIViewController , UITableViewDataSource, UITableViewDelegate{

    
    override func viewDidLoad() {
        super.viewDidLoad()

        // Do any additional setup after loading the view.
    }
    
    // UITableViewDataSource

    func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        return 5
    }
    
    func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
        
        let cell = tableView.dequeueReusableCell(withIdentifier: "cell", for: indexPath)
        
        return cell
    }
    
    // UITableViewDelegate
    func tableView(_ tableView: UITableView, didSelectRowAt indexPath: IndexPath) {
        print("--> \(indexPath.row)")
    }
}
```

## Custom Cell
```Swift
class BountyViewController: UIViewController , UITableViewDataSource, UITableViewDelegate{
    
    let nameList = ["brook", "chopper", "franky", "luffy", "nami", "robin", "sanji", "zoro"]
    let bountyList = [330000000, 50, 440000000, 3000000, 1600000, 8000000, 7700000, 1200000]

    
    override func viewDidLoad() {
        super.viewDidLoad()

        // Do any additional setup after loading the view.
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
    }
}


// Custom Cell
class ListCell: UITableViewCell {
    @IBOutlet weak var imgView: UIImageView!
    @IBOutlet weak var nameLabel: UILabel!
    @IBOutlet weak var bountyLabel: UILabel!
}
```