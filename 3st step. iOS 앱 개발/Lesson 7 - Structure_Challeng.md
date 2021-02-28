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
````


## 도전3
### 위에서 도전해본 과제에
### Structure를 적용해보자
### 현재 오류가 발생했다
### 처음부터 다시 이해해보자
````Swift
//Structure 사용해보기
struct 위치 {
    let x: Int
    let y : Int
}

struct 편의점 {
    let loc: 위치
    let name: String
}

//나의 위치
let 나의위치 = (x: 3, y: 5)

//테스트용 편의점 위치
let 테스트편의점 = (x: 1, y: 6)

//편의점 정보
let store1 = (loc: 위치(x: 3, y: 5), name: "GS편의점")
let store2 = (loc: 위치(x: 4, y: 6), name: "CU편의점")
let store3 = (loc: 위치(x: 1, y: 4), name: "세븐일레븐")
let stores = [store1, store2, store3]

// 나와 편의점의 거리 구하기 - 피타고라스의 정리
func distance(내위치: 위치, 편의점위치: 위치) -> Double {
    let distanceX = Double(내위치.x - 편의점위치.x)
    let distanceY = Double(내위치.y - 편의점위치.y)
    let distance = sqrt(distanceX * distanceX + distanceY * distanceY)
    // sqrt는 인자에 루트를 씌워준다.
    return distance
}
// 오류발생 


// Structure사용
struct 위치 {
    let x: Int
    let y: Int
}

struct 편의점위치 {
    let loc: 위치
    let name: String
}

// 편의점 점보
let store1 = 편의점위치(loc: 위치(x: 3, y: 5), name: "GS편의점")
let store2 = 편의점위치(loc: 위치(x: 4, y: 6), name: "CU편의점")
let store3 = 편의점위치(loc: 위치(x: 1, y: 7), name: "세븐일레븐")
// 편의점 Array
let 편의점묶음 = [store1, store2, store3]

// 나의 위치
let 나의위치 = 위치(x: 2, y: 2)

// 편의점 거리구하기용
let 편의점거리 = 위치(x: 4, y: 5)

// 거리 구하는 함수
func 거리구하기(기준위치: 위치, 상대위치: 위치) -> Double {
    let 거리구하기X = Double(상대위치.x - 기준위치.x)
    let 거리구하기Y = Double(상대위치.y - 기준위치.y)
    let 거리구하기 = sqrt(거리구하기X * 거리구하기X + 거리구하기Y * 거리구하기Y)
    return 거리구하기
}
print(거리구하기(기준위치: 나의위치, 상대위치: 편의점거리)) // 거리 구하는 함수 실행 성공

//// 가장 가까운 거리에 있는 편의점 찾아 프린트하기
//func 가까운편의점프린트하기(현재위치: 위치, 편의점정보: 편의점위치) {
//    var 가장가까운편의점이름 = ""
//    var 가장가까운편의점거리 = Double.infinity
//
//    for 편의점 in 편의점묶음 {
//        let 편의점과의거리 = 거리구하기(기준위치: 현재위치, 상대위치: [편의점위치])
//        가장가까운편의점거리 = min(편의점과의거리, 가장가까운편의점거리)
//        if 가장가까운편의점거리 == 편의점과의거리 {
//            가장가까운편의점이름 = 편의점위치.name
//        }
//    }
//    print("가장 가까운 편의점은 \(가장가까운편의점이름)")
//}
func printClosestStore(현재위치:(x: Int, y: Int), 편의점정보:[(x: Int, y: Int, name: String)]) {
    var 가장가까운편의점이름 = ""
    var 가장가까운편의점거리 = Double.infinity

    for 편의점위치 in 편의점묶음 {
        let 편의점과의거리 = 거리구하기(기준위치: 현재위치, 상대위치: 편의점위치.loc)
        가장가까운편의점거리 = min(편의점과의거리, 가장가까운편의점거리)
        if 가장가까운편의점거리 == 편의점과의거리 {
            가장가까운편의점이름 = 편의점위치.name
        }
    }
    print("가장 가까운 편의점은: \(가장가까운편의점이름) 입니다.")
}
```
