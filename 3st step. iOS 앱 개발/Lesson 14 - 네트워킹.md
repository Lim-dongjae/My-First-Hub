#아이폰 개발 시작하기

#주제 - iOS 앱 개발 #패스트 캠퍼스 

---
 
14강 스위프트 __네트워킹__

## 네트워킹
#### 네트워킹은 동시성이 굉장히 중요하다.(네트워킹 로딩중에 사용자가 아무것도 하지 못하면 화가나기 때문이다.)
#### 이러한 동시성을 위한 API가 있다.
#### 그 API는 GCD & Operations 이다.

## HTTP 기초 (Hyper Text Transfer Protocol)
#### 동작 방식
1. 클라이언트 -> 서버에 요청
2. 서버 -> 클라이언트 응답

#### 클라이언트가 서버에 요청을 하는 방법
#### HTTP Request Method
1. POST : CREATE
2. GET : READ
3. UPDATE : UPDATE
4. DELETE : DELETE

## Concurrency (동시성)
- 한번에 여러개의 일을 수행하는 것
1. 사용자 인터랙션 처리 - 메인쓰레드
2. 네트워킹
3. 백그라운드에서 파일 다운로드
4. 파일 저장하고 읽기
5. 등등...
#### 컴퓨터 입장에서 일을 처리하는 공간을 쓰레드라고 한다.

## GCD (Grand Central Patch)
- iOS 개발 시 동시성을 제공해주기 위해 사용할 수 있는 API
- 스레드 위에 만들어진 녀석
- GCD는 task를 Queue를 이용해서 관리한다
#### - Queue - 자료 구조 First - in, First - out
- GCD에서 사용하는 Queue는 DispatchQueue라고 한다.

- DispatchQueue의 타입
1. Main Queue - 메인 스레드에서 작동하는 큐
```Swift
// UI처리, 사용자 인터액션 처리
DispatchQueue.main.async {
    // Do Any UI Update Here
}
```

2. Global Queue
- 큐에 들어가는 태스크의 우선순위를 QoS클래스를 통해서 표현할 수 있다.
- QoS (Quality of Service)
- QoS는 4개릐 클래스로 표현할 수 있다. (번호 순서대로 우선순위가 높다)
1. userInteravtive - 바로 수행되어야할 작업
2. userInitiated - 사용자가 결과를 기다리는 작업
3. default - 2번보다 조금 덜 중요한 작업
4. utility - 수초에서 수분이 걸리는 작업(네트워킹, 파일불러오기 등)
5. background - 사용자에게 당장 인식될 필요없는 작업
```Swift
DispatchQueue.Global(qos: .background).async {
    // Do Heavey Work Here
}
```

3. Custom Queue
- 큐를 직접 생성해서 관리해야할 때 사용한다.
- 사용 용도에 따라 qos, attributes를 사용할 수 있다.
```Swift
let concurrentQueue = DispatchQueue(label: "concurrent", qos: .background, attributes: .concurrent)
let serialQueue = DispatchQueue(label: "serial", qos: .background)
```

### 두개의 Queue 같이 쓰기
- 작업간의 의존성이 있을 때
( 이미지를 다운받고 결과에 따라서 UI를 업데이트 해야하는 경우 )
```Swift
DispatchQueue.global(qos: .background).async {

let image = downloadImageFromServer()
    DispatchQueue.main.async {
        self.imageView.image = image
    }
}
```

### 큐에 들어온 테스크 수행하기 ( Sync & Async (동기 & 비동기) )
#### Async (비동기 - 앞에 일이 수행되지 않아도 진행)
```Swift
DispatchQueue.global(qos: .background).async {
    for i in 0...5 {
        print("보라색 \(i)")
    }
}

DispatchQueue.global(qos: .userInteractive).async {
    for i in 0...5 {
        print("노란색 \(i)")
    }
}
```
async로 작업했기 떄문에 우선순위가 높은 아래 노란색 프린트 작업이 끝나기 전에 위에 보라색 작업이 시작된다.


#### sync (동기 - 앞에 일이 수행된 후 진행)
```Swift
DispatchQueue.global(qos: .background).sync {
    for i in 0...5 {
        print("보라색 \(i)")
    }
}

DispatchQueue.global(qos: .userInteractive).async {
    for i in 0...5 {
        print("노란색 \(i)")
    }
}
```
보라색을 sync로 작업했기 때문에 우선순위가 높은 아래 노란색 프린트 작업이 끝난 뒤 위에 보라색 작업이 시작된다.

