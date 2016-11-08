# 친구 목록 화면 만들기

## 화면 구성하기
-----------

친구 목록 화면 구성을 위해 [ionic의 ion-item-sliding](http://ionicframework.com/docs/v2/api/components/item/ItemSliding/) Component를 활용할 예정입니다.

먼저 pages 폴더 아래에 follows.html 파일을 생성하겠습니다.

이제 상단 헤더영역과 기본 화면을 생성하겠습니다.

```html
<ion-header>
  <ion-navbar>
    <ion-title>
      Follows
    </ion-title>
  </ion-navbar>
</ion-header>
<ion-content >
  <ion-list >
  </ion-list>
</ion-content>
```

follows.html 과 매핑될 follows.ts을 생성하겠습니다.

```javascript
import { Component } from '@angular/core';

import { NavController, App } from 'ionic-angular';

import {SharedService} from '../../app/sharedService';

@Component({
  selector: 'page-follows',
  templateUrl: 'follows.html'
})
export class FollowsPage {

  users:any[] = [];

  constructor(public navCtrl: NavController, public ss: SharedService, private app:App) {
    var self = this;
  }
}
```

## Tab변경하기
-----------
FollowsPage를 Tab의 첫번째 페이지로 사용할 수 있도록, `tabs.html`과 `tabs.ts`를 수정하겠습니다.

`tabs.html`

```html
<ion-tab [root]="tab1Root" tabTitle="Follows" tabIcon="people"></ion-tab>
```

`tabs.ts`

```javascript
import { Component } from '@angular/core';

import { FollowsPage } from '../follows/follows';

@Component({
  templateUrl: 'tabs.html'
})
export class TabsPage {

  tab1Root: any = FollowsPage;
  ...

  constructor() {

  }
}
```

## S5Platform의 친구목록 조회하기
-----------

친구 목록을 조회할수 있는 함수인 `loadFollows`를 호출해서 친구 목록을 만들어보겠습니다.

```javascript
  this.ss.stalk.loadFollows( function( err, users){
    self.users = users;  
  });
```

이제 users 객체를 사용하여 item을 재구성해보겠습니다.

```html
  <ion-list >
    <ion-item-sliding *ngFor="let user of users; let inx = index">
      <ion-item>
        <ion-avatar item-left>
          <img src="{{user.avatar}}">
        </ion-avatar>
        <h2>{{user.nickName}}</h2>
        <p>{{user.statusMessage}}</p>
      </ion-item>
      <ion-item-options side="right">
        <button ion-button color="danger">
          <ion-icon name="remove-circle"></ion-icon>
          Remove
        </button>
      </ion-item-options>
    </ion-item-sliding>
  </ion-list>
```

여기서는 [ionic의 ion-item-sliding](http://ionicframework.com/docs/v2/api/components/item/ItemSliding/) Component를 활용하여 우측으로 Slide가 가능한 item을 구현하였습니다.

## 사용자검색 화면 호출하기 
-----------
[ionic의 ion-fab](https://ionicframework.com/docs/v2/api/components/fab/FabButton/) 을 화면 우측하단에 추가하고 클릭시 `openSearchUser` 함수를 클릭하도록 구현하겠습니다.

```html
  <ion-fab bottom right>
    <button ion-fab (click)="openSearchUser();"><ion-icon name="add"></ion-icon></button>
  </ion-fab>
```

이제 `openSearchUser` 함수를 생성하여 SearchUserPage 팝업을 열겠습니다.

```javascript
  public openSearchUser = () => {
    this.app.getRootNav().push(SearchUserPage, {callback:this.addFollow, btnNm:"Add"});
  }

  public addFollow = () => {
  }
```

여기서는 [ionic의 NavController](https://ionicframework.com/docs/v2/api/navigation/NavController/)를 사용해서 SearchUserPage 팝업을 열도록 구현하였습니다. `push` 의 두번째 인자로 팝업 결과를 받을 함수를 구현하였고, 팝업창에서 보여줄 버튼 이름을 함께 전송하였습니다.

팝업창은 아래와 같이 열리게 되고, `Add` 버튼을 클릭하면 callback으로 넘긴 `addFollow` 함수를 호출하게 됩니다.

## S5Platform에 친구 추가하기
-----------

stalk의 `createFollow` 함수를 호출하여 친구를 추가할 수 있습니다.

위에서 생성한 addFollow 함수를 아래와 같이 수정하겠습니다.

```javascript
  public addFollow = (userIds) => {
    var self = this;

    this.ss.stalk.createFollow(userIds, function(err, result){
      if( err ){
        alert(err.message);
        return;
      }

      self.ss.stalk.loadFollows( function(err, results){
        self.users = results;
      });
    });
  };
```

## 채팅화면 입장하기
-----------

이제 `ion-item`을 클릭하여 선택한 유저와 채팅이 가능한 화면으로 이동할 수 있도록 onclick 이벤트를 추가해보겠습니다.
```html
<ion-item (click)="gotoChat(user);">
...
</ion-item>
```

```javascript
  public gotoChat = (user) => {
    this.navCtrl.rootNav.push(ChatPage, {users:[user]});
  }
```

[NavController](https://ionicframework.com/docs/v2/api/navigation/NavController/)를 사용해서 ChatPage 로 이동하도록 구현했습니다.

이제 채팅 화면을 만들어보겠습니다.