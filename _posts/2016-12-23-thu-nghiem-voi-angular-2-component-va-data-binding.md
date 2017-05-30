---
id: 195
title: 'Thử Nghiệm Với Angular 2 Phần 1: Component và Data Binding'
date: 2016-12-23T03:36:44+00:00
author: Tiep Phan
layout: post
guid: http://www.tiepphan.com/?p=195
permalink: /thu-nghiem-voi-angular-2-component-va-data-binding/
beans_layout:
  - default_fallback
image: /assets/uploads/2016/12/angular2-PHAN1.jpg
categories:
  - Javascript
  - Lập Trình
  - Lập Trình Angular
  - Programming
  - Web
tags:
  - Angular
  - Angular 2
  - Angular Component
  - Javascript
  - Lập Trình Angular 2
  - Web Dev
---
<h2 style="text-align: center;">
  Angular 2 Component và Data Binding
</h2>

Xin chào các bạn, đây là bài học đầu tiên trong series _**Thử Nghiệm Với Angular 2**_, trong bài học này chúng ta sẽ cùng tìm hiểu về Angular 2 Component và Data Binding trong Angular 2.

<!--more-->

Đây là bài kế tiếp bài giới thiệu lúc trước mình đã làm.

<blockquote data-secret="HnvnGsLN9y" class="wp-embedded-content">
  <p>
    <a href="http://www.tiepphan.com/angular-2-ban-da-san-sang-thu-nghiem/">Angular 2 &#8211; Bạn Đã Sẵn Sàng Thử Nghiệm</a>
  </p>
</blockquote>



Từ bài này trở đi mình sẽ tạo video để mô tả trực quan hơn. Các bạn có thể trở lại bài trước để tham khảo các phần mềm cần cài đặt, hay các chi tiết mà trong video mình có thể bỏ qua.

&nbsp;<figure id="attachment_197" style="width: 1280px" class="wp-caption aligncenter">

[<img class="wp-image-197 size-full" src="http://www.tiepphan.com/assets/uploads/2016/12/component-hierarchy.png" alt="Angular 2 Component Hierarchy" width="1280" height="720" srcset="http://www.tiepphan.com/assets/uploads/2016/12/component-hierarchy.png 1280w, http://www.tiepphan.com/assets/uploads/2016/12/component-hierarchy-300x169.png 300w, http://www.tiepphan.com/assets/uploads/2016/12/component-hierarchy-768x432.png 768w, http://www.tiepphan.com/assets/uploads/2016/12/component-hierarchy-1024x576.png 1024w" sizes="(max-width: 1280px) 100vw, 1280px" />](http://www.tiepphan.com/assets/uploads/2016/12/component-hierarchy.png)<figcaption class="wp-caption-text">Hình 1: Angular 2 Component Hierarchy</figcaption></figure> 



&nbsp;

Một số link hữu ích cho các bạn tham khảo về kiến trúc của Angular 2:

Architecture Overview: kiến trúc tổng thể của ứng dụng Angular 2 như Component, NgModule, Data Binding, &#8230;

<a href="https://angular.io/docs/ts/latest/guide/architecture.html" target="_blank">https://angular.io/docs/ts/latest/guide/architecture.html</a>

Template Syntax: Các cú pháp để thao tác trên template.

<a href="https://angular.io/docs/ts/latest/guide/template-syntax.html" target="_blank">https://angular.io/docs/ts/latest/guide/template-syntax.html</a>

Cheat Sheet: từ điển tham khảo cho Angular 2 developer.

<a href="https://angular.io/docs/ts/latest/guide/cheatsheet.html" target="_blank">https://angular.io/docs/ts/latest/guide/cheatsheet.html</a>

Angular-CLI: tool để khởi tạo một app sử dụng Angular 2.

<a href="https://github.com/angular/angular-cli" target="_blank">https://github.com/angular/angular-cli</a>

&nbsp;