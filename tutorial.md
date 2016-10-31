# Ionic 프로젝트 시작하기

Ionic 은 nodejs 기반의 cli 를 제공하고 있습니다.
Prepare 세션에서 설치한 ionic의 [tab template](https://github.com/driftyco/ionic2-starter-tabs)을 사용하여 chat-tutorial 프로젝트를 생성하겠습니다. `ionic start chat-tutorial tabs --v2`
	
	$ionic start chat-tutorial tabs --v2

생성된 chat-tutorial 프로젝트를 아래와 같은 명령어를 사용해서 web application의 형태로 실행할 수 있습니다. `ionic serve`

	$ionic serve

## 프로젝트의 구조

생성이 완료후 해당 프로젝트 폴더를 사용하는 에디터를 이용하여 열어보면 다음과 같은 구조로 되어 있는 것을 확인 하실 수 있습니다.

### ./src/index.html

`src/index.html` 은 이 프로젝트의 시작 페이지입니다. ionic을 사용하기 위한 angular, bootstrap 등의 javascript와 css 파일이 import 되어 있습니다.
	

### ./src/

실제로 작업을 수행할 디렉토리입니다.

`src/app/app.module.ts` angular2 에서 사용하는 모듈 설정 파일입니다. ionic 에서 제공하는 module과 plugin 대한 설정이 포함되어 있습니다.