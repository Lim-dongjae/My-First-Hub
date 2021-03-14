#아이폰 개발 시작하기

#주제 - iOS 앱 개발 #패스트 캠퍼스 

---

7강 스위프트 __Structure__

## 값들을 많이 저장할 때 관계가 있는 값들을 한꺼번에 저장하는 방법
## Object = Data + Method
## Object가 구현될 수 있는 방법은
## Structure와 Class가 있다.
### 두개는 무엇이 다른것일까?
### Structure와 Class는 동작이 다르다.
### Structure는 Value Types이고 값을 Copy하여 사용한다.
### Class는 Reference Types이고 값을 참조(공유)하여 사용한다.
### Class는 Heap 공간에 생성되고 Structure는 Stack 공간에 생성된다.

````Swift
// --- Class vs. Structure 동작 차이 알아보기

class PersonClass {
    var name: String
    var age: Int

    init(name: String, age: Int) {
        self.name = name
        self.age = age
    }
}


struct PersonStruct {
    var name: String
    var age: Int

    init(name: String, age: Int) {
        self.name = name
        self.age = age
    }
}

let pClass1 = PersonClass(name: "Jason", age: 5)
let pClass2 = pClass1
pClass2.name = "Hey"

pClass1.name
pClass2.name


var pStruct1 = PersonStruct(name: "Jason", age: 5)
var pStruct2 = pStruct1
pStruct2.name = "Hey"

pStruct1.name
pStruct2.name

````

---

# __Structure__
## 어떤 관계있는 데이터를 묶어서 표현할 때 Structure를 사용하는 것이 좋다.

## 예) 편의점
> ### - 브랜드
> ### - 위치
> ### - 로고
> ### - 물품 목록
> ### ...

---

## Structure 공부를 위해
## 가장 가까운 편의점 찾기 를 주제로 실습해보자.

````Swift
//Structure
//가장 가까운 편의점 찾기!

// 맨처음 시작시.... 코드

/*
// 주어진 편의점 정보
let store1 = (x: 3, y: 5, name: "gs")
let store2 = (x: 4, y: 6, name: "seven")
let store3 = (x: 1, y: 7, name: "cu")


// 거리 구하는 함수
func distance(current: (x: Int, y: Int), target: (x: Int, y: Int)) -> Double {
    // 피타고라스..
    let distanceX = Double(target.x - current.x)
    let distanceY = Double(target.y - current.y)
    let distance = sqrt(distanceX * distanceX + distanceY * distanceY)
    return distance
}


// 가장 가까운 편의점 프린트하는 함수
func printClosestStore(currentLocation:(x: Int, y: Int), stores:[(x: Int, y: Int, name: String)]) {
    var closestStoreName = ""
    var closestStoreDistance = Double.infinity

    for store in stores {
        let distanceToStore = distance(current: currentLocation, target: (x: store.x, y: store.y))
        closestStoreDistance = min(distanceToStore, closestStoreDistance)
        if closestStoreDistance == distanceToStore {
            closestStoreName = store.name
        }
    }
    print("Closest store: \(closestStoreName)")
}


// Store Array 세팅, 현재 내 위치 세팅
let myLocation = (x: 2, y: 2)
let stores = [store1, store2, store3]


// printClosestStore 함수이용해서 현재 가장 가까운 스토어 출력하기
printClosestStore(currentLocation: myLocation, stores: stores)
*/


// 위 처럼 Struct를 사용하지 않으면 정보가 많이 흩어져있음을 확인해볼 수 있다.
// 이제 하나씩 Sturct를 사용해보자


