# 사용자 검색 화면 만들기

## 화면 구성하기
-----------

이제 사용자 검색 화면 만들어보겠습니다.

사용자 검색 화면 구성을 위해 [ionic의 avatar-list](http://ionicframework.com/docs/v2/components/#avatar-list) Component를 활용할 예정입니다.

먼저 `pages/follows` 폴더 아래의 `searchUser.html` 파일을 확인하겠습니다.

상단 헤더영역의 버튼과 검색창이 포함된 화면이 아래와 같이 생성되어 있습니다.

```html
<ion-header>
  <ion-navbar>
    <ion-title>
      Search User
    </ion-title>
    <ion-buttons end>
      <button ion-button color="royal" (click)="confirm();">Add</button>
    </ion-buttons>
  </ion-navbar>
</ion-header>
<ion-content >
  <ion-searchbar ></ion-searchbar>
  <ion-list >
  </ion-list>
</ion-content>
```

`searchUser.html` 과 매핑되는 `searchUser.ts`이 아래와 같이 생성되어 있습니다.

```javascript
import { Component } from '@angular/core';

import { NavController, App, NavParams} from 'ionic-angular';

import {SharedService} from '../../app/sharedService';

@Component({
  selector: 'page-searchUser',
  templateUrl: 'searchUser.html'
})
export class SearchUserPage {

  searchQuery: string = '';
  users:any[] = [];

  timeout:any;  //timeout을 컨트롤 하기 위한 변수
  checkedList: any = {}; // 체크 여부가 매핑될 변수
  callback: any;

  constructor(public navCtrl: NavController, public ss: SharedService, private app:App, private navParam) {
    this.callback = navParams.get('callback');
  }
}
```

## STALK-IM 의 사용자 조회하기
----------
검색창에 검색어를 입력할 경우, 사용자를 검색하는 이벤트를 추가하기 위해 [검색창](http://ionicframework.com/docs/v2/api/components/searchbar/Searchbar/)에 `ion-input` event를 추가하겠습니다.

```html
<ion-searchbar (ionInput)="getItems($event)"></ion-searchbar>
```

이제 입력받은 이벤트 객체의 value를 활용하여 STALK-IM에 가입되어 있는 사용자를 조회하도록 getItems 함수를 구현하겠습니다.

```javascript
  getItems(ev: any) {
    var self = this;

    // set val to the value of the searchbar
    let val = ev.target.value;

    if( this.timeout )clearTimeout(this.timeout);
    this.timeout = setTimeout(function(){

      // 사용자 조회하기
      self.ss.stalk.searchUsers( val, function( err, users ){
        self.users = users;
        console.log( users );
      });

    }, 200 );
  }
```

여기서는 키 입력시 딜레이를 주도록 timeout을 이용하여 구현하였고, STALK-IM의 `searchUsers` 라는 함수를 활용하였습니다.


## array를 화면에 binding 하기
-----------

List 유형의 Component는 item의 갯수만큼 loop를 돌며 그리도록 되어 있습니다.

users 라는 array 객체를 만들어 Dom 객체에 Binging 하도록 화면을 수정해보겠습니다.

```html
  <ion-list >
    <ion-item *ngFor="let user of users;">
      <ion-checkbox color="primary" checked="false" [(ngModel)]="checkedList[user.id]"></ion-checkbox>
      <ion-avatar item-left>
        <img src="{{user.avatar}}" />
      </ion-avatar>
      <ion-label>{{user.nickName}}</ion-label>
    </ion-item>
  </ion-list>
```

여기서는 list를 구현하기 위해 angular2의 [ngFor](https://angular.io/docs/ts/latest/api/common/index/NgFor-directive.html)를 사용했습니다. [ngFor](https://angular.io/docs/ts/latest/api/common/index/NgFor-directive.html)는 javascript의 for 문과 비슷한 형태로 동작하며, array 형태의 변수에서 object를 꺼내와서 바인딩하도록 되어 있습니다.

[ion-checkbox](http://ionicframework.com/docs/v2/api/components/checkbox/Checkbox/)에는 `checkedList`를 매핑하여, 선택한 유저의 id 정보를 가지고 있도록 구성하였습니다.

## 콜백함수 호출하기
-----------
사용자 검색 화면은 다른 화면에서 호출하는 팝업 형태로 제공될 예정입니다.
우측 상단의 버튼을 클릭하면, `checkedList`에서 체크된 id를 array로 변환한 후 호출한 화면으로 넘겨주도록 로직을 구성하겠습니다.


```javascript
  confirm = () => {
    var self = this;

    var checkUserIds = [];
    for( var userId in this.checkedList ){
      if( this.checkedList[userId] ){
        checkUserIds.push( userId );
      }
    }

    if( self.callback ){
      self.callback(checkUserIds);
    }
    self.navCtrl.pop();
  }

```

이제 `SearchUserPage`를 호출할 `FollowsPage`를 작성하겠습니다.