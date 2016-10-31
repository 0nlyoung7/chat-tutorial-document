#. 준비하기

이번 튜토리얼에서는 ionic v2와 XPush의 Javascript 라이브러리를 사용해서 간단한 채팅 서비스를 만들어보려고 합니다.

## docker 설치하기

[Docker for Mac](https://download.docker.com/mac/stable/Docker.dmg)

[Docker for Windows](https://download.docker.com/win/stable/InstallDocker.msi)
	
## nodejs 설치하기

ionic은 nodejs 기반으로 개발환경을 제공하고 있습니다. Nodejs의 버젼 중에서 최신의 LTS 버젼을 설치하겠습니다.

[Nodejs Downloads](https://nodejs.org/ko/download/)

## Ionic 설치하기

마지막으로 화면을 개발하기 위해 사용하는 ionic을 설치하겠습니다.
사용하는 환경에 따라 2분~5분 정도가 소요될 수 있습니다.
cordova 사용을 위해서 필요한 Android SDK나 XCODE가 필요할 수 있습니다.

``` bash
	sudo npm install -g cordova ionic
```

이렇게 하면 ionic 개발을 위한 모든 준비가 완료된 상태입니다.