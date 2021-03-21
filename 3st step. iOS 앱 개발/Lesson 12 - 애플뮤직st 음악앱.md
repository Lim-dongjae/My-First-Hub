#아이폰 개발 시작하기

#주제 - iOS 앱 개발 #패스트 캠퍼스 

---
 
12강 스위프트 __애플뮤직st 음악앱__
복습: 20.03.21

## 뮤직앱 구현 계획
1. 메인화면
> 화면 구성을 어떻게 할지?
> 곡은 어떻게 로드할 것인지?
> 곡들은 어떻게 표시할 것인지?

2. 플레이어 페이지 구성
> 화면 구성을 어떻게 할지?
> 재생을 위한 곡 준비는 어떻게 할지?
> 곡은 어떻게 재생할지?

## AVFoundation
#### 음악 재생을 포함한 모든 미디어 작업을 할 땐 AVFoundation 프레임워크(공간)을 이용하게된다.
#### 대표적인 AVFoundation 프레임워크(공간)을 사용한 앱
#### > 카메라
#### > 비디오
#### > 음악

## AVPlayer
#### 미디어를 플레이 시켜주는 객체
1. 재생 및 정지를 어떻게 해야할지?
2. 슬라이더를 이용한 구간 시핑은 어떻게 할지?

## DarkMode
#### 다크모드를 구현해보자

## CollectionReusableView (섹션헤더뷰)
#### 메인화면 구성에 헤더 구현

## 실습 - 트랙 리스트 만들기
```Swift
import AVFoundation

class TrackManager {
    // TODO: 프로퍼티 정의하기 - 트랙들, 앨범들, 오늘의 곡
    var tracks: [AVPlayerItem] = []
    var albums: [Album] = []
    var todaysTrack: AVPlayerItem?
    
    // TODO: 생성자 정의하기
    init() {
        let tracks = loadTracks()
        self.tracks = tracks
        self.albums = loadAlbums(tracks: tracks)
        self.todaysTrack = self.tracks.randomElement()
    }

    // TODO: 트랙 로드하기
    func loadTracks() -> [AVPlayerItem] {
        // 파일들을 읽어서 AVPlayerItem 만들기
        let urls = Bundle.main.urls(forResourcesWithExtension: "mp3", subdirectory: nil) ?? []
        let items = urls.map { url in
            return AVPlayerItem(url: url)
        }
        return items
    }
    
    // TODO: 인덱스에 맞는 트랙 로드하기
    func track(at index: Int) -> Track? {
        let playerItem = tracks[index]
        let track = playerItem.convertToTrack()
        return track
    }

    // TODO: 앨범 로딩메소드 구현
    func loadAlbums(tracks: [AVPlayerItem]) -> [Album] {
        let trackList: [Track] = tracks.compactMap { $0.convertToTrack() }
        let albumDics = Dictionary(grouping: trackList, by: { (track) in track.albumName })
        var albums: [Album] = []
        for (key, value) in albumDics {
            let title = key
            let tracks = value
            let album = Album(title: title, tracks: tracks)
            albums.append(album)
        }
        return albums
    }

    // TODO: 오늘의 트랙 랜덤으로 선책
    func loadOtherTodaysTrack() {
        self.todaysTrack = self.tracks.randomElement()
    }
}
```

## 참고 - Extention + AVPlayerItem // convertToTrack
```Swift
// Extention의 사용법과 asset의 활용법을 참고해보자
// 아래 같은 코드는 내가 직적 구현한다기보다는
// 구글링과 깃헙 등에서 찾아서 활용하는 경우가 많다.
import AVFoundation
import UIKit

extension AVPlayerItem {
    func convertToTrack() -> Track? {
        let metadatList = asset.metadata
        
        var trackTitle: String?
        var trackArtist: String?
        var trackAlbumName: String?
        var trackArtwork: UIImage?
        
        for metadata in metadatList {
            if let title = metadata.title {
                trackTitle = title
            }
            
            if let artist = metadata.artist {
               trackArtist = artist
            }
            
            if let albumName = metadata.albumName {
                trackAlbumName = albumName
            }
            
            if let artwork = metadata.artwork {
                trackArtwork = artwork
            }
        }
        
        guard let title = trackTitle,
            let artist = trackArtist,
            let albumName = trackAlbumName,
            let artwork = trackArtwork else {
                return nil
        }
        return Track(title: title, artist: artist, albumName: albumName, artwork: artwork)
    }
}
 
extension AVMetadataItem {
    var title: String? {
        guard let key = commonKey?.rawValue, key == "title" else {
            return nil
        }
        return stringValue
    }
    
    var artist: String? {
        guard let key = commonKey?.rawValue, key == "artist" else {
            return nil
        }
        return stringValue
    }
    
    var albumName: String? {
        guard let key = commonKey?.rawValue, key == "albumName" else {
            return nil
        }
        return stringValue
    }
    
    var artwork: UIImage? {
        guard let key = commonKey?.rawValue, key == "artwork", let data = dataValue, let image = UIImage(data: data) else {
            return nil
        }
        return image
    }
}

extension AVPlayer {
    var isPlaying: Bool {
        guard self.currentItem != nil else { return false }
        return self.rate != 0
    }
}

```

## 실습 - 홈화면 컬렉션뷰
```Swift
// 헤더뷰 어떻게 표시할까?
    func collectionView(_ collectionView: UICollectionView, viewForSupplementaryElementOfKind kind: String, at indexPath: IndexPath) -> UICollectionReusableView {
        switch kind {
        case UICollectionView.elementKindSectionHeader:
            guard let item = trackManager.todaysTrack else {
                return UICollectionReusableView()
            }
            
            guard let header = collectionView.dequeueReusableSupplementaryView(ofKind: kind, withReuseIdentifier: "TrackCollectionHeaderView", for: indexPath) as? TrackCollectionHeaderView else {
                return UICollectionReusableView()
            }
            
            header.update(with: item)
            header.tapHandler = { item -> Void in
                // 플레이어 띄우기
                print("---> item title: \(item.convertToTrack()?.title)")
            }
            
            // TODO: 헤더 구성하기
            return header
        default:
            return UICollectionReusableView()
        }
    }


 class TrackCollectionHeaderView: UICollectionReusableView {
    @IBOutlet weak var thumbnailImageView: UIImageView!
    @IBOutlet weak var descriptionLabel: UILabel!
    
    var item: AVPlayerItem?
    var tapHandler: ((AVPlayerItem) -> Void)?
    
    override func awakeFromNib() {
        super.awakeFromNib()
        thumbnailImageView.layer.cornerRadius = 4
    }
    
    func update(with item: AVPlayerItem) {
        // TODO: 헤더뷰 업데이트 하기
        self.item = item
        guard let track = item.convertToTrack() else { return }
        
        self.thumbnailImageView.image = track.artwork
        self.descriptionLabel.text = "Today`s pick is \(track.artist) album - \(track.albumName), Let`s Listen."
    }
    
    @IBAction func cardTapped(_ sender: UIButton) {
        // TODO: 탭했을때 처리
        guard let todaysItem = item else { return }
        tapHandler?(todaysItem)
    }
}
```

