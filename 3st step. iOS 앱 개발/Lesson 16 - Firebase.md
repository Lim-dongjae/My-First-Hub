#아이폰 개발 시작하기

#주제 - iOS 앱 개발 #패스트 캠퍼스 

---
 
16강 스위프트 __Firebase__


## Firebase
### 서버를 제공해주는 서비스
#### 1. 데이터 저장
#### 2. 실시간 데이터 동기화
#### 3. 사용자 인증
#### 4. 데이터 분석
#### 5. A/B Testing

### Firebase에서 제공하는 3가지 큰 기능
#### 1. Build better apps
- 제공해주는 서비스를 사용하여 굉장히 빠르고 쉽게 개발할 수 있도록 도와준다.

#### 2. Improve app quality
- 앱의 퀄리티를 관리해주는 기능을 제공해준다.

#### 3. Grow your app
- 비지니스 성장을 도모하는 기능을 제공해준다.


## CocoaPods
#### 외부 라이브러리 관리 모듈

### Cocoapods 설치 - Firebase 콘솔로 접속 - Pod init - Pod install - 사용


## 데이터베이스에서 데이터 가져오기
### Firebase 콘솔 - RealtimeDatabase - 데이터베이스 생성 - Swift Pod 파일에 입력 - 터미널에서 Podinstall - 데이터베이스접속 - 생성
### Swift에 inport
```Swift
// 데이터베이스에서 데이터 가져오기
import UIKit
import Firebase

class ViewController: UIViewController {
    
    let db = Database.database().reference()
    @IBOutlet weak var dataLabel: UILabel!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view.
        updateLabel()
    }
    
    // 데이터 가져오기
    func updateLabel() {
        db.child("firstData").observeSingleEvent(of: .value) { snapshot in
            print("--> \(snapshot)")
            
            let value = snapshot.value as? String ?? ""
            
            DispatchQueue.main.async {
                self.dataLabel.text = value
            }
        }
    }
}
```

## 데이터 추가하기
```Swift
class ViewController: UIViewController {
    
    let db = Database.database().reference()
    @IBOutlet weak var dataLabel: UILabel!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view.
        updateLabel()
        saveBasicTypes()
        saveCostomers()
    }
    
    // 데이터 가져오기
    func updateLabel() {
        db.child("firstData").observeSingleEvent(of: .value) { snapshot in
            print("--> \(snapshot)")
            
            let value = snapshot.value as? String ?? ""
            
            DispatchQueue.main.async {
                self.dataLabel.text = value
            }
        }
    }
}

// 데이터 추가하기
extension ViewController {
    func saveBasicTypes() {
        // Firebase child("Key").setValue(Value)
        // - string, number, dictionary, array
        
        db.child("int").setValue(3)
        db.child("double").setValue(3.5)
        db.child("str").setValue("안녕하세요")
        db.child("array").setValue(["a", "b", "c"])
        db.child("dict").setValue(["id": "anyID", "age": "28", "city": "Seoul"])
    }
    
    func saveCostomers() {
        // 책가게
        // 사용자를 저장
        // 모델 Customer + Book
        
        let books = [Book(title: "아프니까 청춘이다", author: "김난도"), Book(title: "그런 사람 또 없습니다", author: "원태연")]
        
        let Customer1 = Customer(id: "\(Customer.id)", name: "Son", books: books)
        Customer.id += 1
        
        let Customer2 = Customer(id: "\(Customer.id)", name: "Dele", books: books)
        Customer.id += 1
        
        let Customer3 = Customer(id: "\(Customer.id)", name: "Kane", books: books)
        Customer.id += 1
        
        db.child("customers").child(Customer1.id).setValue(Customer1.toDictionary)
        db.child("customers").child(Customer2.id).setValue(Customer2.toDictionary)
        db.child("customers").child(Customer3.id).setValue(Customer3.toDictionary)
    }
}

struct Customer {
    let id: String
    let name: String
    let books: [Book]
    
    var toDictionary: [String: Any] {
        let booksArray = books.map { $0.toDictionary }
        let dict: [String: Any] = ["id": id, "name": name, "books": booksArray]
        return dict
    }
    
    static var id: Int = 0
}

struct Book {
    let title: String
    let author: String
    
    var toDictionary: [String: Any] {
        let dict: [String: Any] = ["title": title, "author": author]
        return dict
    }
}
```

