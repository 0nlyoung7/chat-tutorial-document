# 채널 목록 화면 만들기

이제 친구목록 화면 만들어보겠습니다.

## 화면 구성하기
-----------

채널 목록 화면 구성을 위해 [ionic의 ion-item-sliding](http://ionicframework.com/docs/v2/api/components/item/ItemSliding/) Component를 활용할 예정입니다.

먼저 `pages/channels` 폴더 아래의 `channels.html` 파일을 확인하겠습니다.

상단 헤더영역의 버튼과 검색창이 포함된 화면이 아래와 같이 생성되어 있습니다.

```html
<ion-header>
  <ion-navbar>
    <ion-title>
      Channels
    </ion-title>
  </ion-navbar>
</ion-header>
<ion-content >
</ion-content>
```

`channels.html` 과 매핑되는 `channels.ts`이 아래와 같이 생성하겠습니다.

```javascript
import { Component } from '@angular/core';

import { NavController, App } from 'ionic-angular';

import {SharedService} from '../../app/sharedService';

@Component({
  selector: 'page-channels',
  templateUrl: 'channels.html'
})
export class ChannelsPage {

  channels:any[] = [];
  constructor(public navCtrl: NavController, public ss: SharedService, private app:App) {
    var self = this;
  }  
}
```

## STALK-IM의 채널목록 조회하기
-----------

채널 목록을 조회할수 있는 함수인 `loadChannels`를 호출하는 로직을 생성자 안에 추가해서, 처음 화면에 접속할 경우에 친구 목록을 조회할 수 있도록 구현하겠습니다.

```javascript
  this.ss.stalk.loadChannels( function( err, channels){
    self.channels = channels;  
  });
```

이제 users array와 [ionic의 ion-item-sliding](http://ionicframework.com/docs/v2/api/components/item/ItemSliding/) 사용하여 list를 재구성해보겠습니다.

```html
  <ion-list >
    <ion-item-sliding *ngFor="let channel of channels; let inx = index">
      <ion-item (click)="gotoChat(channel);">
        <ion-avatar item-left>
          <img src="{{channel.image}}">
        </ion-avatar>
        <h2>{{channel.name}}</h2>
        <p>{{channel.latestMessage}}</p>
        <ion-note item-right>{{channel.updated}}</ion-note>
      </ion-item>
      <ion-item-options side="right">
        <button ion-button color="danger" (click)="removeChannel(channel, inx)">
          <ion-icon name="exit"></ion-icon>
          Exit
        </button>
      </ion-item-options>
    </ion-item-sliding>
  </ion-list>
```

여기서는 [ionic의 ion-item-sliding](http://ionicframework.com/docs/v2/api/components/item/ItemSliding/) Component를 활용하여 우측으로 Slide가 가능한 item을 구현하였습니다.

## 채팅화면 입장하기
-----------

이제 `ion-item`을 클릭하여 선택한 유저와 채팅이 가능한 화면으로 이동할 수 있도록 onClick 이벤트를 추가해보겠습니다.
```html
<ion-item (click)="gotoChat(channel);">
...
</ion-item>
```

```javascript
  public gotoChat = (channel) => {
    this.app.getRootNav().push(ChatPage, {channelId:channel.channelId, users:channel.users});
  }
```

Ionic [App](https://ionicframework.com/docs/v2/api/components/app/App/)안의 [NavController](https://ionicframework.com/docs/v2/api/navigation/NavController/)인 getRootNav()를 사용해서 `ChatPage`로 이동하도록 구현했습니다. 이동할 시에 선택된 채널의 channelId와 유저 객체를 파라미터로 넘기고 있습니다. 

이제 채팅 화면을 만들어보겠습니다.