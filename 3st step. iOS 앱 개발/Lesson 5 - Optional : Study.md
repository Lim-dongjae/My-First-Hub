#아이폰 개발 시작하기

#주제 - iOS 앱 개발 #패스트 캠퍼스

---

5강 스위프트 __Optional__ - Study
전 강의에서 배운 내용을 복습하고
이해가 되지 않는 부분의 강의를 다시 복습

```Swift


// 복습 문제
// 1. 내가 가장 좋아하는 강아지의 종류는? (타입 String?)
// 2. 내가 가장 좋아하는 강아지의 이름은? (타입 String?)
// 3. 내가 가장 좋아하는 강아지는 누구인가? (함수 복습)
var myFavoriteDogSpecies: String? = "푸들"
var myFavoriteDogName: String? = "다롱이"
if let Species = myFavoriteDogSpecies, let Name = myFavoriteDogName {
    print("내가 가장 좋아하는 강아지는 \(Species)인 \(Name)입니다.")
}


var carName: String?
carName = nil
carName = "탱크"

if let unwrappedCarName = carName {
    print(unwrappedCarName)
} else {
    print("Car Name 없다")
}

var cafeName: String? = "스타벅스"

if let callName = cafeName {
    print(callName)
} else {
    print("키페 이름을 모릅니다.")
}


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

func convertingIntToSring(value: String) {
    if let converting = Int(value) {
        print(converting)
    } else {
        print("컨버팅이 불가능합니다")
    }
}

convertingIntToSring(value: "문자")
convertingIntToSring(value: "10")



func printParsedInt1(from: String) {
    guard let parsedInt = Int(from) else {
        print("Int로 컨버팅 불가")
        return
    }
    
    print(parsedInt)
}
printParsedInt1(from: "100")
printParsedInt1(from: "카")



let myCarName: String = carName ?? "모델 S"



var myFavortieFood: String? = nil

if let foodName = myFavortieFood {
    print(foodName)
} else {
    print("좋아하는 음식을 알 수 없습니다.")
}

func printNickName(Name: Any?) {
    guard let nickName = Name else {
        print("닉네임을 입력해주세요.")
        return
    }
    print(nickName)
}
printNickName(Name: "다롱이")
printNickName(Name: 10)
printNickName(Name: nil)


func noneName(_ Name: Any?) {
    guard let whatName = Name else {
        print("이름을 알 수 없습니다.")
        return
    }
    print(whatName)
}

noneName(10)
noneName("다롱다리")
noneName(nil)


func minimumName(_ Name: String?) {
    guard let whatName = Name, whatName.count > 1, whatName.count < 8 else {
        print("이름을 두글자 이상, 8글자 미만으로 작성해주세요.")
        return
    }
    print(whatName)
}

minimumName("임동재입니다")
minimumName("111임동재")
minimumName("임")
minimumName("임동재임동재입니다")

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