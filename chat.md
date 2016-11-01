# 채팅 화면 만들기

이제 채팅 화면을 만들어보겠습니다.

## 1) 화면 구성하기

pages/chat 폴더 아래에 chat.ts 파일을 생성하겠습니다.


```js
export class ChatPage {

  channelId: string; //생성할 channelId
  inputMessage: string; //입력 받을 메시지
  messages:any[] = []; //message의 array

  constructor(private renderer: Renderer, public navCtrl: NavController, public ss: SharedService, private navParams: NavParams) {

    let users = navParams.get('users');    
    var self = this;

    self.channelId = ss.xpush.generateChannelId( users );
  }

  public send = () => {
    this.messages.push( {"content": this.inputMessage, "sr":"S" } );
    this.inputMessage = "";
  }
}
```

이에 대응하는 `chat.html` 파일을 아래와 같이 작성하겠습니다.

```html
<ion-header>
  <ion-navbar>
    <ion-title>
      Chat
    </ion-title>
  </ion-navbar>
</ion-header>

<ion-content padding>
  <ol class="messages">
    <li *ngFor="let message of messages;" [ngClass]="message.sr">
        <span>{{message.content}}</span>
	  </li>
  </ol>
</ion-content>

<ion-footer class="bar-frosted" >
  <ion-item>
    <ion-input [(ngModel)]="inputMessage" placeholder="Start typing..."></ion-input>
    <button ion-button item-right color="primary" style="min-width:50px;" (click)="send()" >Send</button>
  </ion-item>
</ion-footer>
```

## 2) XPush를 이용하여 전송하기

이제 SharedService 로 등록된 xpush 객체를 이용하여 입력된 메세지를 실제로 전송해보겠습니다.

먼저 constructor 함수를 아래와 같이 수정하겠습니다.


```js
this.ss.xpush.on( 'message', function(channel, name, data){

  var msg = decodeURIComponent( data.MG );
  var type = data.TP ? data.TP : '';
  self.messages.push( {content:msg, sr:data.SR, type:type} );
});
```

이제 send 함수를 아래와 같이 수정하겠습니다.

```js
	var msg = encodeURIComponent( this.inputMessage );
	var data = { 'MG' : msg };
	this.ss.xpush.send(this.channelId, 'message', data );
	this.inputMessage = '';
```

이제 XPush를 통해 간단한 text를 주고 받는 모습을 확인하실 수 있습니다.