## 데이터 파싱하기
```Swift
// MARK: Read(Fatch) Data
extension ViewController {
    func fetchCustomers() {
        db.child("customers").observeSingleEvent(of: .value) { snapshot in
            print("--> \(snapshot.value)")
            
            do {
                let data = try JSONSerialization.data(withJSONObject: snapshot.value, options: [])
                let decoder = JSONDecoder()
                let customers: [Customer] = try decoder.decode([Customer].self, from: data)
                DispatchQueue.main.async {
                    self.numOfCustomers.text = "# of Customers: \(customers.count)"
                }
            } catch let error {
                print("---> error: \(error.localizedDescription)")
            }
        }
    }
}
```

## 데이터 수정 및 삭제
```Swift
// 수행하는 코드
override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view.
        updateLabel()
//        saveBasicTypes()
//        saveCostomers()
        fetchCustomers()
        
        // update, delete 둘 중 하나만 써야한다 (업데이트되고 삭제되면 안되니까)
        updateBasicTypes()
//        deleteBasicTypes()
    }

// 업데이트, 딜리트 코드
extension ViewController {
    func updateBasicTypes() {
        db.updateChildValues(["int" : 6])
        db.updateChildValues(["double" : 6.1])
        db.updateChildValues(["str" : "업데이트 실험"])
        
    }
    
    func deleteBasicTypes() {
        db.child("int").removeValue()
        db.child("double").removeValue()
        db.child("str").removeValue()
    }
}
```

## 데이터 업데이트를 버튼에 연결하여 사용해보기
```Swift
// 기본코드
class ViewController: UIViewController {
    
    @IBOutlet weak var dataLabel: UILabel!
    @IBOutlet weak var numOfCustomers: UILabel!
    let db = Database.database().reference()
    var customers: [Customer] = [] // 추가된 코드
    
    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view.
        updateLabel()
        fetchCustomers()
    }

// 버튼 연결
@IBAction func createCustomer(_ sender: Any) {
        saveCostomers()
    }
    
    @IBAction func fetchCustomer(_ sender: Any) {
        fetchCustomers()
    }
    
    func updateCustomer() {
        guard customers.isEmpty == false else { return }
        customers[0].name = "Min"
        
        let dictionary = customers.map { $0.toDictionary }
        db.updateChildValues(["customers" : dictionary])
    }
    
    @IBAction func updateCustomer(_ sender: Any) {
        updateCustomer()
    }
    
    func deleteCustomer() {
        db.child("customers").removeValue()
    }
    
    @IBAction func deleteCustomer(_ sender: Any) {
        deleteCustomer()
    }
}

// fetch
extension ViewController {
    func fetchCustomers() {
        db.child("customers").observeSingleEvent(of: .value) { snapshot in
            print("--> \(snapshot.value)")
            
            do {
                let data = try JSONSerialization.data(withJSONObject: snapshot.value, options: [])
                let decoder = JSONDecoder()
                let customers: [Customer] = try decoder.decode([Customer].self, from: data)
                self.customers = customers // 추가된 코드
                DispatchQueue.main.async {
                    self.numOfCustomers.text = "# of Customers: \(customers.count)"
                }
            } catch let error {
                print("---> error: \(error.localizedDescription)")
            }
        }
    }
}
```

