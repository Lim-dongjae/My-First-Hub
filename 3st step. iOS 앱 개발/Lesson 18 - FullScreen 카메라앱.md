#아이폰 개발 시작하기

#주제 - iOS 앱 개발 #패스트 캠퍼스 

---
 
18강 스위프트 __FullScreen 카메라 앱__
#### 카메라앱은 시뮬레이터로 돌려볼 수 없다.
#### 가지고 있는 아이폰에 올려서 돌려봐야한다.

## 사이드 로딩
### - 스위프트로 만들어본 앱을 아이폰에서 실행해보는 것.

### 사이드 로딩 문제 발생(iOS 13.4 이후 부터 발생한다)
#### 서드파티 라이브러리가 연결되어있을 경우 로드에 오류가 발생한다.
#### 이 땐 아래와 같이 코드를 수정해준다.
#### 그 다음에 스위프트 패키지 매니저에서 서드파티를 지운 후
#### cocoaPods에서 다시 가져온다.
#### 그 이후 터미널에서 인스톨해준다.
```Swift
// cocoaPod - Pods - Podfile
#  use_frameworks! <- 이 코드를 아래 코드로 바꿔준다.
  use_modular_headers!

pod 'Kingfisher', '~> 5.0' <- 추가
```

## AVFoundation
#### 오디오, 미디어 등 많은 기능을 포함하고 있는
#### AVFoundation을 꼭 사용해야한다.


## 커스텀 카메라
#### 커스텀 카메라를 사용하는 경우는 아래와 같다.
#### 1. 커스텀 UI를 만들고 싶을 때
#### 2. 필터 또는 워터마크를 넣고싶을 때
#### 3. 영상 데이터, AR을 넣고싶을 때


## 기본 카메라
#### 기본적인 카메라(시스템 카메라)만 사용할 땐 UIImagePickerController를 사용한다.


## 커스텀 카메라 - Media Capture
### 미디어 캡쳐링에서 알아야하는 구조 및 주요 구성요소
#### 1. AVCaptureSession
#### - 카메라, 마이크 인풋에서 들어오는 비디오, 오디오 데이터를 아웃풋에 연결시켜주는 중간 역할

#### 2. AVCaptureDeviceInput
#### - 미디어 소스를 제공해주는 카메라, 마이크

#### 3. AVCAptureOutput
#### - 인풋에서 들어오는 데이터를 사용하는 과정

```Swift
class PreviewView: UIView {
    var videoPreviewLayer: AVCaptureVideoPreviewLayer {
        guard let layer = layer as? AVCaptureVideoPreviewLayer else {
            fatalError("Expected `AVCaptureVideoPreviewLayer` type for layer. Check PreviewView.layerClass implementation.")
        }
        
        layer.videoGravity = AVLayerVideoGravity.resizeAspectFill
        layer.connection?.videoOrientation = .portrait
        return layer
    }
    
    var session: AVCaptureSession? {
        get {
            return videoPreviewLayer.session
        }
        set {
            videoPreviewLayer.session = newValue
        }
    }
    
    // MARK: UIView
    
    override class var layerClass: AnyClass {
        return AVCaptureVideoPreviewLayer.self
    }
}
```