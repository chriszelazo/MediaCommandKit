# MediaCommandKit (iOS Media Player Command Observing)
Observe and Hijack iOS Media Remote Commands (Volume, Play / Pause, etc.). This could be helpful for music players, camera, or event mapping control events from bluetooth media controllers such as the [Ortho Remote](https://teenage.engineering/products/orthoremote) to be used as generic controllers.


## Observable Events
* `volume`
* `togglePlayPauseCommand`
* `playCommand`
* `pauseCommand`
* `stopCommand`
* `enableLanguageOptionCommand`
* `disableLanguageOptionCommand`
* `changePlaybackRateCommand`
* `changeRepeatModeCommand`
* `changeShuffleModeCommand`
* `nextTrackCommand`
* `previousTrackCommand`
* `skipForwardCommand`
* `skipBackwardCommand`
* `seekForwardCommand`
* `seekBackwardCommand`
* `changePlaybackPositionCommand`
* `ratingCommand`
* `likeCommand`
* `dislikeCommand`
* `bookmarkCommand`


## Installation
1. Copy `MediaCommandCenter.swift` to your project and ensure it is included in `Build Phases` > `Compile Sources`.
2. Copy `silence.m4a` into `Assets.xcassets`.


## How to use

### Begin observing
\>= iOS 13:

In `SceneDelegate`, call `MediaCommandCenter`'s prepare and resign methods.

```
func sceneDidBecomeActive(_ scene: UIScene) {
    MediaCommandCenter.prepareToBecomeActive()
}

func sceneWillResignActive(_ scene: UIScene) {
    MediaCommandCenter.prepareToResignActive()
}
```

< iOS 13:

In `AppDelegate`, call `MediaCommandCenter`'s prepare and resign methods.
```
func applicationDidBecomeActive(_ application: UIApplication) {
    MediaCommandCenter.prepareToBecomeActive()
}

func applicationWillResignActive(_ application: UIApplication) {
    MediaCommandCenter.prepareToResignActive()
}
```

### Setup the observer
Add the observing object as an observer and set commands to observe:
```
MediaCommandCenter.addObserver(self)
MediaCommandCenter.observedCommands = [.volume, .togglePlayPause]
```

Conform to the `MediaCommandObserver` protocol:
```
extension MyObject: MediaCommandObserver {
    
    func mediaCommandCenterHandleTogglePlayPause() {
        // Handle play/pause toggle
    }
    
    func mediaCommandCenterHandleVolumeChanged(_ volume: Double) {
        // Handle volume change 
    }
    
    ...
    
}
```

Remove the observing object on deinit:
```
deinit {
    MediaCommandCenter.removeObserver(self)
}
```

### Play your own music
Set the `audioPlayer` instance in `MediaCommandCenter`, otherwise the default audio track of 00:00 length silence will play.


## How it works
While the application is active, it will occupy the system media player with a silent audio track in order to receive system notifications for player control events. `MediaCommandCenter` listens play/pause/etc. commands from `MPRemoteCommandCenter` and volume KVO notifications from `AVAudioSession`.

Note: System media player events will not trigger if the audio player is not initiated.


## Thanks
* Much inspiration from [luiswdy/TallyCounter](https://github.com/luiswdy/TallyCounter)
