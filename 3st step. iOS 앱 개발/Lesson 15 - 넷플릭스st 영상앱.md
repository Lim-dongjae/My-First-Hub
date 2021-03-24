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

```