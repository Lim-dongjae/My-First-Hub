#아이폰 개발 시작하기

#주제 - iOS 앱 개발 #패스트 캠퍼스

---

5강 스위프트 __Optional__
## 값이 있을 수도 있고 없을 수도 있다.

``` Swift
var name: String = "Joon"
var dogName: String = "Mozzi"
var carName: String = ???
```
위 처럼 값이 있는 경우도 있지만
carName 과 같이 값이 있을 수도 있고 없을 수도 있는 경우에
옵셔널을 사용한다.


``` Swift
var carName: String?
carName = nil
carName = "탱크"
// 옵셔널은 ?를 타입뒤에 선언함으로써
// 값이 있을 수도 있고 없을 수도 있음을 표시한다.



// 아주 간단한 과제
// 1. 여러분이 최애하는 영화배우의 이름을 담는 변수를 작성해주세요 (타입 String?)
// 2. let num = Int("10") -> 타입은 뭘까요?

// 아주 간단한 과제 풀이
// 1. 여러분이 최애하는 영화배우의 이름을 담는 변수를 작성해주세요 (타입 String?)
var movieStar: String? = nil
movieStar = "Jo BoA"


// 2. let num = Int("10") -> 타입은 뭘까요?
// --> Int?


// 고급 기능 4가지
// Forced unwrapping > 강제 추출
// Optional binding (if let) > 부드럽게 언박싱 1
// Optional binding (guard) > 부드럽게 언박싱 2
// Nil coalescing > 언박싱했을 때 내용물이 없다면 디폴트 값을 내보내기


// 1. Forced unwrapping > 강제 추출 방법
carName = nil
//print(carName!)
// 값이 있을 땐 carName의 값만을 출력하지만
// 값이 없을 땐 컴파일 오류를 출력한다

// 2. Optional binding (if let) > 부드럽게 언박싱 1 방법
if let unwrappedCarName = carName {
    print(unwrappedCarName)
} else {
    print("Car Name 업다")
}
// 값이 있을 땐 if let 구문의 프린트를 출력하고
// 값이 없을 땐 else 구문의 프린트를 출력한다


/*
여행 계획표
 - 싱가폴
 - 싱가폴 맛집
 - 싱가폴 숙소
 
 여행 계획표
  - 싱가폴
    - 뭐뭐
    - 뭐뭐
        -뭐뭐뭐
  - 싱가폴 맛집
  - 싱가폴 숙소
 
 위 처럼 여행 계획표가 복잡해지면 읽기가 어렵다
 코딩도 마찬가지로 코드가 복잡해지면 가독성이 떨어지기 때문에
 코드는 최대한 간단하게 작성해야한다.
*/

func printParsedInt(from: String) {
    if let parsedInt = Int(from) {
        print(parsedInt)
        // Cyclomatic Complexity
    } else {
        print("Int로 컨버팅 불가")
    }
}
printParsedInt(from: "100")
printParsedInt(from: "카")
// 위 방법으로 코드를 작성한다면
// 조건이 계속해서 추가될 경우
// if let 부분에 if / else 가 계속 추가되야하고
// 결국 코드의 레벨 딥스가 깊어져
// 싸이클로메틱 컴플렉시티가 높아진다
// 따라서 조건이 많을 경우에
// 실무진에서는 위처럼 코드를 작성하는 것을 피해야한다.



// 복잡할 수 있는 코드를 더욱 간단하게 만드는 방법
// Optional binding (guard) > 부드럽게 언박싱 2 방법

func printParsedInt1(from: String) {
    guard let parsedInt = Int(from) else {
        print("Int로 컨버팅 불가")
        return
    }
    
    print(parsedInt)
}
printParsedInt1(from: "100")
printParsedInt1(from: "카")

// 위 코드는 guard let 구문을 통과하면 밑에 코드를 실행하고
// 가드를 통과하지 못한다면 가드에서 코드가 종료된다.
// 이렇게 작성한다면 코드의 복잡성을 줄일 수 있고
// 싸이클로메틱 컴플렉시티를 낮출 수 있다. (레벨 딥스를 낮추는 방법)



// Nil coalescing > 언박싱했을 때 내용물이 없다면 디폴트 값을 내보내기 방법
let myCarName: String = carName ?? "모델 S"
// 해석 : 만약 myCarName의 값이 nil이면 "모델 S"를 넣어줘라
// 위 처럼 ??를 사용하여 carName이 nil이라면 뒤에 값을 디폴트 값으로 설정할 수 있다.
// 이 것을 닐코얼리싱이라고 한다.




// --- 도전 과제
// 1. 최대 음식 이름을 담는 변수를 작성 (String?)
// 2. 옵셔널 바인딩을 이용해서 값을 확인해보기
// 3. 닉네임을 받아서 출력하는 함수 만들기, 조건 입력 파라미터는 String?



// 1. 최대 음식 이름을 담는 변수를 작성 (String?)
var favoriteFood: String? = "카레"


// 2. 옵셔널 바인딩을 이용해서 값을 확인해보기
if let favoriteFoodName = favoriteFood {
    print(favoriteFoodName)
} else {
    print("모릅니다")
}


// 3. 닉네임을 받아서 출력하는 함수 만들기, 조건 입력 파라미터는 String?
func callNic(name: String?) {
    guard let nickName = name else {
        print("넥네임이 없습니다.")
        return
    }
    print(nickName)
}
 
callNic(name: "다람쥐")
callNic(name: nil)
```