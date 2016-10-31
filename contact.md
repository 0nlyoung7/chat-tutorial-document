# 사용자 목록 화면 만들기

이번에는 기존에 만들어진 Template을 활용하여 `chat-tutorial` 서비스를 사용 중인 사용자 리스트를 구성해보겠습니다.

## 1) 화면 구성하기

사용자 목록을 위해 Ionic의 [avatar-list](http://ionicframework.com/docs/v2/components/#avatar-list) Component를 활용할 예정입니다.

먼저 `contact.html` 을 아래와 같이 수정하겠습니다.

```html
	<ion-list>
		<ion-item>
			<ion-avatar item-left>
				<img src="http://ionicframework.com/dist/preview-app/www/assets/img/avatar-ts-woody.png">
			</ion-avatar>
			<h2>Woody</h2>
			<p>This town ain't big enough for the two of us!</p>
		</ion-item>
		<ion-item>
			<ion-avatar item-left>
				<img src="http://ionicframework.com/dist/preview-app/www/assets/img/avatar-ts-buzz.png">
			</ion-avatar>
			<h2>Buzz Lightyear</h2>
			<p>My eyeballs could have been sucked from their sockets!</p>
		</ion-item>
	</ion-list>
```

## 2) Array 객체 binding 하기

List Component는 대부분 item의 갯수만큼 loop를 돌며 그리도록 되어 있습니다.

users 라는 array 객체를 만들어 Dom 객체에 Binging 하도록 수정해보겠습니다.

먼저 `contact.ts` 파일을 수정해서 array 객체를 선언하겠습니다.

```js
	users:any[] = [];
```

생성자에 users 객체를 바인딩하겠습니다.

```js
	users = [
		{'U':'Woody','I':'http://ionicframework.com/dist/preview-app/www/assets/img/avatar-ts-woody.png'},
		{'U':'Buzz Lightyear','I':'http://ionicframework.com/dist/preview-app/www/assets/img/avatar-ts-buzz.png'}
	];
```

이제 users 객체를 사용하여 item을 재구성해보겠습니다.

```html
<ion-item *ngFor="let user of users;" >
	<ion-avatar item-left>
	 	<img src="{{user.I}}">
	</ion-avatar>
	<h2>{{user.U}}</h2>
	<p>{{user.MG}}</p>
</ion-item>
```

중복되는 부분을 아래와 같이 수정하겠습니다.

여기서는 list를 구현하기 위해 angular2의 [ngFor](https://angular.io/docs/ts/latest/api/common/index/NgFor-directive.html)를 사용했습니다. [ngFor](https://angular.io/docs/ts/latest/api/common/index/NgFor-directive.html)는 javascript의 for 문과 비슷한 형태로 동작하며, array 형태의 변수에서 object를 꺼내와서 바인딩을 하도록 되어 있습니다.

## 3) xpush 의 user list 조회하기

