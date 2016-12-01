# 세팅 화면 만들기

이제 세팅 화면 만들어보겠습니다.

## 화면 구성하기
-----------

먼저 `pages/setting` 폴더 아래의 `setting.html` 파일을 확인하겠습니다.

상단 헤더영역의 버튼과 검색창이 포함된 화면이 아래와 같이 생성되어 있습니다.

사용자 정보를 화면에 표시할 수 있도록 user 객체의 avater, nickName, statusMessage가 매핑되어 있습니다. nickName이나 statusMessage를 클릭하면 내용을 변경하기 위한 팝업이 뜨도록 구현되었습니다.

```html
<ion-header>
  <ion-navbar>
    <ion-title>Setting</ion-title>
  </ion-navbar>
</ion-header>

<ion-content class="profile">
  <ion-list >
    <ion-avatar (click)="selectFile()">
      <img src="{{user.avatar}}">
    </ion-avatar>
    <ion-item detail-none (click)="gotoSettingForm('nickName', 'Nickname')">
      <ion-label stacked>Nickname</ion-label>
      <ion-input type="text" value="{{user.nickName}}" readonly></ion-input>
      <ion-icon name="ios-arrow-forward" item-right></ion-icon>
    </ion-item>

    <ion-item detail-none (click)="gotoSettingForm('statusMessage', 'Status Message')">
      <ion-label stacked>Status Message</ion-label>
      <ion-input type="text" value="{{user.statusMessage}}" placeholder="" readonly></ion-input>
      <ion-icon name="ios-arrow-forward" item-right></ion-icon>
    </ion-item>

  </ion-list>
  <div padding>
    <button ion-button color="primary" block (click)="logOut()">logOut</button>
  </div>
  <input type="file" #fileInput id="file" style="display:none" (change)="onFileChange($event, fileInput.value)" />
</ion-content>
```

`setting.html` 과 매핑되는 `setting.ts`이 아래와 같이 생성되어 있습니다.

프로파일 이미지를 클릭하면 이미지를 선택할 수 있는 인풋 박스가 등장하게 되고 해당 인풋박스를 클릭시 이미지를 업로드할 수 있게 구현되어 있습니다.


```javascript
import { Component, ElementRef, ViewChild, Renderer } from '@angular/core';

import { NavController, App } from 'ionic-angular';

import {SharedService} from '../../app/sharedService';

import { SettingFormPage } from './settingForm';


@Component({
  selector: 'page-setting',
  templateUrl: 'setting.html'
})
export class SettingPage {

  user:any;

  @ViewChild('fileInput') fileInput:ElementRef;

  constructor(private renderer: Renderer, public navCtrl: NavController, public ss: SharedService, private app:App) {
    this.user = {};
  }

  public gotoSettingForm(propKey,propKeyNm){
    this.app.getRootNav().push(SettingFormPage, {"propKey":propKey,"propKeyNm":propKeyNm});
  }

  // 파일 다이얼로그를 오픈하는 이벤트
  public selectFile = () => {
    let event = new MouseEvent('click', {bubbles: true});
    this.renderer.invokeElementMethod(this.fileInput.nativeElement, 'dispatchEvent', [event]);
  }

  // 파일 다이얼로그의 내용이 변경되었을때 프로파일을 변경하는 이벤트
  public onFileChange = ($event, fileValue) => {
    var self = this;

    this.ss.stalk.updateUser( "profileFile", self.fileInput.nativeElement, function(err, user){
      console.log( user );
    });
  }

  public logOut = () => {
    var self = this;

    // Code here
    // stalk-im을 이용하여 로그아웃하기
    // 화면을 root로 이동하기
  }
}
```

## STALK-IM의 로그 아웃 구현하기
-----------

현재 세션을 초기화할 수 있는 함수인 `logOut`를 호출하는 로직을 구현하여 로그아웃 버튼을 눌렀을 때, 세션을 초기화한 후 초기화면으로 이동하도록 구현하였습니다.

```javascript
  public logOut = () => {
    var self = this;
    this.ss.stalk.logOut();
    this.app.getRootNav().popToRoot();
  }
```

## 내용변경 팝업 오픈하기
-----------

`SettingFormPage` 를 호출할 때, 클릭된 필드값을 인자로 넘겨 해당 팝업에서 필드를 수정할 수 있도록 구현하였습니다.

```javascript
  public gotoSettingForm(propKey,propKeyNm){
    this.app.getRootNav().push(SettingFormPage, {"propKey":propKey,"propKeyNm":propKeyNm});
  }
```