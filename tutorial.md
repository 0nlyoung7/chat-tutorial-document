## Ionic 시작하기

Ionic 은 nodejs 기반의 cli 를 제공하고 있습니다.
Prepare 세션에서 설치한 ionic의 [tab template](https://github.com/driftyco/ionic2-starter-tabs)을 사용하여 chat-tutorial 프로젝트를 생성하겠습니다. `ionic start chat-tutorial tabs --v2`
	
	$ionic start chat-tutorial tabs --v2

생성된 chat-tutorial 프로젝트를 아래와 같은 명령어를 사용해서 실행합니다. `ionic serve`

	$ionic serve

생성된 프로젝트의 구조에 대한 설명

### ./src/index.html

`src/index.html` 은 이 프로젝트의 시작 페이지입니다. ionic을 사용하기 위한 angular, bootstrap 등의 javascript와 css 파일이 import 되어 있습니다.
	

### ./src/

실제로 작업을 수행할 디렉토리입니다.

`src/app/app.module.ts` angular2 에서 사용하는 모듈 설정 파일입니다. ionic 에서 제공하는 module과 directive에 대한 설정이 포함되어 있습니다.