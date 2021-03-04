#아이폰 개발 시작하기

#주제 - iOS 앱 개발 #패스트 캠퍼스 

---
 
8강 스위프트 __Class__
#### Class는 Structure와 많이 닮아 있다.
#### Class는 관계가 있는 것들을 묶어서 표현한 것이다.
#### Object = Data + Method
#### Object를 Structure와 Class로 표현할 수 있다.

### 두개는 무엇이 다른것일까?
#### Structure와 Class는 동작이 다르다.
#### Structure는 Value Types이고 값을 Copy하여 사용한다.
#### Class는 Reference Types이고 값을 참조(공유)하여 사용한다.
#### Class는 Heap 공간에 생성되고 Structure는 Stack 공간에 생성된다.

### Heap -> (느리다.)
### Stack -> 시스템에서 당장 실행해야하거나 타이트하게 관리를 해야하는 녀석들은 Stack에 생성된다. (효율적이고 빠르다.)

```Swift
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
```

## Class 실습

```Swift
struct PersonStruct {
    var firstName: String
    var lastName: String
    
    var fullName: String {
        return "\(firstName) \(lastName)"
    }
    
    mutating func uppercaseName() {
    //스트럭트에서 어떤 메소드를 통해 자기 스스로의 스토드 프로퍼티를 변경하는 경우에는 mutating 키워드를 사용해야만 한다.
        firstName = firstName.uppercased()
        lastName = lastName.uppercased()
    }
}


// 위의 스트럭트와 거의 비슷한 Class를 만들어보자

class PersonClass {
    var firstName: String
    var lastName: String
    
    init(firstName: String, lastName: String) {
    // 클래스 객체를 생성할 때 사용하는 생성 함수이다.
    // Class에서 사용하지 않으면 오류가 발생하지만, Struct에서는 사용하지 않아도된다.
        self.firstName = firstName
        self.lastName = lastName
    }
    
    var fullName: String {
        return "\(firstName) \(lastName)"
    }
    
    func uppercaseName() {
    //스트럭트에서 어떤 메소드를 통해 자기 스스로의 스토드 프로퍼티를 변경하는 경우에는 mutating 키워드를 사용해야만 한다.
    //하지만 Class에서는 사용하지 않는다.
        firstName = firstName.uppercased()
        lastName = lastName.uppercased()
    }
}


var personStruct1 = PersonStruct(firstName: "Jason", lastName: "Lee")
var personStruct2 = personStruct1

var personClass1 = PersonClass(firstName: "Jason", lastName: "Lee")
var personClass2 = personClass1


personStruct2.firstName = "Jay"
personStruct1.firstName
personStruct2.firstName


personClass2.firstName = "Jay"
personClass1.firstName
personClass2.firstName



personClass2 = PersonClass(firstName: "Bob", lastName: "Lee")
personClass1.firstName
personClass2.firstName

personClass1 = personClass2
personClass1.firstName
personClass2.firstName
```


## Class vs Struct
### 언제, 무엇을 써야할까?

###  이럴 땐 Struct를 쓰자!
#### 1. 두 Object를 "같다, 다르다"로 비교해야 하는 경우
```Swift
let point1 = Point(x: 3, y: 5)
let point2 = Point(x: 3, y: 5)
// 두개의 포인트라는 변수가 다른 객체이지만 서로 비교한다고 했을 때
// 포인트 x, y가 같으니 같다고 판단해야한다.
// 이 처럼 데이터 자체를 비교할 필요가 있을 땐 Struct를 사용한다.
```

#### 2. Copy된 각 객체들이 독립적인 상태를 가져야 하는 경우
```Swift
var myMac = Mac(owner: "Jason")
var yourMac = myMac
yourMac.owner = "Jay"

myMac.owner
yourMac.owner
// Mac이라는 객체가 있을 때 복사했을 때 각자 다른 소유자를 지정하는 것 처럼
// 각 객체가 다르게 관리되어야하는 경우에는 Struct를 사용한다.
```

#### 3. 코드에서 오브젝트의 데이터를 여러 스레드에 걸쳐 사용할 경우