/*
// Structure 사용
struct Location {
    let x: Int
    let y: Int
}

// 주어진 편의점 정보
let store1 = (loc: Location(x: 3, y: 5), name: "gs")
let store2 = (loc: Location(x: 4, y: 6), name: "seven")
let store3 = (loc: Location(x: 1, y: 7), name: "cu")


// 거리 구하는 함수
func distance(current: Location, target: Location) -> Double {
    // 피타고라스..
    let distanceX = Double(target.x - current.x)
    let distanceY = Double(target.y - current.y)
    let distance = sqrt(distanceX * distanceX + distanceY * distanceY)
    return distance
}


// 가장 가까운 편의점 프린트하는 함수
func printClosestStore(currentLocation: Location, stores:[(loc: Location, name: String)]) {
    var closestStoreName = ""
    var closestStoreDistance = Double.infinity

    for store in stores {
        let distanceToStore = distance(current: currentLocation, target: store.loc)
        closestStoreDistance = min(distanceToStore, closestStoreDistance)
        if closestStoreDistance == distanceToStore {
            closestStoreName = store.name
        }
    }
    print("Closest store: \(closestStoreName)")
}


// Store Array 세팅, 현재 내 위치 세팅
let myLocation = Location(x: 2, y: 2)
let stores = [store1, store2, store3]


// printClosestStore 함수이용해서 현재 가장 가까운 스토어 출력하기
printClosestStore(currentLocation: myLocation, stores: stores)
*/


/*
// Structure 사용
struct Location {
    let x: Int
    let y: Int
}

struct Store {
    let loc: Location
    let name: String
}

// 주어진 편의점 정보
let store1 = Store(loc: Location(x: 3, y: 5), name: "gs")
let store2 = Store(loc: Location(x: 4, y: 6), name: "seven")
let store3 = Store(loc: Location(x: 1, y: 7), name: "cu")


// 거리 구하는 함수
func distance(current: Location, target: Location) -> Double {
    // 피타고라스..
    let distanceX = Double(target.x - current.x)
    let distanceY = Double(target.y - current.y)
    let distance = sqrt(distanceX * distanceX + distanceY * distanceY)
    return distance
}


// 가장 가까운 편의점 프린트하는 함수
func printClosestStore(currentLocation: Location, stores: [Store]) {
    var closestStoreName = ""
    var closestStoreDistance = Double.infinity

    for store in stores {
        let distanceToStore = distance(current: currentLocation, target: store.loc)
        closestStoreDistance = min(distanceToStore, closestStoreDistance)
        if closestStoreDistance == distanceToStore {
            closestStoreName = store.name
        }
    }
    print("Closest store: \(closestStoreName)")
}


// Store Array 세팅, 현재 내 위치 세팅
let myLocation = Location(x: 2, y: 2)
let stores = [store1, store2, store3]


// printClosestStore 함수이용해서 현재 가장 가까운 스토어 출력하기
printClosestStore(currentLocation: myLocation, stores: stores)
*/


// Structure도 Object로 표현될 수 있는 모양이다.
// 따라서 메서드로 만들어볼 수 있다.

// Structure 사용 메서드 추가

// 거리 구하는 함수 디스턴스를 위로 올려주면 struct에서 메서드 추가 시 사용할 수 있다.
func distance(current: Location, target: Location) -> Double {
    // 피타고라스..
    let distanceX = Double(target.x - current.x)
    let distanceY = Double(target.y - current.y)
    let distance = sqrt(distanceX * distanceX + distanceY * distanceY)
    return distance
}

struct Location {
    let x: Int
    let y: Int
}

struct Store {
    let loc: Location
    let name: String
    let deliveryTange = 2.0
    
    func isDeliverable(userLoc: Location) -> Bool {
        let distanceToStore = distance(current: userLoc, target: loc)
        return distanceToStore < deliveryTange
    }
}

// 주어진 편의점 정보
let store1 = Store(loc: Location(x: 3, y: 5), name: "gs")
let store2 = Store(loc: Location(x: 4, y: 6), name: "seven")
let store3 = Store(loc: Location(x: 1, y: 7), name: "cu")


// 가장 가까운 편의점 프린트하는 함수
func printClosestStore(currentLocation: Location, stores: [Store]) {
    var closestStoreName = ""
    var closestStoreDistance = Double.infinity
    var isDeliverable = false

    for store in stores {
        let distanceToStore = distance(current: currentLocation, target: store.loc)
        closestStoreDistance = min(distanceToStore, closestStoreDistance)
        if closestStoreDistance == distanceToStore {
            closestStoreName = store.name
            isDeliverable = store.isDeliverable(userLoc: currentLocation)
        }
    }
    print("Closest store: \(closestStoreName), isDeliveralbe: \(isDeliverable)")
}


