# 로그인 만들기

## 화면 구성하기

먼저 account 폴더를 생성한 후에, 그 폴더 아래에 signin.html 파일을 생성하겠습니다.

먼저 상단 헤더영역과 기본 화면을 생성하겠습니다.

```html
<ion-header>
	<ion-navbar>
		<ion-title>Sign In</ion-title>
	</ion-navbar>
	<ion-content padding>
	</ion-content padding>
</ion-header>
```	

singin.html 과 매핑될 signin.ts을 생성하겠습니다.

```js
import { Component } from '@angular/core';

@Component({
	selector: 'page-signin',
	templateUrl: 'signin.html'
})
export class SignInPage {
	constructor(public navCtrl: NavController) {
	}
}
```


## 입력창과 버튼 구성하기

singin.ts 파일에 id, password 를 입력받기 위한 변수와 로그인 버튼 이벤트를 위한 메소드를 생성하겠습니다.

```js
userId: string;
password: string;

public signIn(){
	var self = this;
	console.log( self.userId );
	console.log( self.paassword );
}
 ```

이제 signin.html 파일에 입력창과 로그인 버튼을 추가하겠습니다.
ionic v2에서 제공하는 [Input](http://ionicframework.com/docs/v2/components/#stacked-labels)컴포넌트를 이용하여 로그인을 위한 입력창과 버튼을 생성하겠습니다.
	
```html
<ion-content padding>
	<ion-list>
		<ion-item>
			<ion-label stacked>UserId</ion-label>
			<ion-input type="text" [(ngModel)]="userId" value=""></ion-input>
		</ion-item>
		<ion-item>
			<ion-label stacked>Password</ion-label>
			<ion-input type="password" [(ngModel)]="password"  ></ion-input>
		</ion-item>
	</ion-list>
	<div padding>
		<button ion-button color="primary" block (click)="signIn()">Sign In</button>
	</div>
</ion-content>
```

아래와 같은 화면이 생성되었습니다.

이곳에서는 angular2에서 컴포넌트와 DOM 객체의 바인딩을 위한 방법 중 ngModel 과 click 등을 사용했습니다.

![Data binding](http://i0.wp.com/angular.io/resources/images/devguide/architecture/databinding.png?resize=270%2C251&ssl=1)

ngModel은 양방향 바인딩 위해 사용하고, click은 버튼의 onclick 이벤트를 위해 사용했습니다. angular2에 대한 보다 자세한 내용은 angular2 의 공식 문서를 통해 확인하실 수 있습니다.

## Anugular2 에서 XPush 객체 사용하기

먼저 html에 xpush.js 스크립트를 추가하겠습니다.

XPush 객체를 모든 컴포넌트에서 사용할 수 있도록 하기 위한 [Service Prodiver](https://angular.io/docs/ts/latest/guide/ngmodule.html#!#providers)를 생성하겠습니다.

sharedService.js 를 아래와 같이 작성하겠습니다.


```js
import { Injectable } from '@angular/core';
declare var XPush: any;

@Injectable()
export class SharedService {

	public host = 'http://session.stalk.io:8000';
	public app = 'chat-tutorial';

	//XPush 객체를 생성
	xpush:any;
	constructor() {
		// XPush 초기화 하기
		this.xpush = new XPush(this.host, this.app);
	}
}
```

sharedService 를 providers에 추가합니다.
이렇게 추가된 sharedService는 아래와 같은 방식으로 모든 컴포넌트에서 사용이 가능합니다.

```js
	constructor(public navCtrl: NavController, public ss: SharedService) {
		ss.xpush;		
	}
```

## xpush에 로그인하기

이제 xpush 객체를 이용해서 signIn 함수를 아래와 같이 수정하겠습니다.

```js
	public signIn(){
		var self = this;
		this.ss.xpush.login(this.userId, this.password, function(err, result){
			console.log( result );
		});
	};
```

입력창에 userId 와 password를 입력하고 Login 버튼을 클릭하면, 정상적으로 Login이 되는 것을 확인하실 수 있습니다.
로그인이 완료되면, 기존의 메인 Page 였던 TabPage로 이동하도록 signIn() 수정하겠습니다.

```js
	self.navCtrl.push(TabsPage, {});
```