###  이럴 땐 Class를 쓰자!
#### 1. 두 Object의 인스턴스 자체가 같음을 확인해야 할 때

#### 2. 하나의 객체가 필요하고, 여러 대상에 의해 접근되고 변경이 필요한 경우


### 단순한 설명
#### 1. 일단 Struct로 쓰자.
#### - 그리고 필요하다면 Struct를 Class로 바꿔주면된다.
#### - Swift는 Struct를 더 좋아한다.



## Class 상속개념 코드로 바로 배우기
```Swift
// 처음 주어진 코드
//struct Grade {
//    var letter: Character
//    var points: Double
//    var credits: Double
//}
//
//class Person {
//    var firstName: String
//    var lastName: String
//
//    init(firstName: String, lastName: String) {
//        self.firstName = firstName
//        self.lastName = lastName
//    }
//
//    func printMyName() {
//        print("My name is \(firstName) \(lastName)")
//    }
//}
//
//class Student {
//    var grades: [Grade] = []
//
//    var firstName: String
//    var lastName: String
//
//    init(firstName: String, lastName: String) {
//        self.firstName = firstName
//        self.lastName = lastName
//    }
//
//    func printMyName() {
//        print("My name is \(firstName) \(lastName)")
//    }
//}

//상속의 개념
struct Grade {
    var letter: Character
    var points: Double
    var credits: Double
}

class Person {
    var firstName: String
    var lastName: String

    init(firstName: String, lastName: String) {
        self.firstName = firstName
        self.lastName = lastName
    }

    func printMyName() {
        print("My name is \(firstName) \(lastName)")
    }
}

class Student: Person {
    var grades: [Grade] = []
}


let jay = Person(firstName: "Jay", lastName: "Lee")
let jason = Student(firstName: "Jasson", lastName: "Lee")

jay.firstName
jason.firstName

jay.printMyName()
jason.printMyName()

let math = Grade(letter: "B", points: 8.5, credits: 3)
let history = Grade(letter: "A", points: 10.0, credits: 3)

jason.grades.append(math)
jason.grades.append(history)
//jay.grades

jason.grades.count

// 상속의 개념에 잠깐 알아보자
// Person: Super Class (Parent Class)
// Student: Sub Class (Child Class)

// 상속의 규칙
// - 자식은 한개의 SuperClass만 상속 받음
// - 부모는 여러 자식들을 가질 수 있음
// - 상속의 깊이는 상관이 없음



// 학생인데 운동선수
class StudentAthlete: Student {
    var minimumTrainingTime: Int = 2
    var trainedTime: Int = 0

    func train() {
        trainedTime += 1
    }
}

// 운동선수인데 축구선수
class FootballPlayer: StudentAthlete {
    var footballTeam = "FC Swift"

    override func train() {
    // 상속받은 함수의 내용을 수정할 때(덮어씌울 때) override를 사용한다.
        trainedTime += 2
    }
}

// Person > Student > Athelete > Football Player

var athelete1 = StudentAthlete(firstName: "Yuna", lastName: "Kim")
var athelete2 = FootballPlayer(firstName: "Heung", lastName: "Son")

athelete1.firstName
athelete2.firstName

athelete1.grades.append(math)
athelete2.grades.append(math)

athelete1.minimumTrainingTime
athelete2.minimumTrainingTime

//athelete1.footballTeam
athelete2.footballTeam

athelete1.train()
athelete1.trainedTime

athelete2.train()
athelete2.trainedTime



athelete1 = athelete2 as StudentAthlete
athelete1.train()
athelete1.trainedTime


if let son = athelete1 as? FootballPlayer {
    print("--> team:\(son.footballTeam)")
}
```


## Class 상속은 언제 사용할까?
### 1. Single Responsiblility (단일 책임)
### - 클래스의 깊이가 너무 깊어지면 복잡해지니 단일적인 상속을 하는 것이 좋다.

### 2. Type Safety 
### - 타입이 분명해야 할 때

### 3. Shared Base Classes
### - 다자녀가 있을 때

### 4. Extensibility
### - 확장성이 필요한 경우

### 5. Identity
### - 정체를 파악하기 위해



