# SignUp 만들기

## 화면 구성하기
-----------

먼저 pages 내에 `account` 폴더를 생성한 후에, 그 폴더 아래에 signup.html 파일을 생성하겠습니다.

이제 헤더영역과 기본 화면을 생성하겠습니다.

```html
<ion-header>
  <ion-navbar>
    <ion-title>Sign Up</ion-title>
  </ion-navbar>
  <ion-content padding>
  </ion-content padding>
</ion-header>
``` 

signup.html 과 매핑될 signup.ts을 생성하겠습니다.

```javascript
import { Component } from '@angular/core';

@Component({
  selector: 'page-signup',
  templateUrl: 'signup.html'
})
export class SignUpPage {
  constructor(public navCtrl: NavController) {
  }
}
```

## 입력창과 버튼 생성하기
-----------

signup.ts 파일에 username, password 를 입력받기 위한 변수와 `Sign Up` 버튼 이벤트를 위한 메소드를 생성하겠습니다.

```javascript
  username: string;
  password: string;

  public signUp(){
    var self = this;
    console.log( self.username );
    console.log( self.paassword );
  }
 ```

이제 signup.html 파일에 입력창과 회원가입 버튼을 추가하겠습니다.
ionic v2에서 제공하는 [ionic의 ion-input](http://ionicframework.com/docs/v2/components/#stacked-labels) 컴포넌트를 이용하여 회원가입을 위한 입력창과 버튼을 생성하겠습니다.
  
```html
<ion-content padding>
  <ion-list>
    <ion-item>
      <ion-label stacked>Username</ion-label>
      <ion-input type="text" [(ngModel)]="username" value=""></ion-input>
    </ion-item>
    <ion-item>
      <ion-label stacked>Password</ion-label>
      <ion-input type="password" [(ngModel)]="password"  ></ion-input>
    </ion-item>
  </ion-list>
  <div padding>
    <button ion-button color="primary" block (click)="signUp()">Sign Up</button>
  </div>
</ion-content>
```

이곳에서는 angular2에서 컴포넌트와 DOM 객체의 바인딩을 위한 방법 중 ngModel 과 click 등을 사용했습니다.

![Data binding](http://i0.wp.com/angular.io/resources/images/devguide/architecture/databinding.png?resize=270%2C251&ssl=1)

ngModel은 양방향 바인딩 위해 사용하고, click은 버튼의 onclick 이벤트를 위해 사용했습니다.
angular2에 대한 보다 자세한 내용은 angular2 의 공식 문서를 통해 확인하실 수 있습니다.

## Anugular2 에서 S5Platform SDK 사용하기
-----------

먼저 index.html에 stalk.js 스크립트를 추가하겠습니다.

```html
<script ></script>
```

Stalk 객체를 모든 컴포넌트에서 사용할 수 있도록 하기 위한 [Service Prodiver](https://angular.io/docs/ts/latest/guide/ngmodule.html#!#providers)를 생성하겠습니다.

app폴더 아래에 sharedService.js 를 아래와 같이 작성하겠습니다.

```javascript
import { Injectable } from '@angular/core';

declare var Stalk: any;

@Injectable()
export class SharedService {
  public host = 'https://im.stalk.io';
  public app = 'STALK';
  stalk:any;

  constructor() {
    this.stalk = new Stalk(this.host, this.app);
  }
}
```

sharedService 를 `app.module.ts` 파일 안의 providers에 추가합니다.
이렇게 추가된 sharedService는 아래와 같은 방식으로 모든 컴포넌트에서 사용이 가능합니다.

```javascript
import {SharedService} from '../../app/sharedService';

...

  constructor(public navCtrl: NavController, public ss: SharedService) {
    ss.stalk;
  }
```

## S5Platform에 회원가입 하기
-----------

이제 stalk 객체를 이용해서 signUp 함수를 아래와 같이 수정하겠습니다.

```javascript
  public signUp(){
    var self = this;
    this.ss.stalk.signUp(this.username, this.password, function(err, user){
      console.log( user );
    });
  };
```

입력창에 username 와 password를 입력하고 `Sign Up` 버튼을 클릭하면, 회원가입 결과를 확인하실 수 있습니다.

signUp 실패 시에 alert 창을 보여주기 위한 [ionic의 AlertController](http://ionicframework.com/docs/v2/api/components/alert/AlertController/)을 추가하여 아래와 같은 코드가 완성되었습니다.

#### `signup.ts`

```javascript
import { Component } from '@angular/core';

import { NavController, AlertController } from 'ionic-angular';

import {SharedService} from '../../app/sharedService';

@Component({
  selector: 'page-signup',
  templateUrl: 'signup.html'
})
export class SignUpPage {

  username: string;
  password: string;

  // Alert 을 위한 alertCtrl 을 추가함
  constructor(public navCtrl: NavController, public ss: SharedService, public alertCtrl: AlertController) {
  }

  public signUp(){
    var self = this;
    this.ss.stalk.signUp(this.username, this.password, function(err, user){

      var title = 'Success';
      var subTitle = 'SignUp Success.';

      if( err ){
        title = 'Failed';
        subTitle = err.message;
      }

      let alert = self.alertCtrl.create({
        title: title,
        subTitle: subTitle,
        buttons: ['OK']
      });
      alert.present();

      if( !err ){
        self.navCtrl.pop();
      }
    });
  }
}
```

#### `signup.html`
```html
<ion-header>
  <ion-navbar>
    <ion-title>Sign Up</ion-title>
  </ion-navbar>
</ion-header>

<ion-content padding>
  <ion-list>
    <ion-item>
      <ion-label stacked>Username</ion-label>
      <ion-input type="text" [(ngModel)]="username" value=""></ion-input>
    </ion-item>
    <ion-item>
      <ion-label stacked>Password</ion-label>
      <ion-input type="password" [(ngModel)]="password"  ></ion-input>
    </ion-item>
  </ion-list>
  <div padding>
    <button ion-button color="primary" block (click)="signUp()">Sign Up</button>
  </div>
</ion-content>
```