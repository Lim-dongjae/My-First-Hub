#아이폰 개발 시작하기

#주제 - iOS 앱 개발 #패스트 캠퍼스 

---
 
7강 스위프트 __Structure - property__
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
## property
#### 위 코드에서 데이터 부분이 프로퍼티 부분이다.

### 1. Stored Property
#### - 어떠한 값을 저장해서 변수로 가지고있는 것이 Stored Property이다.

### 2. Computed Property
#### - 어떤 값을 직접 저장하지는 않지만 저장된 정보를 가공 혹은 계산된 값을 제공할 때 사용한다.

### 3. Type Property
#### - 생성된 인스턴스와 상관 없이 스트럭트의 타입 혹은 클래스 타입 자체에 속성을 정하고 싶을 때 사용한다.

```Swift
// 프로퍼티

// 1. Stored Property
struct Person {
    var firstName: String
    var lastName: String
}
//실무에서는 벨류 타입을 상수로 선언하는 것을 선호한다.
//버그를 방지하기 위해서이다.

var person = Person(firstName: "Jason", lastName: "Lee")

person.firstName
person.lastName

//저장된 값을 바꿔보자
person.firstName = "Jim"
person.lastName = "Kim"

//결과 확인
person.firstName
person.lastName



// 2. Computed Property
// 이 프로퍼티는 var만 사용이 가능하다.
struct Person1 {
    var firstName: String
    var lastName: String
    
    var fullName: String {
        return "\(firstName) \(lastName)"
    }
}
var person1 = Person1(firstName: "Dongjae", lastName: "Lim")

person1.firstName
person1.lastName
person1.fullName

// 위에서 fullName은 firstName 와 lastName와의 어떠한 관계를 갖고있는데.
// 이 때 관계만 잘 정리해 놓는다면 셋팅도 가능하게 할 수 있다.
// 대신 get set을 사용해줘야한다.

struct Person2 {
    var firstName: String
    var lastName: String
    
    var fullName: String {
        get {
            return "\(firstName) \(lastName)"
        }
        
        set {
            if let firstName = newValue.components(separatedBy: " ").first {
            // newValue 즉 새로운 값이 들어온다면 separatedBy의 값으로 분리한다. 위 코드에서는 공백으로 분리한다.
            // 그리고 그 중에 .first 즉 첫번 째 값이 firstName 이다.
                self.firstName = firstName
                // firstName에 할당된 값을 self.firstName를 통해 get 코드의 값으로 넣어준다.
            }
            
            if let lastName = newValue.components(separatedBy: " ").last {
                self.lastName = lastName
            }
        }
    }
}
var person2 = Person2(firstName: "Dongjae", lastName: "Lim")

person2.firstName
person2.lastName
person2.fullName
// newValue
person2.fullName = "Jay Park"
// 결과 확인
person2.firstName
person2.lastName


// 위의 Stored Property 와 Computed Property 는 인스턴스 프로퍼티이며,
// 아래의 프로퍼티는 Type Property 이다.

// 3. Type Property
struct Person3 {
    var firstName: String
    var lastName: String
    
    var fullName: String {
        get {
            return "\(firstName) \(lastName)"
        }
        
        set {
            if let firstName = newValue.components(separatedBy: " ").first {
            // newValue 즉 새로운 값이 들어온다면 separatedBy의 값으로 분리한다. 위 코드에서는 공백으로 분리한다.
            // 그리고 그 중에 .first 즉 첫번 째 값이 firstName 이다.
                self.firstName = firstName
                // firstName에 할당된 값을 self.firstName를 통해 get 코드의 값으로 넣어준다.
            }
            
            if let lastName = newValue.components(separatedBy: " ").last {
                self.lastName = lastName
            }
        }
    }
    static let isAlien: Bool = false
}
var person3 = Person3(firstName: "Dongjae", lastName: "Lim")

Person3.isAlien
```


## Property 고급내용

### 1. 이름이 바뀐 시점을 찾아 프린트해보자
#### - didSet 사용
#### (값이 변경될 때 실행된다.)

```Swift
    var firstName: String {
        willSet { //옵저베이션(관찰)
            print("willSet: \(firstName) --> \(newValue)")
        }
        didSet { //옵저베이션(관찰)
            print("didSet: \(oldValue) --> \(firstName)")
        }
    }
```

### 2. lazy Property
#### - 인스턴스가 생성될 때 실행되는 것이 아닌 해당 프로퍼티에 접근할 때 실행되는 프로퍼티이다.
#### (lazy프로퍼티는 엔지니어링 측면에서 효율적인 최적화를 위해 사용한다.)

```Swift
    lazy var isPopular: Bool = {
        if fullName == "Jay Park" {
            return true
        } else {
            return false
        }
    }()
```

### 고급내용 코드
```Swift
// 프로퍼티 고급

// 이름이 바뀐 시점을 찾아 프린트해보자
struct Person {
    var firstName: String {
        willSet { //옵저베이션(관찰)
            print("willSet: \(firstName) --> \(newValue)")
        }
        didSet { //옵저베이션(관찰)
            print("didSet: \(oldValue) --> \(firstName)")
        }
    }
    var lastName: String
    
    lazy var isPopular: Bool = {
        if fullName == "Jay Park" {
            return true
        } else {
            return false
        }
    }()
    
    var fullName: String {
        get {
            return "\(firstName) \(lastName)"
        }
        
        set {
            if let firstName = newValue.components(separatedBy: " ").first {
            // newValue 즉 새로운 값이 들어온다면 separatedBy의 값으로 분리한다. 위 코드에서는 공백으로 분리한다.
            // 그리고 그 중에 .first 즉 첫번 째 값이 firstName 이다.
                self.firstName = firstName
                // firstName에 할당된 값을 self.firstName를 통해 get 코드의 값으로 넣어준다.
            }
            
            if let lastName = newValue.components(separatedBy: " ").last {
                self.lastName = lastName
            }
        }
    }
    static let isAlien: Bool = false
}
var person = Person(firstName: "Dongjae", lastName: "Lim")

person.firstName
person.lastName
person.fullName
// newValue
person.fullName = "Jay Park"
// 결과 확인
person.firstName
person.lastName

person.isPopular
```


### 2. Computed Property - 추가 내용
#### - 복잡한 Computed Property의 설명 보충

```Swift
struct Person1 {
    var firstName: String
    var lastName: String
    
    //이것은 Computed Property
   var fullName: String {
        return "\(firstName) \(lastName)"
    }
    // 이것은 Method
    func fullName() -> String {
        return "\(firstName) \(lastName)"
    }
}
```
#### 과연 위에서 보는 것과 같이
#### Computed Property 와 Method 는 똑같은 일을 할 수 있는데
#### 무엇이 더 좋은 방법일까?

### __Property__ vs __Method__
#### Property
- 호출시 (저장된)값을 하나 반환한다!!

#### Method
- 호출시 어떤 작업을 한다.

### 만약에 Method가 그냥 값을 리턴하는 작업을 한다면?
### 이에 따른 기준을 정해볼 수 있겠다.

Setter가 필요한가?
네 -> Computed Property
아니오 -> 2번째 질문
계산이 많이 필요한가? 혹은 DB access나 File io가 필요한가?
네 -> Method
아니오 -> Computed Property