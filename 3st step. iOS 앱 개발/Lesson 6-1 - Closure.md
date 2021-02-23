#아이폰 개발 시작하기

#주제 - iOS 앱 개발 #패스트 캠퍼스

---

6-1강 스위프트 __Closure__ -보강
## 클로저란 무엇인가?

## Swift에서 함수처럼, 기능을 수행하는 코드블록의 특별한 타입
## 아주 정확히는 함수는 클로저의 한가지 타입이다.
### Closure는 크게 3가지 타입이 있다.
> ### Global 함수
> ### Nested 함수
> ### Closure Expressions
### 우리가 Closure라고 배우고 있는 것은 사실 Closure Expressions 이다.
#### Swift 공식 사이트에 가면 정확한 정의를 알아볼 수 있다. (docs.swift.org)

### Closure는 함수처럼 기능을 수행하는 코드 블럭이다.
### 하지만 함수와 다르게 이름이 없다.

# 함수와의 차이점
## Function (Global)
### - 이름이 있다.
### - func 키워드 필요

## Closure (Closure Expressions)
### - 이름이 없다.
### - func 키워드 필요 없음

# 함수와의 공통점
### - 인자를 받을 수 있다.
### - 값을 리턴할 수 있다.
### - 변수로 할당할 수 있다.
### - First Class Type 이다.
> - 변수에 할당할 수 있다.
> - 인자로 받을 수 있다.
> - 리턴할 수 있다.

# 실무에서 Closure가 자주 쓰이는 형태
## - Completion Bloock
> - 네트워크에서 데이터를 받아올 때 해당 일이 끝나고 수행될 코드

## - Higher Order Functions
> - 함수인데 인자를 함수로 받을 수 있는 개념인 고계함수
> - 대표적인 고계함수는 맵, 필터, 미지수이다.

---

## Closure syntax
### Closure 문법 실습

``` Swift
//Closure Syntax
//Closure expression syntax has the following general form:
//{ (parameters) -> return type in
//    statements
//    ....
//}

//Closure 실습

//Example 1: 심플한 Closure
let simpleColsure = {
    
}

simpleColsure() // 클로저 실행



//Example 2: 코드블록을 구현한 Closure
let simpleColsure2 = {
    print("Hello, 클로저, 코로나 끝나라!")
}

simpleColsure2() // 클로저 실행



//Example 3: 인풋 파라미터를 받는 Closure
let simpleColsure3: (String) -> Void = { name in
    print("나의 이름은 \(name) 입니다!")
}

simpleColsure3("코로나가 제일 싫어") // 클로저 실행



//Example 4: 값을 리턴하는 Closure
let simpleColsure4: (String) -> String = { name in
    let message = "iOS 개발 어렵지만, \(name)야 잘할 수 있을거야!"
    return message
}

let result = simpleColsure4("동재") // 클로저 실행
print(result)



//Example 5: Closure를 파라미터로 받는 함수 구현
func someSimpleFunction(simpleClosure: () -> Void) {
    print("함수에서 클로저가 호출되었다.")
    simpleClosure()
}

someSimpleFunction(simpleClosure: {
    print("iOS개발 공부 from closure")
})



//Example 6: Trailing Closure
func someSimpleFunction1(message: String, simpleClosure: () -> Void) {
    print("함수에서 클로저가 호출되었다. 메시지는 \(message)")
    simpleClosure()
}

someSimpleFunction1(message: "공부 열심히 하자", simpleClosure: {
    print("iOS개발 공부 from closure")
})

//클로저가 함수에서 받는 인자 중 마지막에 있다면
//클로저를 코드적으로 생략할 수 있는 방법이 있다.
someSimpleFunction1(message: "공부 열심히 하자") {
    print("iOS개발 공부 from closure")
}
```