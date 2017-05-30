---
id: 206
title: 'Thử Nghiệm Với Angular 2 Phần 2: Built-in Directives NgIf, NgFor, NgSwitchCase'
date: 2016-12-25T16:38:39+00:00
author: Tiep Phan
layout: post
guid: http://www.tiepphan.com/?p=206
permalink: /thu-nghiem-voi-angular-2-built-in-directives-ngif-ngfor-ngswitchcase/
beans_layout:
  - default_fallback
image: /assets/uploads/2016/12/angular2-PHAN2.jpg
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
  - Angular Component
  - Lập Trình Angular 2
  - NgFor
  - NgIf
  - NgSwitchCase
  - Web Dev
---
<h2 style="text-align: center;">
  Angular 2 Built-in Directives NgIf, NgFor, NgSwitchCase
</h2>

Xin chào các bạn, đây là bài học thứ hai trong series _**Thử Nghiệm Với Angular 2**, _trong bài trước mình đã giới thiệu về Angular 2 Component và Data Binding.

<!--more-->

<blockquote data-secret="EjNAoFeu5n" class="wp-embedded-content">
  <p>
    <a href="http://www.tiepphan.com/thu-nghiem-voi-angular-2-component-va-data-binding/">Thử Nghiệm Với Angular 2 Phần 1: Component và Data Binding</a>
  </p>
</blockquote>



Trong bài này chúng ta sẽ đi tìm hiểu các Directives được cung cấp bởi Angular 2 là **_NgIf_**, **_NgFor_**, **_NgSwitchCase_** qua các ví dụ tiếp nối từ bài học trước.

> Lưu ý: để sử dụng các directives này, bạn cần import <a href="https://angular.io/docs/ts/latest/api/platform-browser/index/BrowserModule-class.html" target="_blank">BrowserModule</a> hoặc <a href="https://angular.io/docs/ts/latest/api/common/index/CommonModule-class.html" target="_blank">CommonModule</a>

### NgIf:

Sử dụng khi muốn thêm hoặc xóa bỏ một phần tử khi render. Ví dụ: hiển thị thông báo lỗi khi người dùng nhập form chưa đúng.

Cú pháp:

<pre class="brush:html;">&lt;h2 *ngIf="printable"&gt;{{ message }}&lt;/h2&gt;</pre>

> Lưu ý: đừng quên dấu (_*_) phía trước _ngIf_ directive

Link tham khảo: <a href="https://angular.io/docs/ts/latest/guide/template-syntax.html#ngIf" target="_blank">https://angular.io/docs/ts/latest/guide/template-syntax.html#ngIf</a>

### NgFor

Sử dụng khi muốn render một list các phần tử. Ví dụ: render list các bài học trong một series chẳng hạn.

Cú pháp:

<pre class="brush:html;">&lt;div *ngFor="let contact of contacts"&gt;
  &lt;h3&gt;{{ contact.name }}&lt;/h3&gt;
  &lt;div&gt;
    &lt;img *ngIf="contact.avatar?.url" [src]="contact.avatar?.url" alt="Avatar of {{ contact.name }}"&gt;
  &lt;/div&gt;
&lt;/div&gt;</pre>

> Lưu ý: đừng quên dấu (_*_) phía trước _ngFor_ directive và sử dụng cấu trúc &#8220;_let &#8230; of &#8230;_&#8220;

Link tham khảo: <a href="https://angular.io/docs/ts/latest/guide/template-syntax.html#ngFor" target="_blank">https://angular.io/docs/ts/latest/guide/template-syntax.html#ngFor</a>

### NgSwitchCase

Sử dụng thay thế việc if nhiều lần, tương tự như switch-case trong Javascript.

Cú pháp:

<pre class="brush:html;">&lt;div [ngSwitch]="conditionExpression"&gt;
  &lt;template [ngSwitchCase]="case1Exp"&gt;...&lt;/template&gt;
  &lt;template ngSwitchCase="case2LiteralString"&gt;...&lt;/template&gt;
  &lt;template ngSwitchDefault&gt;...&lt;/template&gt;
&lt;/div&gt;</pre>

Hoặc:

<pre class="brush:html;">&lt;div [ngSwitch]="tabIndex"&gt;
  &lt;div *ngSwitchCase="1"&gt;
    &lt;div&gt;
      Tab content 1
    &lt;/div&gt;
    &lt;p&gt;
      Lorem ipsum dolor sit amet, consectetur adipisicing elit. Labore, rerum.
    &lt;/p&gt;
  &lt;/div&gt;
  &lt;div *ngSwitchCase="2"&gt;
    &lt;div&gt;
      Tab content 2
    &lt;/div&gt;
    &lt;p&gt;
      Raw denim you probably haven't heard of them jean shorts Austin. Nesciunt tofu stumptown aliqua, retro synth master cleanse. Mustache cliche tempor, williamsburg carles vegan helvetica. Reprehenderit butcher retro keffiyeh dreamcatcher synth. Cosby sweater eu banh mi, qui irure terry richardson ex squid. Aliquip placeat salvia cillum iphone. Seitan aliquip quis cardigan american apparel, butcher voluptate nisi qui.
    &lt;/p&gt;
  &lt;/div&gt;
  &lt;div *ngSwitchCase="3"&gt;
    &lt;div&gt;
      Tab content 3
    &lt;/div&gt;
    &lt;p&gt;
      Lorem ipsum dolor sit amet, consectetur adipisicing elit. Ducimus a sequi cupiditate accusantium vitae impedit eum illo voluptatem neque, nisi.
    &lt;/p&gt;
  &lt;/div&gt;
&lt;/div&gt;</pre>

> Lưu ý: không có dấu (_*_) ở phía trước _ngSwitch_ directive. Thay vào đó, sử dụng property binding.
> 
> Đặt dấu (_*_) ở phía trước _ngSwitchCase_và _ngSwitchDefault_. Trường hợp sử dụng với thẻ _template_ như ở ví dụ đầu tiên của ngSwitch thì không.

Link tham khảo: <a href="https://angular.io/docs/ts/latest/guide/template-syntax.html#ngSwitch" target="_blank">https://angular.io/docs/ts/latest/guide/template-syntax.html#ngSwitch</a>

### Video bài học: