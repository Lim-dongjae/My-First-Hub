#아이폰 개발 시작하기

#주제 - iOS 앱 개발 #패스트 캠퍼스 

---
 
15강 스위프트 __넷플릭스st 영상앱__

## 서치바 구현
#### 서치바에 텍스트가 없다면 guard구문 때문에 프린트 코드까지 내려가지 못하고 리턴되어
#### 검색이 되지 않는다.
```Swift
extension SearchViewController: UISearchBarDelegate {
    // 키보드가 올라와 있을 때, 내려가게하기 함수
    private func dismissKeyboard() {
        searchBar.resignFirstResponder()
    }
    
    // 검색 화면에서 검색 버튼을 눌렀을 때 아래 코드 실행
    func searchBarSearchButtonClicked(_ searchBar: UISearchBar) {
        
        // 키보드가 올라와 있을 때, 내려가게하기 실행코드
        dismissKeyboard()
        
        // 검색어가 있는지
        guard let searchTerm = searchBar.text,
              searchTerm.isEmpty == false else { return }
        
        // 네트워킹을 통한 검색
        
        
        print("---> 검색어는 \(searchTerm)")
    }
}
```

## 네트워킹을 통한 검색 (1단계)
#### 1단계에서는 구현하고자 하는 기능을 작성하기 전
#### 무엇이 필요한지 정리해보고
#### 정리된 기능들을 껍데기만 우선 만들어본다.
```Swift
extension SearchViewController: UISearchBarDelegate {
    // 키보드가 올라와 있을 때, 내려가게하기 함수
    private func dismissKeyboard() {
        searchBar.resignFirstResponder()
    }
    
    // 검색 화면에서 검색 버튼을 눌렀을 때 아래 코드 실행
    func searchBarSearchButtonClicked(_ searchBar: UISearchBar) {
        
        // 키보드가 올라와 있을 때, 내려가게하기 실행코드
        dismissKeyboard()
        
        // 검색어가 있는지
        guard let searchTerm = searchBar.text, searchTerm.isEmpty == false else { return }
        
        // 네트워킹을 통한 검색
        // - 최종 목표 : 서치텀을 가지고 네트워킹을 통해서 영화 검색
        // - 검색API가 필요
        // - 결과를 받아올 모델 Movie, Response
        // - 결과를 받아와서, CollectionView로 표현해주기
        
        // - 결과를 받아와서, CollectionView로 표현해주기 -> 만들기
        SearchAPI.search(searchTerm) { movies in
            // CollectionView 표현
        }
        
        print("---> 검색어는 \(searchTerm)")
    }
}

// - 검색API가 필요 -> 만들기
class SearchAPI {
    static func search(_ Term: String, completion: @escaping ([Movie]) -> Void) {
        
    }
}

// - 결과를 받아올 모델 Movie, Response -> 만들기
struct Response {
    
}

struct Movie {
    
}
```

## 네트워킹을 통한 검색 (2단계)
```Swift
extension SearchViewController: UISearchBarDelegate {
    // 키보드가 올라와 있을 때, 내려가게하기 함수
    private func dismissKeyboard() {
        searchBar.resignFirstResponder()
    }
    
    // 검색 화면에서 검색 버튼을 눌렀을 때 아래 코드 실행
    func searchBarSearchButtonClicked(_ searchBar: UISearchBar) {
        
        // 키보드가 올라와 있을 때, 내려가게하기 실행코드
        dismissKeyboard()
        
        // 검색어가 있는지
        guard let searchTerm = searchBar.text, searchTerm.isEmpty == false else { return }
        
        // 네트워킹을 통한 검색
        // - 최종 목표 : 서치텀을 가지고 네트워킹을 통해서 영화 검색
        // - 검색API가 필요
        // - 결과를 받아올 모델 Movie, Response
        // - 결과를 받아와서, CollectionView로 표현해주기
        
        // - 결과를 받아와서, CollectionView로 표현해주기 -> 만들기
        SearchAPI.search(searchTerm) { movies in
            // CollectionView 표현
            print("--> 몇개가 넘어왔나? \(movies.count), 첫번째 무비 제목: \(movies.first?.title)")
        }
        
        print("---> 검색어는 \(searchTerm)")
    }
}

// - 검색API가 필요 -> 만들기
class SearchAPI {
    static func search(_ term: String, completion: @escaping ([Movie]) -> Void) {
        // URLSession으로 구현해보기
        let session = URLSession(configuration: .default)
        
        // URLComponents를 사용해서 URL을 구성해보기
        var urlComponents = URLComponents(string: "https://itunes.apple.com/search?")!
        let mediaQuery = URLQueryItem(name: "media", value: "movie")
        let entityQuery = URLQueryItem(name: "entity", value: "movie")
        let termQuery = URLQueryItem(name: "term", value: term)
        urlComponents.queryItems?.append(mediaQuery)
        urlComponents.queryItems?.append(entityQuery)
        urlComponents.queryItems?.append(termQuery)
        
        // requestURL만들기
        let requestURL = urlComponents.url!
        
        // URLSessionDataTask 만들어보기
        let dataTask = session.dataTask(with: requestURL) { data, response, error in
            // 오류 코드 범위 (200~299가 성공 범위)
            let successRange = 200..<300
            
            guard error == nil,
            // 에러가 없어야하고
                  let statusCode = (response as? HTTPURLResponse)?.statusCode,
                  // statusCode는 response를 HTTPURLResponse로 캐스팅한 다음에 캐스팅한 것에 프로퍼티중 하나가 statusCode이다.
                  successRange.contains(statusCode) else {
                  // 그리고 그게 successRange안에 포함이 되는지 확인한다.
                    completion([])
                    return
                  // 문제가 있으면 completion에 제대로 내려온 무비 객체가 없다고 알려주고
                  // 문제가 없으면 아래로 내려간다.
            }
            
            guard let resultData = data else {
                completion([])
                return
            }
            
            // data -> [Movie]로 바꾸기 위해서 파싱하기
            let movies = SearchAPI.parseMovies(resultData)
            completion(movies)
            print("--> result: \(movies.count)")
            
        }
        // dataTask를 만들었으니 resume을 통해서 네트워킹 작업을 시작
        dataTask.resume()
        
    }
    
    // 파싱하는 메서드
    static func parseMovies(_ data: Data) -> [Movie] {
    // 데이터를 넣으면 무비 어레이로 만들어준다
        let decoder = JSONDecoder()
        
        do {
            let reponse = try decoder.decode(Response.self, from: data)
            // 디코더에서 reponse를 파싱한다. 이 때 디코더로 디코드하는데 Response에 대한 객체를 만들고 넘어온 데이터를 파싱한다.
            // 이 떄 파싱하는 것이 될 수도 있고 안될 수도 있기 때문에 try를 써준다.
            let movies = reponse.movies
            return movies
        } catch let error { // 시도했다가 안되면 캐치로 넘어온다.
            print("--> 파싱에 에러가 발생했습니다. 에러코드 : \(error.localizedDescription)")
            return []
        }
    }
}

// - 결과를 받아올 모델 Movie, Response -> 만들기
struct Response: Codable {
    let resultCount: Int
    let movies: [Movie]
    
    // 프로퍼티 이름과 실제 내려오는 Json의 키와 이름 맞추기
    enum CodingKeys: String, CodingKey {
        case resultCount
        case movies = "results"
    }
    
}

struct Movie: Codable {
    // 내려오는 많은 정보중에 내가 필요한 정보만 정의한다
    let title: String
    let director: String
    let thumnailPath: String
    let previewURL: String
    
    // 프로퍼티 이름과 실제 내려오는 Json의 키와 이름 맞추기
    enum CodingKeys: String, CodingKey {
        case title = "trackName"
        case director = "artistName"
        case thumnailPath = "artworkUrl100"
        case previewURL = "previewUrl"
    }
}
```
## 검색 API 복습(직접 코드 작성해보기)
```Swift
class SearchAPI {
    static func search(_ term: String, completion: @escaping ([Movie]) -> Void) {
        let session = URLSession(configuration: .default)

        var urlComponents = URLComponents(string: "https://itunes.apple.com/search?")!
        let mediaQuery = URLQueryItem(name: "media", value: "movie")
        let entityQuery = URLQueryItem(name: "entity", value: "movie")
        let termQuery = URLQueryItem(name: "term", value: term)
        urlComponents.queryItems?.append(mediaQuery)
        urlComponents.queryItems?.append(entityQuery)
        urlComponents.queryItems?.append(termQuery)

        let requestURL = urlComponents.url!

        let dataTask = session.dataTask(with: requestURL) { data, response, error in
            let successRange = 200..<300

            guard error == nil,
                  let statusCode = (response as? HTTPURLResponse)?.statusCode,
                  successRange.contains(statusCode) else {
                completion([])
                return
            }

            guard let resultData = data else {
                completion([])
                return
            }

            let movies = SearchAPI.parseMovies(resultData)
            completion(movies)
            print("--> result: \(movies.count)")
            
        }
        dataTask.resume()
    }
    
    static func parseMovies(_ data: Data) -> [Movie] {
        let decoder = JSONDecoder()

        do {
            let response = try decoder.decode(Response.self, from: data)
            let movies = response.movies
            return movies
        } catch let error {
            print("--> 파싱에 에러가 발생했습니다. 에러코드: \(error.localizedDescription)")
            return []
        }
    }
}

struct Response: Codable {
    let resultCount: Int
    let movies: [Movie]

    enum CodingKeys: String, CodingKey {
        case resultCount
        case movies = "results"
    }
}

struct Movie: Codable {
    let title: String
    let director: String
    let thumnailPath: String
    let previewURL: String

    enum CodingKeys: String, CodingKey {
        case title = "trackName"
        case director = "artistName"
        case thumnailPath = "artworkUrl1100"
        case previewURL = "previeUrl"
    }
}
```



