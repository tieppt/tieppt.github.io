---
id: 253
title: 'Thử Nghiệm Với Angular 2 Phần 7: Truyền Dữ Liệu Cho Component Với @Input'
date: 2017-01-05T11:32:34+00:00
author: Tiep Phan
layout: post
guid: http://www.tiepphan.com/?p=253
permalink: /thu-nghiem-voi-angular-2-truyen-du-lieu-cho-component-voi-input/
beans_layout:
  - default_fallback
image: /assets/uploads/2017/01/angular2-PHAN7.jpg
categories:
  - Javascript
  - Lập Trình
  - Lập Trình Angular
  - Programming
  - Web
tags:
  - Angular
  - Angular 2
  - Angular 2 Directives
  - Angular @Input
  - Angular Component
  - Component Property Binding
  - Lập Trình Angular 2
  - Web Dev
---
<h2 style="text-align: center;">
  Truyền Dữ Liệu Cho Component Với @Input
</h2>

Trong những bài học trước của series _**Thử Nghiệm Với Angular 2**_, chúng ta đã tạo ra các Component, hầu hết các Component này chứa dữ liệu của chính nó mà không có sự tương tác với các Component khác. Trong bài học này chúng ta sẽ tìm hiểu việc truyền dữ liệu cho Component với **_@Input _**decorator, từ cú pháp, đến các chú ý khi thực hiện truyền dữ liệu cho một Component.

<!--more-->

### 1. @Input sử dụng như thế nào?

  * Đầu tiên, để có thể nhận được dữ liệu từ bên ngoài truyền vào cho một Component chúng ta cần thực hiện khai báo tên của property và gán nó với @Input decorator như sau:

<pre class="brush:js; highlight:[9,10]">import { Component, Input } from '@angular/core';

@Component({
  selector: 'tp-contact-image-detail',
  templateUrl: './contact-image-detail.component.html',
  styleUrls: ['./contact-image-detail.component.scss']
})
export class ContactImageDetailComponent {
  @Input() round: boolean = false;
  @Input() avatarUrl: string = '';

  constructor() {}
}
</pre>

  * Sau đó khi sử dụng ở trong một template của một Component khác chúng ta có thể truyền dữ liệu như sau:

<pre class="brush:html;highlight:[2,3]">&lt;tp-contact-image-detail
  [avatarUrl]="contact.avatar?.url"
  [round]="contact.avatar?.round"
&gt;
&lt;/tp-contact-image-detail&gt;</pre>

  * Hoặc sử dụng get/set để thực thi một số hành động như chuẩn hóa dữ liệu đầu vào cho property của Component.

<pre class="brush:js;highlight:[18,19]">import { Component, Input } from '@angular/core';

@Component({
  selector: 'tp-contact-image-detail',
  templateUrl: './contact-image-detail.component.html',
  styleUrls: ['./contact-image-detail.component.scss']
})
export class ContactImageDetailComponent {
  @Input() round: boolean = false;
  _avatarUrl: string = '';

  constructor() {}

  get avatarUrl() {
    return this._avatarUrl;
  }
  
  @Input()
  set avatarUrl(value: string) {
    this._avatarUrl = value.trim();
  }
}
</pre>

Cách sử dụng tương tự như ở ví dụ phía trên.

### 2. Yêu cầu một property nào đó là required cho một Component.

Bạn có thể kiểm tra property trong life-cycle là OnInit hoặc OnChange.

<pre class="brush:js;highlight:[23]">import { Component, OnInit, Input } from '@angular/core';

@Component({
  selector: 'tp-contact-image-detail',
  templateUrl: './contact-image-detail.component.html',
  styleUrls: ['./contact-image-detail.component.scss']
})
export class ContactImageDetailComponent implements OnInit {
  @Input() round: boolean = false;
  _avatarUrl: string = '';

  constructor() {}

  get avatarUrl() {
    return this._avatarUrl;
  }
  
  @Input()
  set avatarUrl(value: string) {
    this._avatarUrl = value.trim();
  }

