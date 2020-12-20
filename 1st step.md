#아이폰 개발 시작하기1
2020년 12월 20일

#주제 - ios 로티 애니메이션

class MainViewController: UIViewController {

    let titleLabel: UIlabel = {
        let label = UILabel()
         label.textColor = .black
         label.textAlihment = .center
         label.text = "메인화면"
         label.font = UIFont.boldSystemFont(ofSize: 70)
         return label
    }
}

//뷰가 생성되었을 때
override func viewDidLoad() {
    super.viewDidLoad()
    // Do any additional setup after loading the view.

    view.addSubview(titleLabel)

    titleLabel.translatesAutoresizingMaskIntoConstraints = False
    titleLabel.centerXAnchor.constraint(equalTo: view.centerXAnchor).isActive = true
    titleLabel.centerYAnchor.constraint(equalTo: view.centerYAnchor).isActive = true
}

코멘트
코코아팟을 설치해야하고 아직 코드작성 및 이해도가 떨어지기 때문에
여기까지만 한다.