## Class 생성자 이해하기
### initializer (생성자)
#### 이니셜 라이저는 아직 이해가되지 않으니 추가적인 공부가 필요하다.
```Swift
// 처음 코드
//struct Grade {
//    var letter: Character
//    var points: Double
//    var credits: Double
//}
//
//class Person {
//    var firstName: String
//    var lastName: String
//
//    init(firstName: String, lastName: String) {
//        self.firstName = firstName
//        self.lastName = lastName
//    }
//
//    func printMyName() {
//        print("My name is \(firstName) \(lastName)")
//    }
//}
//
//class Student: Person {
//    var grades: [Grade] = []
//}
//
//// 학생인데 운동선수
//class StudentAthlete: Student {
//    var minimumTrainingTime: Int = 2
//    var trainedTime: Int = 0
//
//    func train() {
//        trainedTime += 1
//    }
//}
//
//// 운동선인데 축구선수
//class FootballPlayer: StudentAthlete {
//    var footballTeam = "FC Swift"
//
//    override func train() {
//        trainedTime += 2
//    }
//}
//


struct Grade {
    var letter: Character
    var points: Double
    var credits: Double
}

class Person {
    var firstName: String
    var lastName: String

    init(firstName: String, lastName: String) {
        self.firstName = firstName
        self.lastName = lastName
    }

    func printMyName() {
        print("My name is \(firstName) \(lastName)")
    }
}

class Student: Person {
    var grades: [Grade] = []
    
    override init(firstName: String, lastName: String) {
        super.init(firstName: firstName, lastName: lastName)
    }
    
    convenience init(student: Student) {
        self.init(firstName: student.firstName, lastName: student.lastName)
    }
}

// -----
// 2-Phase Initialization ( 클래스 생성시의 2단계)
// Phase 1
// 모든 스토드 프로퍼티는 이니셜라이즈 되어야한다.
// 자식 클래스의 프로퍼티부터 이니셜라이즈 되어야한다.
// 페이즈1이 끝나기 전에는 어떠한 메소드나 프로퍼티를 사용할 수 없다.

// Phase 2
// 부모 프로퍼티의 셋팅을 끝내야 프로퍼티나 메소드를 사용할 수 있다.

// 위와 같은 규칙이 없다면
// 이니셜라이저에서 프로퍼티가 셋팅도 안된 상태에서
// 인스턴스 메소드를 호출하게 되고
// 이 경우 원하는대로 작동하지 않고
// 버그가 발생할 수 있다.

// 어떻게 작성하는지 알아보자
// -----

// 학생인데 운동선수
class StudentAthlete: Student {
    var minimumTrainingTime: Int = 2
    var trainedTime: Int = 0
    var sports: [String]
    
    init(firstName: String, lastName: String, sports: [String]) {
    // Phase1
        self.sports = sports
        super.init(firstName: firstName, lastName: lastName)
        
        // Phase2
        self.train()
        // 위 코드가 Phase1 구간으로 가면 오류가 발생한다.
    }
    
    // 이니셜 라이저가 커질 경우에 파라미터가 많아질 수 있다.
    // 이 경우에 간단하게 만드는 방법이 있다.
    // 컨비니언스 이니셜라이즈
    convenience init(name: String) {
        self.init(firstName: name, lastName: "", sports: [])
    }

    func train() {
        trainedTime += 1
    }
}



// 운동선인데 축구선수
class FootballPlayer: StudentAthlete {
    var footballTeam = "FC Swift"

    override func train() {
        trainedTime += 2
    }
}

let student1 = Student(firstName: "Jason", lastName: "Lee")
let student2 = StudentAthlete(firstName: "Jay", lastName: "Lee", sports: ["Football"])

// 컨비니언스 이니셜라이즈
let student3 = StudentAthlete(name: "Mike")

let student1_1 = Student(student: student1)



// designated vs convenience
// 지정이니셜라이저 vs 간편이니셜라이저

// 규칙
// designated는 자신의 부모의 designated를 호출해야한다.
// convenience는 같은 클래스의 이니셜라이저를 꼭 하나 호출해야한다.
// convenience는 궁극적으로는 designated를 호출해야한다.
```