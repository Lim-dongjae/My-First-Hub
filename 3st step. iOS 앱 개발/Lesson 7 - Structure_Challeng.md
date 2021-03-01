#아이폰 개발 시작하기

#주제 - iOS 앱 개발 #패스트 캠퍼스 

---

7강 스위프트 __Structure__
#### 도전 과제가 이해되지 않아 처음부터 천천히 작성해보자.

## 도전1
### 우선 이번에는 피타고라스 정리를 이용해
### 나의 위치와 편의점의 위치를 선언한 뒤
### 나와 편의점의 거리를 구해보자.

````Swift
// 나와 가장 가까운 편의점 찾기

// 먼저 피타고라스의 정리로 좌표 평면에서 두 점 사이의 거리를 구해보자
// 예)
// A와 B의 위치를 A(3, 5), B(6, 1)로 가정해보자
// 이 경우 A와 B를 직선으로 연결할 경우
// 피타고라스 정리에서 필요한 직삼각형의 빗변으로 가정할 수 있다.
// (직각 삼각형의 높이 변은 a라 하고, 밑변은 b라 하고, 빗변은 c라 한다.)
// 또한, X축에 평행인 변의 길이는 x1 -x2로 구할 수 있고,
// Y축에 평행인 변의 길이는 y1 - y2를 통해 구할 수 있다.
// 이대로 계산해본다면 (x1, y1)이 첫 번째 점이 되며
// (x2, y2)가 두 번째 점이 될 것이다.

// 현재 주어진 두 좌표는 A(3, 5), B(6, 1)이며, 이에 따른 밑변의 길이는 다음과 같이 구할 수 있다.
// x1 - x2 = 3 - 6 = -3 = 3

// 이제 높이를 다음과 같이 구할 수 있다.
// y1 - y2 = 1 - 5 = -4 = 4

// 위와 같이 우리에게 주어진 직각삼각형의 직각변을 낀 변 a,b의 길이는
// a = 3, b = 4 이다.

// 그렇다면 A좌표와 B좌표의 거리인 빗면을 구해볼 수 있다.
// (a제곱 + b제곱 = c제곱)

// (3*3) + (4*4) = ?
// 9 + 16 = 25

// 위와 같이 빗면 c는 루트(25)로 확인해볼 수 있다.
// 따라서 A좌표와 B좌표의 거리는 5이다.

// 피타고라스의 정리를 통한 계산은 위와 같았다.
// 그렇다면 Structure 예제 문제에 다시 접근해보자

// 나와 가장 가까운 편의점 찾기
// 나와 가장 가까운 편의점을 찾기위해선
// 나의 좌표 그리고 편의점에 좌표만 있다면
// 아주 쉽게 찾아볼 수 있다.

// 예시로 하나의 거리만 계산해보자

// 나의 좌표 (2, 5), 편의점 좌표 (7, 2)
// 밑변의 길이 = (2 - 7) = -5 = 5

// 높이의 길이 = (5 - 2) = -3 = 3

// 빗변의 길이 = (5의 제곰 + 3의 제곱 = ?의 제곱) = (25 + 9 = 34)
// 나와 편의점의 거리 5.8309...

// 그렇다면 위 공식을 대입해 함수를 만들어보자

// 나와 편의점의 거리 구하기
let myLocation = (x: 3, y: 4)
let store = (x: 2, y: 2)

func distance(내위치: (x: Int, y: Int), 편의점위치: (x: Int, y: Int)) -> Double {
    let distanceX = Double(내위치.x - 편의점위치.x)
    let distanceY = Double(내위치.y - 편의점위치.y)
    let distance = sqrt(distanceX * distanceX + distanceY * distanceY)
    // sqrt는 인자에 루트를 씌워준다.
    return distance
}

distance(내위치: myLocation, 편의점위치: store)

````


## 도전2
### 편의점의 정보를 입력하고
### 나의 위치와 가장 가까운 편의점을 찾아
### 프린트해보자

````Swift
// 가장 가까운 거리에 있는 편의점을 찾아 프린트하기
// 여기선 이 전에 도전해봤던 "나와 가장 편의점 찾기" 함수가 필요하기 때문에
// 함수를 가져온다.

// 함수 가져오기(나와 가장 편의점 찾기)
func distance(내위치: (x: Int, y: Int), 편의점위치: (x: Int, y: Int)) -> Double {
    let distanceX = Double(내위치.x - 편의점위치.x)
    let distanceY = Double(내위치.y - 편의점위치.y)
    let distance = sqrt(distanceX * distanceX + distanceY * distanceY)
    // sqrt는 인자에 루트를 씌워준다.
    return distance
}

// 필요한 정보 선언
let myLocation = (x: 3, y: 4) // 나의 현재 위치
let store1 = (x: 3, y: 5, name: "GS편의점") // 편의점 정보 1
let store2 = (x: 4, y: 6, name: "CU편의점") // 편의점 정보 2
let store3 = (x: 1, y: 4, name: "세븐일레븐") // 편의점 정보 3
let stores = [store1, store2, store3] // 편의점 정보의 묶음(Array)

