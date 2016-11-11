# 준비하기

먼저 개발에 필요한 프로그램인 [nodejs](https://nodejs.org/)와 [ionic](http://ionicframework.com/docs/v2/) 설치를 진행하겠습니다.
	
## nodejs 설치하기
-----------

ionic은 nodejs 기반으로 개발환경을 제공하고 있습니다.
ionic
Nodejs의 버젼 중에서 최신의 LTS 버젼을 설치하겠습니다.

[Nodejs Downloads](https://nodejs.org/en/download/)

> ionic v2 를 설치하기 위해서는 nodejs 6.0 이상이 필요합니다.

## Ionic 설치하기
-----------

화면을 개발하기 위해 사용하는 ionic을 설치하겠습니다.
사용하는 환경에 따라 2분~5분 정도가 소요될 수 있습니다.
향후 cordova Plugin 사용을 위해서 필요한 Android SDK나 XCODE가 필요할 수 있습니다.

아래 명령어를 이용하여 ionic을 설치하겠습니다.

```bash
$sudo npm install -g cordova ionic
```
> ionic을 command line 명령어로 사용하기 위해서는 root 권한이 필요합니다.

## 프로젝트 다운로드
-----------

위 단계에서 설치한 ionic을 이용하면 [Tab Template](https://github.com/driftyco/ionic2-starter-tabs)을 활용한 프로젝트를 생성할 수 있습니다.

이번 튜토리얼 세션에서는 [ionic2의 Tab Template](https://github.com/driftyco/ionic2-starter-tabs)을 활용하여 미리 생성한 프로젝트를 이용하여 실습을 진행하겠습니다.

git을 이용하여 프로젝트를 다운로드 받겠습니다.

```bash	
$ git clone https://github.com/0nlyoung7/chat-tutorial
```

생성된 chat-tutorial 프로젝트 폴더로 이동하여 `npm install` 명령어를 통해 모듈 설치를 진행하겠습니다.

```bash
$ cd chat-tutorial
$ npm install
```
