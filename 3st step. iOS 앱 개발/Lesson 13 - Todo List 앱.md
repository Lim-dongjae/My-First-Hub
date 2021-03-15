#아이폰 개발 시작하기

#주제 - iOS 앱 개발 #패스트 캠퍼스 

---
 
13강 스위프트 __Todo List앱__ - (Any do List 참고)

## 데이터 저장
#### 데이터 저장 방법
- NSCoding // 작고 덜 복잡한 데이터 (복잡한 구석이 있다)
- Property List // 작고 덜 복잡한 데이터
- Serialization
- Core Data // 많고 복잡한 데이터 (구현 난이도가 높고 관리가 어렵다)
- Realm // 많고 복잡한 데이터 (쉽게 사용이 가능하다)
- Codable (Swift 4) // 작고 덜 복잡한 데이터, Json을 아주 손쉽게 다룰 수 있다.
> 파일을 관리한다.

## 탭 바 컨트롤러 만들기
#### 스토리보드 -> 상단 Editor -> Embed In -> Tab Bar Controller

## 스태틱 테이블뷰로 설정페이지 구현하기
#### 스토리보드 -> 오브젝트 추가 -> 테이블 뷰 컨트롤러 -> 메인 컨트롤러 클릭 -> Ctr + 드래그로 생성한 테이블 뷰 컨트롤러에 연결(뷰컨트롤러 옵션)

## 태스크 관리뷰, 컬렉션뷰, todolist셀
```Swift
extension TodoListViewController: UICollectionViewDelegateFlowLayout {
    func collectionView(_ collectionView: UICollectionView, layout collectionViewLayout: UICollectionViewLayout, sizeForItemAt indexPath: IndexPath) -> CGSize {
        // [완] TODO: 사이즈 계산하기
        let width: CGFloat = collectionView.bounds.width
        let height: CGFloat = 50
        return CGSize(width: width, height: height)
    }
}

// TODO: Codable과 Equatable 추가
struct Todo: Codable, Equatable {
    let id: Int
    var isDone: Bool
    var detail: String
    var isToday: Bool
    
    mutating func update(isDone: Bool, detail: String, isToday: Bool) {
        // [완] TODO: update 로직 추가
        // mutating 메서드인 것을 보아 이 코드를 실행하면 위의 스트럭트 내용(프로퍼티)을 바꿀 수 있음을 확인할 수 있다.
        self.isDone = isDone
        self.detail = detail
        self.isToday = isToday
        
    }
    
    static func == (lhs: Self, rhs: Self) -> Bool {
        // [완] TODO: 동등 조건 추가
        // 객체간의 동등비교를 하기 위해서는 위 스트럭트의 프로토콜처럼 Equatable프로토콜을 따라야한다.
        // Equatable를 따르면 위 스태틱처럼 == 를 마음대로 정의할 수 있다.
        return lhs.id == rhs.id
    }
}
```

## 태스크 관리뷰, todoManager
```Swift
class TodoManager {
    
    static let shared = TodoManager()
    // 싱글톤
    
    static var lastId: Int = 0
    
    var todos: [Todo] = []
    
    func createTodo(detail: String, isToday: Bool) -> Todo {
        // [완] TODO: create로직 추가
        let nexId = TodoManager.lastId + 1
        TodoManager.lastId = nexId
        return Todo(id: nexId, isDone: false, detail: detail, isToday: isToday)
    }
    
    func addTodo(_ todo: Todo) {
        // [완] TODO: add로직 추가
        todos.append(todo)
        saveTodo()
    }
    
    func deleteTodo(_ todo: Todo) {
        // [완] TODO: delete 로직 추가
        
        // 방법 1 - 이게 더 효율적일 수 있다.
        if let index = todos.firstIndex(of: todo) {
        // todos에서 인덱스를 찾고
            todos.remove(at: index)
            // todos에서 인덱스를 지워라
        }
        saveTodo()
        /*
        // 방법 2
        todos = todos.filter { existingTodo in
        // todos에서 필터링을 하는데
            return existingTodo.id != todo.id
            // 기존에 todos(existingTodo.id) 에서 todo.id 와 같지 않은 애들만 찾아서 넘겨준다.
        }
        // 방법 2 의 줄인 표현
        todos = todos.filter { $0.id != todo.id }
        */
    }
    
    func updateTodo(_ todo: Todo) {
        // [완] TODO: updatee 로직 추가
        guard let index = todos.firstIndex(of: todo) else { return }
        todos[index].update(isDone: todo.isDone, detail: todo.detail, isToday: todo.isToday)
        saveTodo()
    }
    
    func saveTodo() {
        Storage.store(todos, to: .documents, as: "todos.json")
    }
    
    func retrieveTodo() {
        todos = Storage.retrive("todos.json", from: .documents, as: [Todo].self) ?? []
        
        let lastId = todos.last?.id ?? 0
        TodoManager.lastId = lastId
    }
}

class TodoViewModel {
    
    enum Section: Int, CaseIterable {
        case today
        case upcoming
        
        var title: String {
            switch self {
            case .today: return "Today"
            default: return "Upcoming"
            }
        }
    }
    
    private let manager = TodoManager.shared
    
    var todos: [Todo] {
        return manager.todos
    }
    
    var todayTodos: [Todo] {
        return todos.filter { $0.isToday == true }
    }
    
    var upcompingTodos: [Todo] {
        return todos.filter { $0.isToday == false }
    }
    
    var numOfSection: Int {
        return Section.allCases.count
    }
    
    func addTodo(_ todo: Todo) {
        manager.addTodo(todo)
    }
    
    func deleteTodo(_ todo: Todo) {
        manager.deleteTodo(todo)
    }
    
    func updateTodo(_ todo: Todo) {
        manager.updateTodo(todo)
    }
    
    func loadTasks() {
        manager.retrieveTodo()
    }
}
```

## 태스크 관리뷰, todolist뷰모델
```Swift
class TodoViewModel {
    
    enum Section: Int, CaseIterable {
        case today
        case upcoming
        
        var title: String {
            switch self {
            case .today: return "Today"
            default: return "Upcoming"
            }
        }
    }
    
    private let manager = TodoManager.shared
    
    var todos: [Todo] {
        return manager.todos
    }
    
    var todayTodos: [Todo] {
        return todos.filter { $0.isToday == true }
    }
    
    var upcompingTodos: [Todo] {
        return todos.filter { $0.isToday == false }
    }
    
    var numOfSection: Int {
        return Section.allCases.count
    }
    
    func addTodo(_ todo: Todo) {
        manager.addTodo(todo)
    }
    
    func deleteTodo(_ todo: Todo) {
        manager.deleteTodo(todo)
    }
    
    func updateTodo(_ todo: Todo) {
        manager.updateTodo(todo)
    }
    
    func loadTasks() {
        manager.retrieveTodo()
    }
}
```

## 컬렉션뷰 구현하기
```Swift
func updateUI(todo: Todo) {
        // [완] TODO: 셀 업데이트 하기
        checkButton.isSelected = todo.isDone
        descriptionLabel.text = todo.detail
        descriptionLabel.alpha = todo.isDone ? 0.2 : 1
        deleteButton.isHidden = todo.isDone == false
        showStrikeThrough(todo.isDone)
    }
    
    private func showStrikeThrough(_ show: Bool) {
        if show {
            strikeThroughWidth.constant = descriptionLabel.bounds.width
        } else {
            strikeThroughWidth.constant = 0
        }
    }
    
    func reset() {
        // [완] TODO: reset로직 구현
        descriptionLabel.alpha = 1
        deleteButton.isHidden = true
        showStrikeThrough(false)
        
    }
    
    @IBAction func checkButtonTapped(_ sender: Any) {
        // [완] TODO: checkButton 처리
        checkButton.isSelected = !checkButton.isSelected // !표시는 반대라는 뜻이다.
        let isDone = checkButton.isSelected
        showStrikeThrough(isDone)
        descriptionLabel.alpha = isDone ? 0.2 : 1
        deleteButton.isHidden = !isDone
        
        doneButtonTapHandler?(isDone)
    }
    
    @IBAction func deleteButtonTapped(_ sender: Any) {
        // [완] TODO: deleteButton 처리
        deleteButtonTapHandler?()
    }
}
```

## Storage
```Swift

```