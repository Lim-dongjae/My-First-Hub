#아이폰 개발 시작하기

#주제 - iOS 앱 개발 #패스트 캠퍼스

---

5강 스위프트 __Function and Optional__

## Method - 어떠한 기능을 수행하는 코드 블럭
## Function - 어떠한 기능을 수행하는 코드 블럭
### 그렇다면 Method와 Function의 차이점은 무엇일까?
> __Method__ 는 __오브젝트 안에서만__ 활용되고
> __Function__ 은 __독립적으로__ 사용이 가능하다
> __Function__ 이 오브젝트 안으로 들어간다면 그것 또한 메서드가 된다.

```Swift

func printName() {
    print("---> My name is Jason")
}

printName()



// param 1개
// 숫자를 받아서 10을 곱해서 출력한다
func printMultipleOfTen(value: Int) {
    print("\(value) * 10 = \(value * 10)")
}

printMultipleOfTen(value: 5)


// param 2개
// 물건값과 갯수를 받아서 전체 금액을 출력하는 함수
func printTotalPrice(price: Int, count: Int) {
    print("TotalPrice: \(price * count)")
}
printTotalPrice(price: 1500, count: 5)
// 위의 코드는 값을 입력할 때 printTotalPrice(price: 1500, count: 5) 와 같이 파라미터를 입력해야한다.
// 만약 printTotalPrice(500, 5) 이렇게 간단하게 입력하기 위해서는 아래와 같이 하면 된다.

func printTotalPrice1(_ price: Int, _ count: Int) {
    print("TotalPrice: \(price * count)")
}
printTotalPrice1(1500, 5)
// 또한 한글로 파라미터를 받아오고 싶을 땐
// 아래와 같이 작성할 수 있다.

func printTotalPrice2(가격 price: Int, 갯수 count: Int) {
    print("TotalPrice: \(price * count)")
}
printTotalPrice2(가격: 1500, 갯수: 5)
// 협업을 하는 경우가 많으니 이런 정보는 명확한 의미전달을위해 명시적으로 작성하는 것이 좋다


printTotalPrice(price: 1500, count: 1)
printTotalPrice(price: 1500, count: 2)
printTotalPrice(price: 1500, count: 3)
printTotalPrice(price: 1500, count: 4)
// 위와 같이 가격이 같은 경우
// 저 가격은 디폴트 값으로 유추할 수 있다.
// 만약 디폴트가 맞을 경우
// 아래와 같이 코드를 작성할 수 있다.

func printTotalPrice3(price: Int = 1500, count: Int) {
    print("TotalPrice: \(price * count)")
}

printTotalPrice3(count: 1)
printTotalPrice3(count: 2)
printTotalPrice3(count: 3)
printTotalPrice3(count: 4)
// 위와 같이 코드가 조금은 깔끔해진 것을 확인할 수 있다.
// 만약 디폴트값이 존재하지만 가끔 튀는 경우로
// 다른 가격을 계산해야 한다면
printTotalPrice3(price: 2000, count: 1)
// 위와 같이 그냥 넣어주면 정상적으로 처리된다.


// 계산된 TotalPrice를 활용하기
func totalPrice(price: Int, count: Int) -> Int {
    let totalPrice = price * count
    return totalPrice
}

let calculatedPrice = totalPrice(price: 10000, count: 77)
calculatedPrice

```

## Funtion - 도전 과제
>1. 성, 이름을 받아서 fullname을 출력하는 함수 만들기
>2. 1번에서 만든 함수인데, 파라미터 이름을 제거하고 fullname 출력하는 함수 만들기
>3. 성, 이름을 받아서 fullname return 하는 함수 만들기

```Swift

//1. 성, 이름을 받아서 fullname을 출력하는 함수 만들기
func printFullName(firstName: String, lastName: String) {
    print("FullName is = \(firstName) \(lastName)")
}
printFullName(firstName: "동재", lastName: "임")


//2. 1번에서 만든 함수인데, 파라미터 이름을 제거하고 fullname 출력하는 함수 만들기
func printFullName2(_ firstName: String, _ lastName: String) {
    print("FullName is = \(firstName) \(lastName)")
}
printFullName2("동재", "임")


//3. 성, 이름을 받아서 fullname return 하는 함수 만들기
func fullName(firstName: String, lastName: String) -> String {
    return "\(firstName) \(lastName)"
}

let name = fullName(firstName: "동재", lastName: "임")
name

```

## Funtion 고급
### - 함수의 구조
>func functionName(externalName param: ParamType) -> ReturnType {
>    //.......
>    return returnValue
>}

## 1. 오버로드
### - 같은 함수를 다르게 표현하는 방법

```Swift
//일반 정수
func printTotalPrice(price: Int, count: Int) {
    print(" Total Price: \(price * count) ")
}

//소숫점을 포함한 정수
func printTotalPrice2(price: Double, count: Double) {
    print(" Total Price: \(price * count) ")
}

//한국어로 표시
func printTotalPrice3(가격: Int, 갯수: Int) {
    print(" Total Price: \(가격 * 갯수) ")
}
```

## 2. In - Out paramter
>func incrementAndPrint(_ value: Int) {
>    value += 1
>    print(value)
>}
위 코드는 오류가 발생한다.

### 오류가 발생하는 이유
파라미터는 기본적으로 함수 안으로 복사되어 들어온다
파라미터 자체는 컨스턴스이다 (한번 정의가 되면 바꿀 수 없다)
근데 위 "value += 1" 에서 바꾸려고 했기 때문에 오류가 발생한다.
파라미터 변수를 변경하고 싶을 땐 In-Out 을 사용해야한다.

```Swift
//사용법
var value = 3
func incrementAndPrint(_ value: inout Int) {
    value += 1
    print(value)
}

incrementAndPrint(&value)


//함수자체를 파라미터로 넘겨주는 방법
// ---- Function as a param

func add(_ a: Int, _ b: Int) -> Int {
    return a + b
}

func subtrack(_ a: Int, _ b: Int) -> Int {
    return a - b
}

var function = add
function(4, 2)
function = subtrack
function(4, 2)


func printResult(_ function: (Int, Int) -> Int, _ a: Int, _ b: Int) {
    let result = function(a, b)
    print(result)
}

printResult(add, 10, 5)
printResult(subtrack, 10, 5)

// 실무에서 함수를 사용할 땐 되도록이면 한 함수당 한가지 일만 하도록 구성하는 것이 좋다
```