## 총 뷰컨트롤러 코드
```Swift
class ViewController: UIViewController {
    
    @IBOutlet weak var dataLabel: UILabel!
    @IBOutlet weak var numOfCustomers: UILabel!
    let db = Database.database().reference()
    var customers: [Customer] = []
    
    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view.
        updateLabel()
        fetchCustomers()
    }
    
    // 데이터 가져오기
    func updateLabel() {
        db.child("firstData").observeSingleEvent(of: .value) { snapshot in
            print("--> \(snapshot)")
            
            let value = snapshot.value as? String ?? ""
            
            DispatchQueue.main.async {
                self.dataLabel.text = value
            }
        }
    }
    
    @IBAction func createCustomer(_ sender: Any) {
        saveCostomers()
    }
    
    @IBAction func fetchCustomer(_ sender: Any) {
        fetchCustomers()
    }
    
    func updateCustomer() {
        guard customers.isEmpty == false else { return }
        customers[0].name = "Min"
        
        let dictionary = customers.map { $0.toDictionary }
        db.updateChildValues(["customers" : dictionary])
    }
    
    @IBAction func updateCustomer(_ sender: Any) {
        updateCustomer()
    }
    
    func deleteCustomer() {
        db.child("customers").removeValue()
    }
    
    @IBAction func deleteCustomer(_ sender: Any) {
        deleteCustomer()
    }
}

// 데이터 추가하기
extension ViewController {
    func saveBasicTypes() {
        // Firebase child("Key").setValue(Value)
        // - string, number, dictionary, array
        
        db.child("int").setValue(3)
        db.child("double").setValue(3.5)
        db.child("str").setValue("안녕하세요")
        db.child("array").setValue(["a", "b", "c"])
        db.child("dict").setValue(["id": "anyID", "age": "28", "city": "Seoul"])
    }
    
    func saveCostomers() {
        // 책가게
        // 사용자를 저장
        // 모델 Customer + Book
        
        let books = [Book(title: "아프니까 청춘이다", author: "김난도"), Book(title: "그런 사람 또 없습니다", author: "원태연")]
        
        let Customer1 = Customer(id: "\(Customer.id)", name: "Son", books: books)
        Customer.id += 1
        
        let Customer2 = Customer(id: "\(Customer.id)", name: "Dele", books: books)
        Customer.id += 1
        
        let Customer3 = Customer(id: "\(Customer.id)", name: "Kane", books: books)
        Customer.id += 1
        
        db.child("customers").child(Customer1.id).setValue(Customer1.toDictionary)
        db.child("customers").child(Customer2.id).setValue(Customer2.toDictionary)
        db.child("customers").child(Customer3.id).setValue(Customer3.toDictionary)
    }
}

// MARK: Read(Fatch) Data
extension ViewController {
    func fetchCustomers() {
        db.child("customers").observeSingleEvent(of: .value) { snapshot in
            print("--> \(snapshot.value)")
            
            do {
                let data = try JSONSerialization.data(withJSONObject: snapshot.value, options: [])
                let decoder = JSONDecoder()
                let customers: [Customer] = try decoder.decode([Customer].self, from: data)
                self.customers = customers
                DispatchQueue.main.async {
                    self.numOfCustomers.text = "# of Customers: \(customers.count)"
                }
            } catch let error {
                print("---> error: \(error.localizedDescription)")
            }
        }
    }
}

extension ViewController {
    func updateBasicTypes() {
        db.updateChildValues(["int" : 6])
        db.updateChildValues(["double" : 6.1])
        db.updateChildValues(["str" : "업데이트 실험"])
        
    }
    
    func deleteBasicTypes() {
        db.child("int").removeValue()
        db.child("double").removeValue()
        db.child("str").removeValue()
    }
}

struct Customer: Codable {
    let id: String
    var name: String
    let books: [Book]
    
    var toDictionary: [String: Any] {
        let booksArray = books.map { $0.toDictionary }
        let dict: [String: Any] = ["id": id, "name": name, "books": booksArray]
        return dict
    }
    
    static var id: Int = 0
}

struct Book: Codable {
    let title: String
    let author: String
    
    var toDictionary: [String: Any] {
        let dict: [String: Any] = ["title": title, "author": author]
        return dict
    }
}

```