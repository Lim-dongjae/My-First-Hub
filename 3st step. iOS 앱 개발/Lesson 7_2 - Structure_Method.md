#아이폰 개발 시작하기

#주제 - iOS 앱 개발 #패스트 캠퍼스 

---

7강 스위프트 __Structure - Method__
#### Object = Data + Method
## 데이터와 메서드는 어떻게 구분할까?

```Swift
struct Store {
    let loc: Location   //데이터
    let name: String    //데이터
    let deliveryRange = 2.0 //데이터
    
    func isDeliverable(userLoc: Location) -> Bool { //메소드
        let distanceToStore = distance(current: userLoc, target: loc)   //메소드
        return distanceToStore < deliveryRange  //메소드
    }   //메소드
}
```
## Method
#### 위 코드에서 메소드 부분이 Method 이다.
#### 어떠한 기능을 수행하는 블럭

```Swift
// Method

struct Lecture {
    var title: String
    var maxStudent: Int = 10
    var numOfRegistered: Int = 0
    
    func remainSeats() -> Int {
        let remainSeats = maxStudent - numOfRegistered
        return remainSeats
    }
    
    mutating func register() {
        // 등록된 학생 수 증가시키기
        numOfRegistered += 1
    }
    
    // 타입 프로퍼티 넣어보기
    static let target: String = "Anybody want to learn something"
    
    // 타입 메서드
    static func 소속학원이름() -> String {
        return "패스트캠프"
    }
}

var lec = Lecture(title: "iOS Basic")


// 강의의 남은 자리 수를 알려주는 함수
// func remainSeats(of lec: Lecture) -> Int {
//    let remainSeats = lec.maxStudent - lec.numOfRegistered
//    return remainSeats
//}

//remainSeats(of: lec)
lec.remainSeats()
// 위처럼 관련된 일을 하는 코드는
// 따로 빼서 작성할 필요 없이
// 스트럭트 안에 넣어서 사용하면된다.

lec.register()
lec.remainSeats()

Lecture.target
Lecture.소속학원이름()
```

## Method
#### 위에서 사용해본 Method를 확장해서 사용해보자

```Swift
// Method 확장

struct Lecture {
    var title: String
    var maxStudent: Int = 10
    var numOfRegistered: Int = 0
    
    func remainSeats() -> Int {
        let remainSeats = maxStudent - numOfRegistered
        return remainSeats
    }
    
    mutating func register() {
        // 등록된 학생 수 증가시키기
        numOfRegistered += 1
    }
    
    // 타입 프로퍼티 넣어보기
    static let target: String = "Anybody want to learn something"
    
    // 타입 메서드
    static func 소속학원이름() -> String {
        return "패스트캠프"
    }
}

var lec = Lecture(title: "iOS Basic")


// 강의의 남은 자리 수를 알려주는 함수
// func remainSeats(of lec: Lecture) -> Int {
//    let remainSeats = lec.maxStudent - lec.numOfRegistered
//    return remainSeats
//}

//remainSeats(of: lec)
lec.remainSeats()
// 위처럼 관련된 일을 하는 코드는
// 따로 빼서 작성할 필요 없이
// 스트럭트 안에 넣어서 사용하면된다.

lec.register()
lec.remainSeats()

Lecture.target
Lecture.소속학원이름()


// 타입메서드가 절대값을 만들어주는 스트럭트
struct Mathod {
    static func abs(value: Int) -> Int {
        if value > 0 {
            return value
        } else {
            return -value
        }
    }
}

Mathod.abs(value: -20)

// 위 처럼 기능을 사용하다가
// 새로운 기능이 필요하거나
// 바로 이어서 사용하고 싶다면 extension을 사용하면된다.
// 많은 코드들 속에서 스트럭트를 찾기 힘들 때 사용하면 좋겠다.

// 뜻 : Mathod 스트럭트의 기능을 확장하겠다.
// 제곱, 반값
extension Mathod {
    static func sqaure(value: Int) -> Int {
            return value * value
    }
    
    static func half(value: Int) -> Int {
            return value/2
    }
}

Mathod.sqaure(value: 5)
Mathod.half(value: 20)


// 애플이 정의한 Int에도 extension을 사용한다면
// 마음대로 사용할 수가 있다.

var value: Int = 3

extension Int {
    func square() -> Int {
        return self * self
    }
    
    func half() -> Int {
        return self/2
    }
}

value.square()
value.half()

// 실무에서는 extension가 많이 사용되기 때문에
// 잘 알아두면 좋겠다.
```