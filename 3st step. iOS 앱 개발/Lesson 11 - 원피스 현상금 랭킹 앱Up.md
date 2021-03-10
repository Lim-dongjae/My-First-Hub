#아이폰 개발 시작하기

#주제 - iOS 앱 개발 #패스트 캠퍼스 

---
 
11강 스위프트 __원피스 현상금 랭킹 앱 만들기 Up__

## CollectionView
### 여러개의 아이템을 한 화면에 보여주는 것

### 아이템을 리스트 형태로 보여주는 것 -> TableView
#### TableView은 표현에 한계가 있다. 바로 열이 한개라는 점 이다.

### 컬렉션뷰는 한행안에 여러개의 아이템들이 들어갈 수 있다.

### 기본적으로 UITableView에 비해서 데이터의 나열이 매우 자유롭기 떄문에
### 많은 아이템을 보여주는 뷰에서 컬렉션뷰는 항상 고려하게된다.

### 컬렉션뷰는 자유로운 레이아웃 때문에 레이아웃에서 전문적으로 관리하는 객체가 필요하다.
### 그것은 바로 UICollectionViewLayout 이다.
우리는 UICollectionViewLayout를 상속받아서 우리만에 객체를 만들 수 있다.
기본적으로 애플에서 제공해주는 레이아웃 객체는 UICollectionViewFlowLayout 이다.
간단한 그리드 형태의 레이아웃은 UICollectionViewFlowLayout으로도 쉽게 만들 수 있다.

```Swift
//    아래 3개를 만들어보자
//    UICollectionViewDataSource
//    몇개를 보여줄까?
    func collectionView(_ collectionView: UICollectionView, numberOfItemsInSection section: Int) -> Int {
        return viewModel.numOfBountyInfoList
        
    }
//    셀을 어떻게 표현할까?
    func collectionView(_ collectionView: UICollectionView, cellForItemAt indexPath: IndexPath) -> UICollectionViewCell {
        
        guard let cell = collectionView.dequeueReusableCell(withReuseIdentifier: "GridCell", for: indexPath) as?
            GridCell else {
            return UICollectionViewCell()
        }
        
        let bountyInfo = viewModel.bountyInfo(at: indexPath.item)
                cell.update(info: bountyInfo)
        cell.update(info: bountyInfo)
        return cell
    }
    
    
//    UICollectionViewDelegate
//    셀이 클릭되었을 때 어떻게 반응하나?
    func collectionView(_ collectionView: UICollectionView, didSelectItemAt indexPath: IndexPath) {
        print("--> \(indexPath.item)")
        performSegue(withIdentifier: "showDetail", sender: indexPath.item)
    }
class GridCell: UICollectionViewCell {
    @IBOutlet weak var imgView: UIImageView!
    @IBOutlet weak var nameLabel: UILabel!
    @IBOutlet weak var bountyLabel: UILabel!
    
    func update(info: BountyInfo) {
        imgView.image = info.image
        nameLabel.text = info.name
        bountyLabel.text = "\(info.bounty)"
    }
}
```

## Cell Size 계산
```Swift
func collectionView(_ collectionView: UICollectionView, layout collectionViewLayout: UICollectionViewLayout, sizeForItemAt indexPath: IndexPath) -> CGSize {
        let itemSpacing: CGFloat = 10
        // 아이템의 간격은 10이다.
        let textAreaHeight: CGFloat = 65
        // 텍스트 에어리어의 높이는 65이다.
        
        
        let width: CGFloat = (collectionView.bounds.width - itemSpacing)/2
        // 너비는 CGFloat 타입이고 값은 collectionView의 너비와 아이템 간격(10)을 뺀 뒤 2로 나눈 값이다.
        // 팁 : bounds 를 사용하면 사이즈를 알 수 있다. (collectionView.bounds.width -> collectionView의 너비 사이즈)

        let height: CGFloat = width * 10/7 + textAreaHeight
        // 높이는 CGFloat 타입이고 값은 너비 * 10분에 7 + textAreaHeight 이다.

        return CGSize(width: width, height: height)
        // CGSize는 너비는 width이고, 높이는 height이다.
    }
```

## Animation 개념
#### 시간에 따라, 뷰의 상태가 바뀌는 것
#### 애니메이션의 3요소 (시작, 끝, 시간)
사용자의 사용성 개선, 몰입성 증진

### Animation API
#### 애니메이션 사용 시 자주 사용하게 될 코드
```Swift
UIView.animate(
    withDuration: 1.0, // 진행 시간
    animations: {
        layoutIfNeeded() // 애니메이션 클로져(애니메이션 시킬 값)
    }
)
```

## 실습 - 뷰 속성을 이용한 애니메이션 효과주기 1
```Swift
override func viewDidLoad() {
        super.viewDidLoad()
        updateUI()
        // 에니메이션 준비
        prepareAnimation()
    }
    
    // 뷰가 보여졌다는 메서드
    override func viewDidAppear(_ animated: Bool) {
        // 뷰가 보여지면 애니메이션을 진행하자
        super.viewDidAppear(animated)
        // 애니메이션 시작
        showAnimation()
    }
    ㅋ
    // 애니메이션 호출 메서드
    private func prepareAnimation() {
        nameLabelCenterX.constant = view.bounds.width
        // nameLabel 초기 X 위치를 view.bounds.width 값만큼 이동한다.(즉 화면 밖으로 나간다.)
        bountyLabelCenterX.constant = view.bounds.width
        // bountyLabel 초기 X 위치를 view.bounds.width 값만큼 이동한다.(즉 화면 밖으로 나간다.)
    }
    
    private func showAnimation() {
        nameLabelCenterX.constant = 0
        // nameLabel을 원상복구한다 (기본값이 0이였다.)
        bountyLabelCenterX.constant = 0
        // bountyLabel을 원상복구한다 (기본값이 0이였다.)
        
        // 간단한 애니메이션
//        UIView.animate(withDuration: 0.3) {
//            self.view.layoutIfNeeded()
//        }
        
        // 여러 설정이 가능한 애니메이션
//        UIView.animate(withDuration: 0.3,
//                       delay: 0.1,
//                       options: .curveEaseIn,
//                       animations: { self.view.layoutIfNeeded() },
//                       completion: nil)
//
        // 튀기는 효과를 넣는 애니메이션
        UIView.animate(withDuration: 0.3,
                       delay: 0.4,
                       usingSpringWithDamping: 0.6,
                       initialSpringVelocity: 2,
                       options: .allowUserInteraction,
                       animations: { self.view.layoutIfNeeded() },
                       // self.view.layoutIfNeeded() 설명
                       // 오토레이아웃이 있는데 constant를 변경하면
                       // 레이아웃을 다시 설정해줘야한다.
                       // 따라서 레이아웃팅을 하면서 애니메이션을 보여주도록 하는 것이다.
                       completion: nil)
        
        // 이미지에 효과를 주기
        UIView.transition(with: imgView,
                          duration: 0.3,
                          options: .transitionFlipFromLeft,
                          animations: nil,
                          // 여기선 constant는 변경이 없이 때문에 nil값을 넣어준다.
                          completion: nil)
    }
```

## 실습 - 뷰 속성을 이용한 애니메이션 효과주기 2
```Swift
    //뷰모델 선언
    let viewModel = DetailViewModel()

    override func viewDidLoad() {
        super.viewDidLoad()
        updateUI()
        // 에니메이션 준비
        prepareAnimation()
    }
    
    // 뷰가 보여졌다는 메서드
    override func viewDidAppear(_ animated: Bool) {
        // 뷰가 보여지면 애니메이션을 진행하자
        super.viewDidAppear(animated)
        // 애니메이션 시작
        showAnimation()
    }
    
    // 애니메이션 호출 메서드
    private func prepareAnimation() {
        // 새로운 애니메이션을 만들어보자
        nameLabel.transform = CGAffineTransform(translationX: view.bounds.width, y: 0).scaledBy(x: 3, y: 3).rotated(by: 180)
        // nameLabel을 변형한다 -> X좌표는 view.bounds.width 값 만큼, y좌표는 0만큼 이동하고 크기는 x, y 모두 3배씩 커진다 그리고 180도 돌아가있는다.
        bountyLabel.transform = CGAffineTransform(translationX: view.bounds.width, y: 0).scaledBy(x: 3, y: 3).rotated(by: 180)
        // bountyLabel을 변형한다 -> X좌표는 view.bounds.width 값 만큼, y좌표는 0만큼 이동하고 크기는 x, y 모두 3배씩 커진다 그리고 180도 돌아가있는다.
        
        nameLabel.alpha = 0
        // nameLabel의 투명도는 0이다
        bountyLabel.alpha = 0
        // bountyLabel의 투명도는 0이다
    }
    
    private func showAnimation() {
        // 이미지에 효과를 주기
        UIView.transition(with: imgView,
                          duration: 0.3,
                          options: .transitionFlipFromLeft,
                          animations: nil,
                          // 여기선 constant는 변경이 없이 때문에 nil값을 넣어준다.
                          completion: nil)
        
        // 새로운 애니메이션을 만들어보자
        UIView.animate(withDuration: 1,
                       delay: 0,
                       usingSpringWithDamping: 0.6,
                       initialSpringVelocity: 2,
                       options: .allowUserInteraction,
                       animations: {
                        self.view.layoutIfNeeded()
                        self.nameLabel.transform = CGAffineTransform.identity
                        // 위에서 변형된 뷰의 값들을 원상태로 돌려보자
                        self.nameLabel.alpha = 1
                       },
                       completion: nil)
        
        UIView.animate(withDuration: 1,
                       delay: 0.2,
                       usingSpringWithDamping: 0.6,
                       initialSpringVelocity: 2,
                       options: .allowUserInteraction,
                       animations: {
                        self.view.layoutIfNeeded()
                        self.bountyLabel.transform = CGAffineTransform.identity
                        // 위에서 변형된 뷰의 값들을 원상태로 돌려보자
                        self.bountyLabel.alpha = 1
                       },
                       completion: nil)
    }
    
    func updateUI() {
        if let bountyInfo = viewModel.bountyInfo {
            imgView.image = bountyInfo.image
            nameLabel.text = bountyInfo.name
            bountyLabel.text = "\(bountyInfo.bounty)"
        }
    }
```