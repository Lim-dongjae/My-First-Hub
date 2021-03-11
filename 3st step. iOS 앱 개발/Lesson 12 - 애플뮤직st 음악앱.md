#아이폰 개발 시작하기

#주제 - iOS 앱 개발 #패스트 캠퍼스 

---
 
12강 스위프트 __애플뮤직st 음악앱__

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

## 실습 - 