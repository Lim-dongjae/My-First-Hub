#아이폰 개발 시작하기

#주제 - iOS 앱 개발 #패스트 캠퍼스 

---
 
17강 스위프트 __넷플릭스st 영상앱 - 확장__

## 스크롤이 가능한 뷰는?
#### 1. 테이블 뷰
#### 2. 컬렉션 뷰

### 스크를 뷰
#### 스크롤 뷰 - 창문(클리핑 마스크되는 박스)
#### 컨텐츠 뷰 - 배경(클리핑 마스크되는 컨텐츠)

#### 스크롤 뷰는 오토레이아웃에서 기존 컬렉션뷰처럼 위 아래 왼쪽 오른쪽을 맞춰줘도 자동 조정이 안된다.
#### 스크롤 뷰 안에 컨텐츠 뷰의 사이즈까지 지정해줘야 스크롤 뷰 설정도 완료된다.

### 스크롤 뷰, 컨텐츠 뷰 사이즈 설정 공식(좀 간단한 공식)
#### 1. 스크롤 뷰의 사이즈를 뷰와 똑같이 한다. ( 탑, 리딩, 트레일일, 바텀 )
#### 2. 컨텐츠 뷰의 사이즈를 스크롤 뷰와 똑같이 한다 ( 탑, 리딩, 트레일링, 바텀 )
#### 3. 컨텐츠 뷰의 너비와 높이를 뷰와 같게 한다.
#### 4. 컨텐츠 뷰의 높이 멀티플라이어를 1로 맞춰준다.
#### 5. 컨텐츠 뷰의 높이 프라이얼리티를 250으로 바꾼다 ( 중요도를 낮춰주는 것 이다. )
#### 5. -> 중요도를 낮춰주는 이유는 컨텐츠의 높이가 안에 내용에 따라 늘어나야할 수도 있기 때문이다.

## 네스티드 스크롤뷰
#### 세로로 스크롤되는 스크롤 뷰 안에
#### 좌우로 스크롤되는 스크롤 뷰를 넣는 것

### 만드는 방법
#### 스크롤 뷰 생성 - 컨텐츠 뷰 생성 - 스택 뷰 생성 - 컨테이너 뷰 생성

### 네스티드 스크롤뷰에 들어갈 컨텐츠
```Swift
override func prepare(for segue: UIStoryboardSegue, sender: Any?) {
        if segue.identifier == "award" {
            let destinationVC = segue.destination as? RecommendListViewController
            awardRecommendListViewController = destinationVC
            awardRecommendListViewController.viewModel.updateType(.award)
            awardRecommendListViewController.viewModel.fetchItems()
        } else if segue.identifier == "hot" {
            let destinationVC = segue.destination as? RecommendListViewController
            hotRecommendListViewController = destinationVC
            hotRecommendListViewController.viewModel.updateType(.hot)
            hotRecommendListViewController.viewModel.fetchItems()
        }
    }
```

## 파일베이스 연결 및 검색어 저장
```Swift
class SearchViewController: UIViewController {
    
    let db = Database.database().reference().child("searchHistory")   //---> 추가된 코드
    
    @IBOutlet weak var searchBar: UISearchBar!
    @IBOutlet weak var resultCollectionView: UICollectionView!
    
    var movies: [Movie] = []
    
    override func viewDidLoad() {
        super.viewDidLoad()
    }
}

func searchBarSearchButtonClicked(_ searchBar: UISearchBar) {
        
        // 키보드가 올라와 있을때, 내려가게 처리
        dismissKeyboard()
        // 검색어가 있는지
        guard let searchTerm = searchBar.text, searchTerm.isEmpty == false else { return }
        
        // 네트워킹을 통한 검색
        // - 목표: 서치텀을 가지고 네트워킹을 통해서 영화 검색
        // - [x] 검색API 가 필요
        // - [x] 결과를 받아올 모델 Movie, Respone
        // - [x] 결과를 받아와서, CollectionView로 표현해주자
        
        SearchAPI.search(searchTerm) { movies in
            // - [x] collectionView로 표현하기
            print("--> 몇개 넘어왔어?? \(movies.count), 첫번째꺼 제목: \(movies.first?.title)")
            DispatchQueue.main.async {
                self.movies = movies
                self.resultCollectionView.reloadData()
                let timestamp: Double = Date().timeIntervalSince1970.rounded()  //---> 추가된 코드
                self.db.childByAutoId().setValue(["term": searchTerm, "timestamp": timestamp])   //---> 추가된 코드
            }
        }
        
        print("--> 검색어: \(searchTerm)")
    }
```

## 검색어 서버에서 가져오기
```Swift
import UIKit
import Firebase
class HistoryViewController: UIViewController {

    @IBOutlet weak var tableView: UITableView!
    
    let db = Database.database().reference().child("searchHistory")
    var searchTerms: [SearchTerm] = []
    
    override func viewDidLoad() {
        super.viewDidLoad()
    }
    
    override func viewWillAppear(_ animated: Bool) {
        super.viewWillAppear(animated)
        
        db.observeSingleEvent(of: .value) { (snapshot) in
            
            guard let searchHistory = snapshot.value as? [String: Any] else { return }
            
            let data = try! JSONSerialization.data(withJSONObject: Array(searchHistory.values), options: [])
            
            let decoder = JSONDecoder()
            let searchTerms = try! decoder.decode([SearchTerm].self, from: data)
            self.searchTerms = searchTerms
            self.tableView.reloadData()
            print("---> snapshot: \(data), \(searchTerms)")
        }
    }
}

extension HistoryViewController: UITableViewDataSource {
    func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        return searchTerms.count
    }
    
    func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
        guard let cell = tableView.dequeueReusableCell(withIdentifier: "HistoryCell", for: indexPath) as? HistoryCell
        else {
            return UITableViewCell()
        }
        cell.searchTerm.text = searchTerms[indexPath.row].term
        return cell
    }
}

class HistoryCell: UITableViewCell {
    @IBOutlet weak var searchTerm: UILabel!
}

struct SearchTerm: Codable {
    let term: String
    let timestamp: TimeInterval
}
```

## 표시되는 검색어히스토리를 정렬하기(검색한 시간에 따라)
```Swift
// 기존 검색어 표시 방법 (무작위)
self.searchTerms = searchTerms

// 검색을 한 순서대로 정렬
self.searchTerms = searchTerms.sorted(by: { (term1, term2) in
                return term1.timestamp > term2.timestamp
            })
// term1을 검색한 시간은 term2를 검색한 시간보다 빠르다.

// 위 코드는 아래처럼 줄여서 사용할 수 있지만 별로 선호되지 않는다.
self.searchTerms = searchTerms.sorted { $0.timestamp > $1.timestamp }
```