func printClosestStore(현재위치:(x: Int, y: Int), 편의점정보:[(x: Int, y: Int, name: String)]) {
    var 가장가까운편의점이름 = ""
    var 가장가까운편의점거리 = Double.infinity

    for store in stores {
        let 편의점과의거리 = distance(내위치: 현재위치, 편의점위치: (x: store.x, y: store.y))
        가장가까운편의점거리 = min(편의점과의거리, 가장가까운편의점거리)
        if 가장가까운편의점거리 == 편의점과의거리 {
            가장가까운편의점이름 = store.name
        }
    }
    print("가장 가까운 편의점은: \(가장가까운편의점이름) 입니다.")
}

printClosestStore(현재위치: myLocation, 편의점정보: stores)


// 자체적 해석본
// 가장 가까운 편의점 프린트하는 함수
func printClosestStore(currentLocation: Location, stores: [Store]) {
    //printClosestStore로 명명한 함수는 currentLocation의 값으로 Location형식을 받아오고, stores의 값으로 Store형식의 Array를 받아온다
    var closestStoreName = ""
    //closestStoreName의 값은 아직 없다
    var closestStoreDistance = Double.infinity
    //closestStoreDistance의 형식은 Double이며 소숫점에 제한이 없는 infinity이다.

    for store in stores {
    //store의 반복문이다
        let distanceToStore = distance(current: currentLocation, target: store.loc)
        //distanceToStore는 distance(거리구하는함수)이다.
        //current의 값은 currentLocation이고, target의 값은 store의 인자 중 loc 타입이다.
        closestStoreDistance = min(distanceToStore, closestStoreDistance)
        //closestStoreDistance는 distanceToStore 와 closestStoreDistance 중 더 작은 수 이다.
        if closestStoreDistance == distanceToStore {
        //closestStoreDistance와 distanceToStore가 같다면
            closestStoreName = store.name
            //closestStoreName는 store의 인자 중 name이다.
        }
    }
    print("Closest store: \(closestStoreName)")
}
````


## 도전3
### 위에서 도전해본 과제에
### Structure를 적용해보자

````Swift
// Structure 사용
struct Location {
    let x: Int
    let y: Int
}

struct Store {
    let loc: Location
    let name: String
}

// 필요한 정보
var myLocation = Location(x: 2, y: 2)
let testStore = Location(x: 3, y: 5)
let store1 = Store(loc: Location(x: 3, y: 5), name: "씨유")
let store2 = Store(loc: Location(x: 4, y: 6), name: "지에스")
let store3 = Store(loc: Location(x: 2, y: 7), name: "미니스톱")
let stores = [store1, store2, store3]

// 거리를 구하는 함수
// 피타고라스의 정리 사용
func distance(current: Location, target: Location) -> Double {
    let distanceX = Double(current.x - target.x)
    let distanceY = Double(current.y - target.y)
    let distance = sqrt(distanceX * distanceX + distanceY * distanceY)
    return distance
}
// 정상작동 테스트
distance(current: myLocation, target: testStore) // 테스트 성공

// 가장 가까운 거리에 있는 타겟 프린트하는 함수
func closestStorePrint(currentLocation: Location, stores: [Store]) {
//closestStorePrint로 명명한 함수는 currentLocation의 값으로 Location스트럭트 타입을 받고, stores의 값으로 Store스트럭트 타입의 Array를 받는다.
    var closestStoreName = ""
    var closestStoreDistance = Double.infinity
    
    for store in stores {
    // stores라는 Array안에 인자들이 store라는 변수에 할당되면서 코드가 실행되며,
    // 아래 코드에 만족하는 값이 나올 때 까지 반복합니다.
        let distanceToStore = distance(current: currentLocation, target: store.loc)
        //distanceToStore는 distance(거리구하는함수)이다.
        //current의 값은 currentLocation이고, target의 값은 store 변수의 인자 중 loc 타입이다.
        closestStoreDistance = min(distanceToStore, closestStoreDistance)
        //closestStoreDistance는 distanceToStore 와 closestStoreDistance 중 더 작은 수 이다.
        if closestStoreDistance == distanceToStore {
        //closestStoreDistance와 distanceToStore가 같다면
            closestStoreName = store.name
            //closestStoreName는 store의 인자 중 name이다.
        }
    }
    print("가장 가까운 거리의 편의점은 \(closestStoreName) 입니다.")
}
// 정상작동 테스트
closestStorePrint(currentLocation: myLocation, stores: stores) // 테스트 성공
````


## 도전4
### 위에서 도전해본 과제에
### 배달 가능 지역을 표시해보자

````Swift
// 배달 가능 지역인지 확인해보기

// 거리를 구하는 함수
// 피타고라스의 정리 사용
func distance(current: Location, target: Location) -> Double {
    let distanceX = Double(current.x - target.x)
    let distanceY = Double(current.y - target.y)
    let distance = sqrt(distanceX * distanceX + distanceY * distanceY)
    return distance
}
// 정상작동 테스트
distance(current: myLocation, target: testStore) // 테스트 성공