#### 무거운 작업을 sync로 작성했을 경우 로딩이 끝나야만 다음 작업이 가능하기 때문에
#### 무거운 작업은 async로 작업한다.

## GCD 실습
```Swift
import UIKit

// Queue - Main, Global, Custom

// Main
DispatchQueue.main.async {
    // UI update
    let view = UIView()
    view.backgroundColor = #colorLiteral(red: 1.0, green: 1.0, blue: 1.0, alpha: 1.0)
}

// Global
DispatchQueue.global(qos: .userInteractive).async {
    // 중요한 작업, 지금 당장해야하는 것
}

DispatchQueue.global(qos: .userInitiated).async {
    // 거의 바로 해줘야하는 작업, 사용자가 결과를 기다리는 작업
}

DispatchQueue.global(qos: .default).async {
    // 순서를 신경쓰지 않는다.
}

DispatchQueue.global(qos: .utility).async {
    // 식간이 좀 걸리는 작업, 사용자가 당장 기다리지 않는 작업, 네트워킹, 큰파일 불러올 때
}

DispatchQueue.global(qos: .background).async {
    // 사용자에게 당장 인식될 필요가 없는 작업, 데이터를 미리 로딩할 때, 위치가 몰래 업데이트될 때
}

// Custom
let concurrentQueue = DispatchQueue(label: "concurrent", qos: .background, attributes: .concurrent)
let serialQueue = DispatchQueue(label: "serial", qos: .background)

// 복합적인 상황

func downloadImageFromServer() -> UIImage {
    // 무거운 작업
    return UIImage()
}

func updateUI(image: UIImage) {
    
}

DispatchQueue.global(qos: .background).async {
    // 다운로드
    let image = downloadImageFromServer()
    
    DispatchQueue.main.async {
        // 업데이트 UI
        updateUI(image: image)
    }
}

// Sync, Async
DispatchQueue.global(qos: .background).sync {
    for i in 0...5 {
        print("보라색 \(i)")
    }
}

DispatchQueue.global(qos: .userInteractive).async {
    for i in 0...5 {
        print("노란색 \(i)")
    }
}
```

## URLSession
- iOS에서는 HTTP를 이용한 네트워킹을 URLSession로 한다.
URLSessionConfiguration Class -> Default : 기본
                              -> Ephemeral : 프라이빗
                              -> Background : 컨텐츠 다운로드

URLSessionTask  -> URLSessionDataTask
                -> URLSessionUploadTask
                -> URLSessionDownloadTask

#### 요약 : iOS에서 네트워킹 할 땐 URLSession을 통해서 한다.
#### 그리고 URLSession을 생성할 땐 URLSessionConfiguration을 사용해야한다.
#### 그렇게 생성된 URLSessionConfiguration에서 실제로 네트워킹 작업을 하기 위해선
#### URLSessionTask를 통해서 수행한다.

### URLSession 실습
```Swift
// URL

let urlString = "https://itunes.apple.com/search?media=music&entity=song&term=Gdragon"
let url = URL(string: urlString)

url?.absoluteString // 주소값이 뭐냐
url?.scheme // 어떤 방식으로 네트워킹을 하고있나
url?.host // 호스트는 누구냐
url?.path // 뒤에 패스는 무엇인가
url?.query // 쿼리(검색어)는 무엇인가
url?.baseURL // 기본 주소값은 무엇인가


let baseURL = URL(string: "https://itunes.apple.com")
let relativeURL = URL(string: "search?media=music&entity=song&term=Gdragon", relativeTo: baseURL)


relativeURL?.absoluteString
relativeURL?.scheme
relativeURL?.host
relativeURL?.path
relativeURL?.query
relativeURL?.baseURL


// URLComponents

var urlComponents = URLComponents(string: "https://itunes.apple.com/search?")
let mediaQuery = URLQueryItem(name: "media", value: "music")
let entityQuery = URLQueryItem(name: "entity", value: "song")
let termQuery = URLQueryItem(name: "term", value: "지드래곤") // URLQueryItem를 사용하면 자동으로 한글이 인코딩된다.

// 쿼리 추가하기
urlComponents?.queryItems?.append(mediaQuery)
urlComponents?.queryItems?.append(entityQuery)
urlComponents?.queryItems?.append(termQuery)

urlComponents?.url
urlComponents?.string
urlComponents?.queryItems
```