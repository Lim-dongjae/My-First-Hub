#아이폰 개발 시작하기

#주제 - iOS 앱 개발 #패스트 캠퍼스

---

5강 스위프트 __Function and Optional__ - Study
전 강의에서 배운 내용을 복습하고
이해가 되지 않는 부분의 강의를 다시 복습

```Swift

func printName() {
    print("---> My name is Jason")
}

printName()


func printMultipleOfTen(value: Int) {
    print("\(value) * 10 = \(value * 10)")
}

printMultipleOfTen(value: 5)


func printTotalPrice1(_ price: Int, _ count: Int) {
    print("TotalPrice: \(price * count)")
}
printTotalPrice1(1500, 5)



func totalPrice(price: Int, count: Int) -> Int {
    let totalPrice = price * count
    return totalPrice
}

let calculatedPrice = totalPrice(price: 10000, count: 77)
calculatedPrice


func printFullName2(_ firstName: String, _ lastName: String) {
    print("FullName is = \(firstName) \(lastName)")
}
printFullName2("동재", "임")


func fullName(firstName: String, lastName: String) -> String {
    return "\(firstName) \(lastName)"
}

let name = fullName(firstName: "동재", lastName: "임")
name


var value = 3
func incrementAndPrint(_ value: inout Int) {
    value += 1
    print(value)
}

incrementAndPrint(&value)


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


```