// Structure 사용
struct Location {
    let x: Int
    let y: Int
}

struct Store {
    let loc: Location
    let name: String
    let deliveryRange = 2.0
    // deliveryRange(배달 지역 범위)는 2.0이다.
    
    func isDeliverable(userLoc: Location) -> Bool {
    // isDeliverable로 명명한 함수는 userLoc의 값으로 Location스트럭트 타입을 받아온다.
    // 그리고 값에 대한 반환은 Bool타입으로 반환한다.
        let distanceToStore = distance(current: userLoc, target: loc)
        // distanceToStore는 distance함수를 사용하고 current의 값은 userLoc를 받아오고, target은 loc의 형식을 받아온다.
        // 이 함수를 사용하기 위해 원래 아래있던 distance함수를 위로 올려줬다.
        return distanceToStore < deliveryRange
        // distanceToStore가 deliveryRange보다 작은지 리턴해달라
        // distanceToStore가 deliveryRange보다 작다면 ture값이 리턴될 것이며
        // distanceToStore가 deliveryRange보다 크다면 false값이 리턴될 것이다
    }
}


// 필요한 정보
var myLocation = Location(x: 2, y: 2)
let testStore = Location(x: 3, y: 5)
let store1 = Store(loc: Location(x: 3, y: 5), name: "씨유")
let store2 = Store(loc: Location(x: 4, y: 6), name: "지에스")
let store3 = Store(loc: Location(x: 2, y: 7), name: "미니스톱")
let stores = [store1, store2, store3]


// 가장 가까운 거리에 있는 타겟 프린트하는 함수
func closestStorePrint(currentLocation: Location, stores: [Store]) {
//closestStorePrint로 명명한 함수는 currentLocation의 값으로 Location스트럭트 타입을 받고, stores의 값으로 Store스트럭트 타입의 Array를 받는다.
    var closestStoreName = ""
    var closestStoreDistance = Double.infinity
    var isDeliverable = false
    
    for store in stores {
    // stores라는 Array안에 인자들이 store라는 변수에 할당되면서 코드가 실행되며,
    // 아래 코드에 만족하는 값이 나올 때 까지 반복합니다.
        let distanceToStore = distance(current: currentLocation, target: store.loc)
        //distanceToStore는 distance(거리구하는함수)이다.
        //current의 값은 currentLocation이고, target의 값은 store 변수의 인자 중 loc 타입이다.
        closestStoreDistance = min(distanceToStore, closestStoreDistance)
        //closestStoreDistance는 distanceToStore 와 closestStoreDistance 중 더 작은 수 이다.
        if closestStoreDistance == distanceToStore {
        //closestStoreDistance와 distanceToStore가 같다면
            closestStoreName = store.name
            //closestStoreName는 store의 인자 중 name이다.
        }
        isDeliverable = store.isDeliverable(userLoc: currentLocation)
        // isDeliverable는 store스트럭트의 isDeliverable이며 isDeliverable에 userLoc는 currentLocation을 받아온다
        //
    }
    print("가장 가까운 거리의 편의점은 \(closestStoreName) 이며, 현재 위치는 배달이 \(isDeliverable) 합니다.")
} 

// 정상작동 테스트
closestStorePrint(currentLocation: myLocation, stores: stores) // 테스트 성공
````


## 도전5
### Structure의 도전과제를 풀어보자
도전 과제
1. 강의 이름, 강사 이름, 학생수를 가지는 Struct 만들기 (lecture)
2. 강의 어레이와 강사 이름을 받아서, 해당 강사의 강의 이름을 출력하는 함수 만들기
3. 강의 3개 만들고 강사 이름으로 강의 찾기

```Swift
// struct 만들기
struct lecture {
    let name: String
    let instructor: String
    let numOfStudent: Int
}


// 강의 목록(어레이)과 강사 이름을 받아, 해당 강사의 강의 이름을 출력하는 함수 만들기
func printLectureName(instructor: String, lectures: [lecture]) {
    var lectureName = ""
    
    for lecture in lectures {
        if instructor == lecture.instructor {
        // 함수에서 넘겨받는 instructor의 값이 반복문 lecture 변수의 instructor값과 같다면
            lectureName = lecture.name
            // lectureName은 반복문 lecture 변수의 name 값이다.
        }
    }
    print("입력한 강사님의 강의 이름은 \(lectureName) 입니다.")
}


// 강의 3개 만들기
let lec1 = lecture(name: "iOS개발 기초", instructor: "일등만선생님", numOfStudent: 100)
let lec2 = lecture(name: "iOS개발 중급", instructor: "이등만선생님", numOfStudent: 200)
let lec3 = lecture(name: "iOS개발 고급", instructor: "삼등만선생님", numOfStudent: 300)
let lectures = [lec1, lec2, lec3]

printLectureName(instructor: "이등만선생님", lectures: lectures)
```