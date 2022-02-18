# macOS

## 网易云音乐 WI-FI模式下无法获取网络
```text
sudo launchctl stop com.apple.audio.coreaudiod && sudo launchctl start com.apple.audio.coreaudiod
```

## Setting PATH for android add tool
export ANDROID_HOME=/Applications/Unity/Hub/Editor/2020.3.3f1c1/PlaybackEngines/AndroidPlayer/SDK
export PATH=${PATH}:${ANDROID_HOME}/tools
export PATH=${PATH}:${ANDROID_HOME}/platform-tools

## homebrew in macOS

- Homebrew安装教程：https://brew.idayer.com/