## 검색하여 내려온 정보를 컬렌션뷰로 표현하기
```Swift
class SearchViewController: UIViewController {
    
    @IBOutlet weak var searchBar: UISearchBar!
    @IBOutlet weak var resultCollectionView: UICollectionView!
    
    // MVVM 모델에서는 뷰컨트롤러가 가지고 있으면 안되지만 이해를 돕기 위하여 여기에 작성한다.
    var movies: [Movie] = []
    
    override func viewDidLoad() {
        super.viewDidLoad()
    }
}

extension SearchViewController: UICollectionViewDataSource {
    // 데이터소스에서는 필수로 2가지를 구현해줘야한다.
    // 1. 몇개가 넘어오나?
    func collectionView(_ collectionView: UICollectionView, numberOfItemsInSection section: Int) -> Int {
        return movies.count
    }
    
    // 2. 어떻게 표현할 것인가?
    func collectionView(_ collectionView: UICollectionView, cellForItemAt indexPath: IndexPath) -> UICollectionViewCell {
        guard let cell = collectionView.dequeueReusableCell(withReuseIdentifier: "ResultCell", for: indexPath) as?
            ResultCell else {
            return UICollectionViewCell()
        }
        cell.backgroundColor = .red
        return cell
    }
}

extension SearchViewController: UICollectionViewDelegate {
    
}

extension SearchViewController: UICollectionViewDelegateFlowLayout { // 셀 사이즈 조절을 위함
    // 셀의 사이즈 비율은 7:10이며, 양 옆의 마진은 8이고, 셀간격은 10이다.
    func collectionView(_ collectionView: UICollectionView, layout collectionViewLayout: UICollectionViewLayout, sizeForItemAt indexPath: IndexPath) -> CGSize {
        
        let margin: CGFloat = 8
        let itemSpacing: CGFloat = 10
        let width = (collectionView.bounds.width - margin * 2 - itemSpacing * 2)/3
        let height = width * 10/7
        return CGSize(width: width, height: height)
    }
}

class ResultCell: UICollectionViewCell {
    @IBOutlet weak var movieThumnail: UIImageView!
}

extension SearchViewController: UISearchBarDelegate {
    // 키보드가 올라와 있을 때, 내려가게하기 함수
    private func dismissKeyboard() {
        searchBar.resignFirstResponder()
    }
    
    // 검색 화면에서 검색 버튼을 눌렀을 때 아래 코드 실행
    func searchBarSearchButtonClicked(_ searchBar: UISearchBar) {
        
        // 키보드가 올라와 있을 때, 내려가게하기 실행코드
        dismissKeyboard()
        
        // 검색어가 있는지
        guard let searchTerm = searchBar.text, searchTerm.isEmpty == false else { return }
        
        // 네트워킹을 통한 검색
        // - 최종 목표 : 서치텀을 가지고 네트워킹을 통해서 영화 검색
        // - 검색API가 필요
        // - 결과를 받아올 모델 Movie, Response
        // - 결과를 받아와서, CollectionView로 표현해주기
        
        // - 결과를 받아와서, CollectionView로 표현해주기 -> 만들기
        SearchAPI.search(searchTerm) { movies in
            // CollectionView 표현
            print("--> 몇개가 넘어왔나? \(movies.count), 첫번째 무비 제목: \(movies.first?.title)")
            DispatchQueue.main.async {
                self.movies = movies
                self.resultCollectionView.reloadData()
            }
        }
        
        print("---> 검색어는 \(searchTerm)")
    }
}
```

## 외부 코드 가져다 쓰는 방법
```Swift
import Kingfisher
        // imagePath(String) -> image
        // cell.movieThumnail.image = movie.thumnailPath 오류 발생
        // 외부 코드 가져다 쓰기
        // SPM(스위프트 패키지 매니저), Cocoa Pod, carthage
        // 이번엔 SPM을 사용하여 킹피셔를 가져와본다.
        // 방법
        // Swift - File - Swift Pakage - Add Package Dependency... -  깃허브 주소 입력 - 가져온 뒤 임포트 입력
        let  url = URL(string: movie.thumnailPath)!
        cell.movieThumnail.kf.setImage(with: url)
```