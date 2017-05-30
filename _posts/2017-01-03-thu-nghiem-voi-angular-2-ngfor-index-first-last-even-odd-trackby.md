---
id: 242
title: 'Thử Nghiệm Với Angular 2 Phần 5: NgFor Và Các Thuộc Tính Index, First, Last, Even, Odd, TrackBy'
date: 2017-01-03T16:27:03+00:00
author: Tiep Phan
layout: post
guid: http://www.tiepphan.com/?p=242
permalink: /thu-nghiem-voi-angular-2-ngfor-index-first-last-even-odd-trackby/
beans_layout:
  - default_fallback
image: /assets/uploads/2017/01/angular2-PHAN5.jpg
categories:
  - Javascript
  - Lập Trình
  - Lập Trình Angular
  - Programming
  - Web
tags:
  - Angular
  - Angular 2 Directives
  - Angular Component
  - Lập Trình Angular 2
  - NgFor
  - Web Dev
---
<h2 style="text-align: center;">
  NgFor Và Các Thuộc Tính Index, First, Last, Even, Odd, TrackBy
</h2>

Trong bài học số 2, chúng ta đã tìm hiểu về một số <a href="http://www.tiepphan.com/thu-nghiem-voi-angular-2-built-in-directives-ngif-ngfor-ngswitchcase/" target="_blank">Structual Directives</a> trong đó có NgFor. Trong bài học này chúng ta sẽ tìm hiểu một số thuộc tính mà Angular 2 NgFor cung cấp như các biến cục bộ: _index, first, last, even, odd_ hay việc tối ưu performance với _trackBy_ function.

<!--more-->

NgFor cung cấp các biến cục bộ, có thể tạo một alias cho các biến đó như:

  * `index` xác định chỉ số hiện tại của vòng lặp tương ứng.
  * `first` xác định vòng lặp hiện tại có phải là vòng lặp đầu tiên không -> true : false.
  * `last` xác định vòng lặp hiện tại có phải là vòng lặp cuối cùng không -> true : false.
  * `even` xác định vòng lặp hiện tại có phải là vòng lặp có chỉ số chẵn không -> true : false.
  * `odd` xác định vòng lặp hiện tại có phải là vòng lặp có chỉ số lẻ không -> true : false.

NgFor trackBy function:

Angular 2 NgFor khi thực hiện lặp qua một collection sẽ có thể tạo một template cho mỗi item (_**thao tác với các immutable object**_). Khi thực hiện thêm hoặc xóa item trong collection, nó sẽ tạo lại template vì nó không track được các item đó. Lúc này để cái thiện performance chúng ta có thể định nghĩa một phương thức để giúp Angular track được các items đó, đó là lý do ra đời của _**trackBy**_ function.

Cú pháp:

<pre class="brush:html;">&lt;li *ngFor="let item of items; let i = index; trackBy: trackByFn"&gt;
  // do something
&lt;/li&gt;
&lt;li template="ngFor let item of items; let i = index; trackBy: trackByFn"&gt;
  // do something
&lt;/li&gt;</pre>

Sử dụng với cú pháp thẻ _template (sẽ có bài giới thiệu sau)_:

<pre class="brush:html;">&lt;template ngFor let-item [ngForOf]="items" let-i="index" [ngForTrackBy]="trackByFn"&gt;
  &lt;li&gt;...&lt;/li&gt;
&lt;/template&gt;
</pre>

Tìm hiểu thêm tại Angular 2 NgFor Directive: <a href="https://angular.io/docs/ts/latest/api/common/index/NgFor-directive.html" target="_blank">https://angular.io/docs/ts/latest/api/common/index/NgFor-directive.html</a>

Video bài học: