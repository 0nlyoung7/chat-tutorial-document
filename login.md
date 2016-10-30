# 로그인 만들기

## 화면 구성하기

먼저 account 폴더를 생성한 후에, 그 폴더 아래에 signin.html 파일을 생성하겠습니다.

먼저 상단 헤더영역과 기본 화면을 생성하겠습니다.

	`<ion-header>
		<ion-navbar>
			<ion-title>Sign In</ion-title>
		</ion-navbar>
		<ion-content padding>
		</ion-content padding>
	</ion-header>`

## 입력창과 버튼 구성하기

signin.html 파일에 입력창과 로그인 버튼을 추가해보겠습니다. 
ionic v2에서 제공하는 [Input](http://ionicframework.com/docs/v2/components/#stacked-labels)컴포넌트를 이용하여 로그인을 위한 입력창과 버튼을 생성하겠습니다.
	
	`<ion-content padding>
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
	</ion-content>`

아래와 같은 화면이 생성되었습니다.

ㅋ이곳에서는 angular2의 ngModel 과 click 등을 사용했습니다.

ngModel은 Controller 와의 매핑을 위해 사용하고, click은 HTML의 onclick 이벤트를 위해 사용했습니다.
보다 자세한 내용은 angular2 의 공식 문서를 통해 확인하실 수 있습니다.