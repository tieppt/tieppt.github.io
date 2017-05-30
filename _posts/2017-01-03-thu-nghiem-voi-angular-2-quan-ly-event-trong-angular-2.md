---
id: 245
title: 'Thử Nghiệm Với Angular 2 Phần 6: Quản Lý Event Trong Angular 2'
date: 2017-01-03T17:08:05+00:00
author: Tiep Phan
layout: post
guid: http://www.tiepphan.com/?p=245
permalink: /thu-nghiem-voi-angular-2-quan-ly-event-trong-angular-2/
beans_layout:
  - default_fallback
image: /assets/uploads/2017/01/angular2-PHAN6.jpg
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
  - Angular Component
  - Javascript
  - Lập Trình Angular 2
  - Web Dev
---
<h2 style="text-align: center;">
  Quản Lý Event Trong Angular 2
</h2>

Trong <a href="http://www.tiepphan.com/thu-nghiem-voi-angular-2-component-va-data-binding/" target="_blank">bài học đầu tiên</a> của series **_Thử Nghiệm Với Angular 2_** chúng ta đã làm quen với **Event Binding** trong Angular 2. Bài học này sẽ củng cố một số kiến thức về Event trong Angular 2 như **_$event_** object hay _**key event filtering**_**_._**

<!--more-->

Angular 2 cung cấp event binding cho bất kỳ event nào của DOM tương ứng như click, keyup, keydown, mouseover, &#8230;

Để binding event cho một phần tử nào bạn chỉ cần bao đóng nó trong cặp dấu **_&#8216;__( <event> )&#8217;_** và bên trong là tên của event, vế tiếp theo của binding là một template statement: có thể là một hàm, hay một câu lệnh thực thi một hành động nào đó.

Ví dụ:

<pre class="brush:html;">&lt;button (click)="onClickMe()"&gt;Click me!&lt;/button&gt;</pre>

  * Thao tác với $event object:

Angular 2 cung cấp một object tên là $event, khi nó được truyền vào là tham số của hàm handle event thì bạn có thể truy cập vào các thuộc tính mà nó chứa trong đó, chẳng hạn event cho keyboard, mouse.

Ví dụ:

<pre class="brush:html;">&lt;input (keyup)="onKey($event)"&gt;
</pre>

<pre class="brush:js;">export class LoggerKeyUpComponent {

  onKey(event: any) { // without type info
    // do something with event param
    console.log(event);
  }
}</pre>

Hoặc có thể có type của event:

<pre class="brush:html;">&lt;input (keyup)="onKey($event)"&gt;
</pre>

<pre class="brush:js;">export class LoggerKeyUpTypeComponent {

  onKey(event: KeyboardEvent) { // with type info
    // do something with event param
    console.log((&lt;HTMLInputElement&gt;event.target).value);
  }
}</pre>

  * Key event filtering

Khi thao tác với keyboard event, có thể chúng ta chỉ muốn lắng nghe trên một key xác định nào đó, lúc đó chúng ta có thể dùng key event filtering.

Ví dụ:

<pre class="brush:html;">&lt;input (keyup.enter)="onEnter($event)"&gt;
</pre>

<pre class="brush:js;">export class KeyUpFilteringComponent {
  value = '';
  onEnter(event) { 
    // do something
    this.value = event.target.value;
  }
}</pre>

Tham khảo thêm tại: <a href="https://angular.io/docs/ts/latest/guide/user-input.html" target="_blank">https://angular.io/docs/ts/latest/guide/user-input.html</a>

Video bài học: