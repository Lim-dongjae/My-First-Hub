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