## 실습 - 플레이어뷰 만들기
```Swift
class SimplePlayer {
    // TODO: 싱글톤 만들기, 왜 만드는가?
    static let shared = SimplePlayer()
    
    
    private let player = AVPlayer()

    var currentTime: Double {
        // TODO: currentTime 구하기
        return player.currentItem?.currentTime().seconds ?? 0
    }
    
    var totalDurationTime: Double {
        // TODO: totalDurationTime 구하기
        return player.currentItem?.duration.seconds ?? 0
    }
    
    var isPlaying: Bool {
        // TODO: isPlaying 구하기
        return player.isPlaying
    }
    
    var currentItem: AVPlayerItem? {
        // TODO: currentItem 구하기
        return player.currentItem
    }
    
    init() { }
    
    func pause() {
        // TODO: pause구현
        player.pause()
    }
    
    func play() {
        // TODO: play구현
        player.play()
    }
    
    func seek(to time:CMTime) {
        // TODO: seek구현
        player.seek(to: time)
    }
    
    func replaceCurrentItem(with item: AVPlayerItem?) {
        // TODO: replace current item 구현
        player.replaceCurrentItem(with: item)
    }
    
    func addPeriodicTimeObserver(forInterval: CMTime, queue: DispatchQueue?, using: @escaping (CMTime) -> Void) {
        player.addPeriodicTimeObserver(forInterval: forInterval, queue: queue, using: using)
    }
}


// 헤더뷰 어떻게 표시할까?
    func collectionView(_ collectionView: UICollectionView, viewForSupplementaryElementOfKind kind: String, at indexPath: IndexPath) -> UICollectionReusableView {
        switch kind {
        case UICollectionView.elementKindSectionHeader:
            guard let item = trackManager.todaysTrack else {
                return UICollectionReusableView()
            }
            
            guard let header = collectionView.dequeueReusableSupplementaryView(ofKind: kind, withReuseIdentifier: "TrackCollectionHeaderView", for: indexPath) as? TrackCollectionHeaderView else {
                return UICollectionReusableView()
            }
            
            header.update(with: item)
            header.tapHandler = { item -> Void in
                // 플레이어 띄우기
                let playerStoryboard = UIStoryboard.init(name: "Player", bundle: nil)
                guard let playerVC = playerStoryboard.instantiateViewController(identifier: "PlayerViewController") as? PlayerViewController else { return }
                // 곡 전달하기
                playerVC.simplePlayer.replaceCurrentItem(with: item)
                self.present(playerVC, animated: true, completion: nil)
                print("---> item title: \(item.convertToTrack()?.title)")
            }
            
            // TODO: 헤더 구성하기
            return header
        default:
            return UICollectionReusableView()
        }
    }
```
## 플레이어뷰에서 플레이 버튼 클릭 시 이미지가 변경되는 것
```Swift
func updatePlayButton() {
        // TODO: 플레이버튼 업데이트 UI작업 > 재생/멈춤
        if simplePlayer.isPlaying {
            let configuration = UIImage.SymbolConfiguration(pointSize: 40)
            let image = UIImage(systemName: "pause.fill", withConfiguration: configuration)
            playControlButton.setImage(image, for: .normal)
        } else {
            let configuration = UIImage.SymbolConfiguration(pointSize: 40)
            let image = UIImage(systemName: "play.fill", withConfiguration: configuration)
            playControlButton.setImage(image, for: .normal)
        }
    }
```
## 플레이어뷰에 씨킹하기
```Swift
   @IBAction func beginDrag(_ sender: UISlider) {
        isSeeking = true
    }
    
    @IBAction func endDrag(_ sender: UISlider) {
        isSeeking = false
    }
    
    @IBAction func seek(_ sender: UISlider) {
        // TODO: 시킹 구현
        guard let currentItem = simplePlayer.currentItem else { return }
        let position = Double(sender.value) // 0....1 -> 0.5
        let seconds = position * currentItem.duration.seconds
        // 3분짜리 음악이면 초로 180초이다.
        // 씨킹이 중간에 됐다고하면 0.5이다.
        // 이 때 위 코드처럼 진행된다면 90.1231235235 이런 값이 나올 것이다.
        // 이걸 currentItem.duration.seconds 여기서 seconds를 붙여줘서 90.12 이렇게 표시되도록 하는 것이다.
        let time = CMTime(seconds: seconds, preferredTimescale: 100)
        simplePlayer.seek(to: time)
    }

    func updateTime(time: CMTime) {
        // print(time.seconds)
        // currentTime label, totalduration label, slider
        
        // TODO: 시간정보 업데이트, 심플플레이어 이용해서 수정
        currentTimeLabel.text = secondsToString(sec: simplePlayer.currentTime)   // 3.1234 >> 00:03
        totalDurationLabel.text = secondsToString(sec: simplePlayer.totalDurationTime)  // 39.2045  >> 00:39
        
        if isSeeking == false {
            // 노래 들으면서 시킹하면, 자꾸 슬라이더가 업데이트 됨, 따라서 시킹아닐때마 슬라이더 업데이트하자
            // TODO: 슬라이더 정보 업데이트
            // 씨킹하지 않을 때 슬라이드를 업데이트한다.
            timeSlider.value = Float(simplePlayer.currentTime/simplePlayer.totalDurationTime)
        }
    }
```
## 다크모드 - 간단한 버전
#### 색상을 사용할 때 시스템 색상을 사용하면
#### 기본적으로 애플에서 다크모드에 대응을 해놨기 때문에
#### 자동으로 색상이 변하는 것을 확인할 수 있다.
```Swift
// 플레이어가 표시될 때인 viewWillAppear를 보면
// updateTintColor()이라는 코드가 있다.
// 이 코드로 인해 플레이어를 실행했을 때
// 다크모드면 토글버튼과 슬라이더가 흰색
// 화이트모드면 토글버큰과 슬라이더가 검정색으로 표시된다.
   override func viewWillAppear(_ animated: Bool) {
        super.viewWillAppear(animated)
        updateTintColor()
        updateTrackInfo()
    }

// 위의 코드는 아래 함수처럼 사용되었고.
    func updateTintColor() {
        playControlButton.tintColor = DefaultStyle.Colors.tint
        timeSlider.tintColor = DefaultStyle.Colors.tint
    }

// 위의 코드는 아래처럼 작성되었다.
// 이 코드는 아직 어려우니 참고만 하고 무슨 뜻인지 이해부터 하자.
public enum DefaultStyle {
    public enum Colors {
        public static let tint: UIColor = {
            if #available(iOS 13.0, *) {
                return UIColor { traitCollction in
                // tint컬러에 대해서 우리는 정의를 했다 -> UIColor 컬러를 tint 컬러로 반환시킬건데 컬러를 반환할 때 조건이 아래와 같이 있다.
                    if traitCollction.userInterfaceStyle == .dark {
                    // traitCollction를 통해 우리는 현재 다크모드인지 아닌지를 확인할 수 있다.
                    // 현재 traitCollction의 userInterfaceStyle가 다크모드면 흰색을 반환하고 아니면 검정을 반환해라.
                        return .white
                    } else {
                        return .black
                    }
                }
            } else {
                return .black
            }
        }()
    }
}
```