  ngOnInit() {
    if (!this.avatarUrl) {
      throw new Error('avatarUrl is required!');
    }
  }
}
</pre>

### 3. Đặt một tên cho property lúc truyền vào khác với tên sử dụng trong Component.

Bạn có thể tạo ra một alias cho một property với cú pháp như sau:

<pre class="brush:js;highlight:[10]">import { Component, Input } from '@angular/core';

@Component({
  selector: 'tp-contact-image-detail',
  templateUrl: './contact-image-detail.component.html',
  styleUrls: ['./contact-image-detail.component.scss']
})
export class ContactImageDetailComponent {
  @Input() round: boolean = false;
  @Input('avatar-url') avatarUrl: string = '';

  constructor() {}
}
</pre>

Hoặc:

<pre class="brush:js;highlight:[18]">import { Component, OnInit, Input } from '@angular/core';

@Component({
  selector: 'tp-contact-image-detail',
  templateUrl: './contact-image-detail.component.html',
  styleUrls: ['./contact-image-detail.component.scss']
})
export class ContactImageDetailComponent implements OnInit {
  @Input() round: boolean = false;
  _avatarUrl: string = '';

  constructor() {}

  get avatarUrl() {
    return this._avatarUrl;
  }
  
  @Input('avatar-url')
  set avatarUrl(value: string) {
    this._avatarUrl = value.trim();
  }

  ngOnInit() {
    if (!this.avatarUrl) {
      throw new Error('avatarUrl is required!');
    }
  }
}
</pre>

Khi sử dụng binding data, bạn sẽ bind cho alias:

<pre class="brush:html;highlight:[2]">&lt;tp-contact-image-detail
  [avatar-url]="contact.avatar?.url"
  [round]="contact.avatar?.round"
&gt;
&lt;/tp-contact-image-detail&gt;</pre>

### 4. Sử dụng **_inputs_** array trong **_@Component()_** để thay thế cho việc dùng **_@Input._**

Lưu ý: cách này thường không được khuyến cáo sử dụng như @Input.

<pre class="brush:js;highlight:[7]">import { Component, OnInit, Input } from '@angular/core';

@Component({
  selector: 'tp-contact-image-detail',
  templateUrl: './contact-image-detail.component.html',
  styleUrls: ['./contact-image-detail.component.scss'],
  inputs: ['avatarUrl']
})
export class ContactImageDetailComponent implements OnInit {
  @Input() round: boolean = false;
  _avatarUrl: string = '';

  constructor() {

  }

  get avatarUrl() {
    return this._avatarUrl;
  }

  set avatarUrl(value: string) {
    this._avatarUrl = value.trim();
  }

  ngOnInit() {
    if (!this.avatarUrl) {
      throw new Error('avatarUrl is required!');
    }
  }
}
</pre>

Khai báo trong inputs array có sử dụng alias:

<pre class="brush:js;highlight:[7]">import { Component, OnInit, Input } from '@angular/core';

@Component({
  selector: 'tp-contact-image-detail',
  templateUrl: './contact-image-detail.component.html',
  styleUrls: ['./contact-image-detail.component.scss'],
  inputs: ['avatarUrl:avatar-url']
})
export class ContactImageDetailComponent implements OnInit {
  @Input() round: boolean = false;
  _avatarUrl: string = '';

  constructor() {

  }

  get avatarUrl() {
    return this._avatarUrl;
  }

  set avatarUrl(value: string) {
    this._avatarUrl = value.trim();
  }

  ngOnInit() {
    if (!this.avatarUrl) {
      throw new Error('avatarUrl is required!');
    }
  }
}
</pre>

> Lưu ý: alias sẽ có thứ tự `internalProp:externalProp` ví dụ như `avatarUrl:avatar-url`.

Link tham khảo: <a href="https://angular.io/docs/ts/latest/cookbook/component-communication.html" target="_blank">https://angular.io/docs/ts/latest/cookbook/component-communication.html</a>

### 5. Video bài học: