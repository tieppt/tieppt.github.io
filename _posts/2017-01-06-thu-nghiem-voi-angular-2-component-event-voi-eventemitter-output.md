---
id: 268
title: 'Thử Nghiệm Với Angular 2 Phần 8: Component Event Với EventEmitter Và @Output'
date: 2017-01-06T16:53:27+00:00
author: Tiep Phan
layout: post
guid: http://www.tiepphan.com/?p=268
permalink: /thu-nghiem-voi-angular-2-component-event-voi-eventemitter-output/
beans_layout:
  - default_fallback
image: /assets/uploads/2017/01/angular2-PHAN8-Componen-Event-Voi-EventEmitter-Va-Output.jpg
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
  - Angular 2 Event
  - Angular @Output
  - Angular Component
  - Lập Trình Angular 2
  - Web Dev
---
<h2 style="text-align: center;">
  Component Event Với EventEmitter Và @Output
</h2>

Sau khi tìm hiểu bài học <a href="http://www.tiepphan.com/thu-nghiem-voi-angular-2-truyen-du-lieu-cho-component-voi-input/" target="_blank" rel="noopener noreferrer">Truyền Dữ Liệu Cho Component Với @Input</a> trong series _**Thử Nghiệm Với Angular 2**_, chúng ta đã biết cách để Component có thể nhận dữ liệu đầu vào. Bài học &#8220;Component Event Với EventEmitter Và @Output&#8221; sẽ giới thiệu cách để Component phát sinh Event để tương tác từ Component con với Component cha.

<!--more-->

### 1. @Output sử dụng như thế nào?

Tương tự như @Input, việc sử dụng @Output cũng rất dễ dàng.

Bạn cần đặt một tên cho event hoặc có thể thêm alias, sau đó gán nó với @Output decorator và một phép gán cho một đối tượng thuộc EventEmitter như sau:

<pre class="brush:js; highlight:[16]" title="switches.component.ts">import { 
  Component, 
  OnInit, 
  Input, 
  Output, 
  EventEmitter 
} from '@angular/core';

@Component({
  selector: 'tp-switches',
  templateUrl: './switches.component.html',
  styleUrls: ['./switches.component.scss']
})
export class SwitchesComponent implements OnInit {
  @Input() checked: boolean = false;
  @Output('checkedChange') change = new EventEmitter&lt;boolean&gt;();

  constructor() { }

  ngOnInit() {
  }
}
</pre>

Khi có một hành động nào đó xảy ra, bạn sẽ kích hoạt event đó và trả về kèm theo một giá trị có thể là kiểu primitive hoặc các kiểu phức hợp như object, array, &#8230;

<pre class="brush:js; highlight:[6]" title="switches.component.ts">...

export class SwitchesComponent implements OnInit {
  ...
  emitChangeValue(event) {
    this.change.emit(event.target.checked);
  }
}
</pre>

<pre class="brush:html;" title="switches.component.html">&lt;label class="tp-toggle tp-toggle--round tp-toggle--flat"&gt;
  &lt;input type="checkbox"
    class="tp-toggle__checkbox"
    [checked]="checked"
    (change)="emitChangeValue($event)"/&gt;
  &lt;span class="tp-toggle__icon"&gt;&lt;/span&gt;
&lt;/label&gt;
</pre>

### 2. Listener event ở Component cha như thế nào?

Ở Component cha, bạn lắng nghe event như các event thông thường được bao đóng bởi cặp dấu _**&#8220;()&#8221;.**_

<pre class="brush:html;highlight:[2]" title="contact-list.component.html">&lt;tp-switches [checked]="contact.avatar?.round"
  (checkedChange)="switchesValueChange($event, i)"&gt;
&lt;/tp-switches&gt;
</pre>

<pre class="brush:js;highlight:[4]" title="contact-list.component.ts">...
export class ContactListComponent {
  ...
  switchesValueChange(event: boolean, index) {
    this.contacts[index].avatar.round = event;
  }
}</pre>

  * Sử dụng _**output**_**_s_** array trong **_@Component()_** để thay thế cho việc dùng **_@Output._**

Lưu ý: cách này thường không được khuyến cáo sử dụng như @Output.

<pre class="brush:js; highlight:[13,16]" title="switches.component.ts">import { 
  Component, 
  OnInit, 
  Input, 
  Output, 
  EventEmitter 
} from '@angular/core';

@Component({
  selector: 'tp-switches',
  templateUrl: './switches.component.html',
  styleUrls: ['./switches.component.scss'],
  outputs: ['change:checkedChange']
})
export class SwitchesComponent implements OnInit {
  @Input() checked: boolean = false;
  public change = new EventEmitter&lt;boolean&gt;();

  constructor() { }

  ngOnInit() {
  }
  emitChangeValue(event) {
    this.change.emit(event.target.checked);
  }
}</pre>

> Lưu ý: alias sẽ có thứ tự `internalEvent:externalEvent` ví dụ như `change:checkedChange`.

Link tham khảo: <a href="https://angular.io/docs/ts/latest/cookbook/component-communication.html" target="_blank" rel="noopener noreferrer">https://angular.io/docs/ts/latest/cookbook/component-communication.html</a>

Source code các bạn lấy theo branch để theo dõi: <a href="https://github.com/tieppt/try-angular/tree/lesson-8" target="_blank" rel="noopener noreferrer">https://github.com/tieppt/try-angular/tree/lesson-8</a>

### 3. Video bài học: