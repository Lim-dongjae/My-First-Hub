#아이폰 개발 시작하기

#주제 - iOS 앱 개발 #패스트 캠퍼스

---

6강 스위프트 __Collection__
## 수 많은 변수들을 효과적으로 관리할 수 있도록 하는 것
## 종류에는 Array, Dictionary, Set, Closure가 있다.

---

## Array
#### - 순서가 있는 리스트 컬렉션
#### - 언제 쓰면 좋은가?
##### 1. 순서가 있는 아이템
##### 2. 아이템의 순서를 알면 유용할 때

``` Swift
//Array
let evenNumbers: [Int] = [2, 4, 6, 8]
let evenNumbers2: Array<Int> = [2, 4, 6, 8]
// Array는 위와 같이 선언할 수 있다.
// 위 2가지 방법중 위의 방법이 더 간단하기 때문에
// 강사님께서는 위에 방법을 사용한다.

// 만약 선언된 Array에 데이터를 추가하고 싶다면
// Array를 var로 선언하고 append를 사용하면 된다.
var evenNumbers3: [Int] = [2,4, 6, 8]
evenNumbers3.append(10)
evenNumbers3
// 또 다른 방법으로는
evenNumbers3 += [12, 14, 16]
evenNumbers3.append(contentsOf: [18, 20])

// evenNumbers3가 비어있는지 확인할 수 있는 방법
let isEmpty = evenNumbers3.isEmpty

// evenNumbers3가 몇개인지 확인할 수 있는 방법
evenNumbers3.count

// evenNumbers3의 첫번째 값이 무엇인지 확인할 수 있는 방법
print(evenNumbers3.first)
// 이 코드의 값은 옵셔널로 반환된다.
// Array의 값이 있을 수도 있고 없을 수도 있으니 옵셔널로 반환된다.

// evenNumbers3의 마지막 값이 무엇인지 확인할 수 있는 방법
print(evenNumbers3.last)
// 이 코드의 값은 옵셔널로 반환된다.
// Array의 값이 있을 수도 있고 없을 수도 있으니 옵셔널로 반환된다.


// 옵셔널 바인딩으로 출력하기
if let firstElement = evenNumbers3.first {
    print("첫번째 아이템은 \(firstElement)입니다.")
} else {
    print("값이 확인되지 않습니다.")
}


// 대소비교를 하는 경우
evenNumbers3.min()
evenNumbers3.max()


// 값을 뽑아오는 경우
var firstItem = evenNumbers3[0]
var secondItem = evenNumbers3[1]
var tenthItem = evenNumbers3[9]
// 위에서 count를 했을 때 evenNumbers3 에는 10개의 데이터가 있다고 했는데
// 이 때 10번째 아이템을 뽑아오고 싶다면 9를 입력하면된다.
// 이유는 숫자가 0, 1, 2, 3, 4, 5 순서로 오르기 때문이다.

// 만약 없는 순서를 가져올 땐 페이탈 에러가 발생한다. (어레이에 없으니 에러 발생)
// var twentithItem = evenNumbers3[19]


// 범위로 가져오기
let firstThree = evenNumbers3[0...2]


// 원하는 값이 들어있는지 확인할 때
evenNumbers3.contains(3)
evenNumbers3.contains(4)


// 인덱스에 새로운 값 삽입
evenNumbers3.insert(0, at: 0)


// 인덱스 안의 값을 모두 삭제하는 방법
// evenNumbers3.removeAll()
// evenNumbers3 = []
// 계속 실습을 위해 주석처리


// 인덱스 안에 특정 값을 삭제하는 방법
evenNumbers3.remove(at: 0) // 0이라는 값 지우기
evenNumbers3 // 결과 확인


// 인덱스 안에 특정 값을 업데이트 하는 방법
evenNumbers3[0] = -2 // 단일 인덱스 변경
evenNumbers3[0...2] = [-2, 0, 2] // 다수 인덱스 변경
evenNumbers3 // 결과 확인


// 인덱스 안에 값을 바꿔주기
evenNumbers3.swapAt(0, 1)
evenNumbers3 // 결과 확인


// 아이템이 많은 경우에 for look을 사용할 수 있다
for num in evenNumbers3 {
    print(num)
}


// 인덱스가 몇번째 아이템인지 확인하는 방법
for (index, num) in evenNumbers3.enumerated() {
    print("idx: \(index), value: \(num)")
}


// 인덱스의 특정 구역까지를 제외하고 확인하는 방법
evenNumbers3.dropFirst(3) // 이 코드는 evenNumbers3 에 영향을 주지 않고 값만 확인할 수 있다.
// 이 방법으로 상수를 선언하면 쉽게 관리할 수 있다.
let rangeRemoved = evenNumbers3.dropFirst(3)
// 이제 첫번째~세번째 인덱스을 제외한 값을 rangeRemoved로 쉽게 호출할 수 있다.
rangeRemoved

// 위 방법에서 인덱스 제거를 앞에서 부터가 아닌 뒤에서 부터 제거하고 싶다면
// first를 last로 바꿔주면 된다.
// 예) evenNumbers3.dropLast(3)

// 위와 달리 앞에서 3개만 가져오거나, 뒤에서 3개만 가져오고 싶을 땐 아래와 같이 사용하면 된다.
evenNumbers3.prefix(3) // 인덱스 앞에 3개만 가져오기
evenNumbers3.suffix(3) // 인덱스 뒤에 3개만 가져오기


// 사실 위에 내용을 모두 아는 것은 크게 중요하지 않다.
// 하지만 아래에 다시한번 강조하는 내용은 꼭!! 알아야한다.


// evenNumbers3가 비어있는지 확인할 수 있는 방법
let isEmpty1 = evenNumbers3.isEmpty

// evenNumbers3가 몇개인지 확인할 수 있는 방법
let count = evenNumbers3.count

// 값을 뽑아오는 경우
var firstItem1 = evenNumbers3[0]
var secondItem1 = evenNumbers3[1]
var tenthItem1 = evenNumbers3[9]
// 위에서 count를 했을 때 evenNumbers3 에는 10개의 데이터가 있다고 했는데
// 이 때 10번째 아이템을 뽑아오고 싶다면 9를 입력하면된다.
// 이유는 숫자가 0, 1, 2, 3, 4, 5 순서로 오르기 때문이다.

// 인덱스가 몇번째 아이템인지 확인하는 방법
for (index, num) in evenNumbers3.enumerated() {
    print("idx: \(index), value: \(num)")
}
```

### Dictionary
#### - 순서가 없고, 키와 값의 쌍으로 이루어진 컬렉션
#### - 언제 쓰면 좋은가?
##### 1. 어떤 값을 의미 단위로 찾을 때 유용하다.
##### - 예) 학교 시험 채점 후 반에서 내 이름을 검색해서 내가 몇점인지 찾을 수 있다.

``` Swift
// Dictionary
var scoreDic: [String: Int] = ["jason": 80, "jay": 95, "jake": 90] // [Key: value]
//var scoreDic2: Dictionary<String, Int> = ["Jason": 80, "Jay": 95, "jake": 90]
// Dictionary 또한 위와 같이 선언할 수 있다.
// 위 2가지 방법중 위의 방법이 더 간단하기 때문에
// 강사님께서는 위에 방법을 사용한다.

// 인덱스 값을 확인하는 방법
scoreDic["jason"]
scoreDic["jay"]

// 만약 인덱스에 없는 값을 확인하려하면
scoreDic["dongjae"]
// 위와 같이 없는 값을 물어봐도 오류는 발생하지 않고 nil값을 반환해준다.

// 옵셔널 바인딩으로 좀 더 안정적으로 가져오기
if let score = scoreDic["jason"] {
    score
} else {
    print("값을 찾을 수 없습니다.")
}


// Array에서 배웠던 isEmpty 와 count 를 Dictionary에서도 사용할 수 있다.
scoreDic.isEmpty
scoreDic.count

// 인덱스를 깡통으로 만드는 방법
//scoreDic = [:] // 계속 실습을 위하여 주석처리


// 인덱스를 업데이트 하는 벙법
scoreDic["jason"] = 99
scoreDic // 결과 확인


// 인덱스를 추가하는 방법
scoreDic["jack"] = 100
scoreDic // 결과 확인


// 특정 인덱스를 삭제하는 방법
scoreDic["jack"] = nil
scoreDic // 결과 확인


// 아이템이 많을 때 반복적으로 무엇인가를 해볼 때
// For Loop를 사용하여 시도해볼 수 있다.
for (name, score) in scoreDic {
    print("\(name), \(score)")
}

for key in scoreDic.keys {
    print(key)
}



// 도전 과제
// 1. 이름, 직업, 도시에 대해서 본인의 딕셔너리 만들기
// 2. 여기서 도시를 부산으로 업데이트 하기
// 3. 딕셔너리를 받아서 이름과 도시를 프린트하는 함수 만들기

// 1. 이름, 직업, 도시에 대해서 본인의 딕셔너리 만들기
var practice: [String: String] = ["name": "Dongjae", "job": "WebDesigner", "city": "Uijeongbu"]

// 2. 여기서 도시를 부산으로 업데이트 하기
practice["city"] = "Busan"
practice

// 3. 딕셔너리를 받아서 이름과 도시를 프린트하는 함수 만들기
func printPractice(dic: [String: String]) {
    if let name = dic["name"], let city = dic["city"], let job = dic["job"] {
        print("이름: \(name), 거주지: \(city), 직업: \(job)")
    } else {
        print("정보를 찾을 수 없습니다.")
    }
}
printPractice(dic: practice)
```

### Set
#### - 순서가 없고, 멤버가 유일한 컬렉션
#### - 언제 쓰면 좋은가?
##### 1. 

``` Swift

```

### Closure
#### - 
#### - 언제 쓰면 좋은가?
##### 1. 

``` Swift

```