// Store Array 세팅, 현재 내 위치 세팅
let myLocation = Location(x: 2, y: 5)
let stores = [store1, store2, store3]


// printClosestStore 함수이용해서 현재 가장 가까운 스토어 출력하기
printClosestStore(currentLocation: myLocation, stores: stores)
````

---

## Structure 도전과제

````Swift
// 도전 과제
// 1. 강의 이름, 강사 이름, 학생수를 가지는 Struct 만들기 (lecture)
// 2. 강의 어레이와 강사 이름을 받아서, 해당 강사의 강의 이름을 출력하는 함수 만들기
// 3. 강의 3개 만들고 강사 이름으로 강의 찾기

struct Lecture {
    let name: String
    let instructor: String
    let numOfStudent: Int
}


func printLectureName(form instructor: String, lectures: [Lecture]) {
    //어레이 받아오기
//    var lectureName = ""
//
//    for lecture in lectures {
//        if instructor == lecture.instructor {
//        // 만약에 instructor가 lecture.instructor와 같으면
//            lectureName = lecture.name
//            // lectureNmae은 lecture.name을 받아온다
//        }
//    }
    
    //클로저로 구현해보기
//    let lectureName = lectures.first { (lec) -> Bool in
//                                    return lec.instructor == instructor
//    }?.name ?? ""
    
    //클로저로 구현해보기 줄여서 쓰기
    let lectureName = lectures.first { $0.instructor == instructor }?.name ?? ""
    // lecture instructor가 함수에 들어온 instructor와 같은 녀석이냐?
    // 그 중에서 찾는거 제일 첫번째 것을 가져오고 가져온 녀석에 이름을 가져오고
    // 이름이 옵셔널 String타입인데 없다면 기본 값을 줘보자
    
    // $0은 lecture에 있는 아이템이다.
    // 아이템을 가져와서 하나하나 비교를 하는 것이다
    // 코드안에 조건으로
    
    // 위 처럼 클로저로 코드를 줄여서 작성할 수도 있지만
    // 초보자라면 제일 위처럼 코드를 풀어서 작성하는 것이 좋다
    
    print("강사님 강의는요: \(lectureName)")
}


let lec1 = Lecture(name: "iOS Baic", instructor: "Jason", numOfStudent: 5)
let lec2 = Lecture(name: "iOS Advanced", instructor: "Jack", numOfStudent: 5)
let lec3 = Lecture(name: "iOS Pro", instructor: "Jim", numOfStudent: 5)
let lectures = [lec1, lec2, lec3]

printLectureName(form: "Jack", lectures: lectures)
````


# __Protocol__
## 지켜야 할 약속
## (어떤 서비스를 이용할 때 우리가 해야할 일들의 목록)

```Swift
// Protocol

// CustomStringConvertible
struct Lecture: CustomStringConvertible {
    var description: String {
        return "Title: \(name), Instructor: \(instructor)"
    }
    
    let name: String
    let instructor: String
    let numOfStudent: Int
}


func printLectureName(form instructor: String, lectures: [Lecture]) {
    var lectureName = ""

    for lecture in lectures {
        if instructor == lecture.instructor {
        // 만약에 instructor가 lecture.instructor와 같으면
            lectureName = lecture.name
            // lectureNmae은 lecture.name을 받아온다
        }
    }
    print("강사님 강의는요: \(lectureName)")
}


let lec1 = Lecture(name: "iOS Baic", instructor: "Jason", numOfStudent: 5)
let lec2 = Lecture(name: "iOS Advanced", instructor: "Jack", numOfStudent: 5)
let lec3 = Lecture(name: "iOS Pro", instructor: "Jim", numOfStudent: 5)
let lectures = [lec1, lec2, lec3]

printLectureName(form: "Jack", lectures: lectures)
print(lec1)
```