---
id: 225
title: 'Thử Nghiệm Với Angular 2 Phần 4: Built-in Directives NgStyle, NgClass'
date: 2016-12-29T06:38:51+00:00
author: Tiep Phan
layout: post
guid: http://www.tiepphan.com/?p=225
permalink: /thu-nghiem-voi-angular-2-built-in-directives-ngstyle-ngclass/
beans_layout:
  - default_fallback
image: /assets/uploads/2016/12/angular2-PHAN4.jpg
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
  - Javascript
  - Lập Trình Angular 2
  - Style Component
  - Web Dev
---
<h2 style="text-align: center;">
  Built-in Directives NgStyle, NgClass
</h2>

Trong những bài trước của series _**Thử Nghiệm Với Angular 2**_ mình đã giới thiệu một số_** **_<a href="https://angular.io/docs/ts/latest/guide/structural-directives.html" target="_blank">Structual Directives</a> và hướng dẫn <a href="http://www.tiepphan.com/thu-nghiem-voi-angular-2-style-component-view-encapsulation/" target="_blank">Style cho Component, View Encapsulation</a>. Trong bài học này, chúng ta sẽ tìm hiểu về những Attribute Directives như <a href="https://angular.io/docs/ts/latest/guide/template-syntax.html#!#ngStyle" target="_blank">NgStyle</a>, <a href="https://angular.io/docs/ts/latest/guide/template-syntax.html#!#ngClass" target="_blank">NgClass </a>và property binding cho class, style.

<!--more-->

  1. ### Style binding:
    
    <div>
      <p>
        Cú pháp:
      </p>
      
      <pre class="brush:html;">[style.style-property]="expression"
// or camel case
[style.styleProperty]="expression"
// or
[style.style-property.&lt;unit&gt;]="expression"
// or
[style.styleProperty.&lt;unit&gt;]="expression"
</pre>
      
      <p>
        Example:
      </p>
      
      <pre class="brush:html;">&lt;button [style.color]="isSpecial ? 'red': 'green'"&gt;Some button&lt;/button&gt;

&lt;button [style.font-size.em]="isSpecial ? 3 : 1" &gt;Yes, another button&lt;/button&gt;
&lt;button [style.font-size.%]="!isSpecial ? 150 : 50" &gt;Huh, button again&lt;/button&gt;

// same for camel case and unit px, rem, vh, vw, ...</pre>
    </div>

  2. ### NgStyle:
    
    <div>
      <p>
        Cú pháp:
      </p>
      
      <pre class="brush:html;">&lt;some-element [ngStyle]="{'font-style': styleExp}"&gt;...&lt;/some-element&gt;
&lt;some-element [ngStyle]="{'max-width.px': widthExp}"&gt;...&lt;/some-element&gt;
&lt;some-element [ngStyle]="objExp"&gt;...&lt;/some-element&gt;

</pre>
      
      <p>
        Example:
      </p>
      
      <pre class="brush:html;">&lt;div [ngStyle]="{
    // CSS property names
    'font-style': this.canSave ? 'italic' : 'normal',        // italic
    'font-weight': !this.isUnchanged ? 'bold' : 'normal',    // normal
    'font-size.px': this.isSpecial ? 24 : 8,                 // with unit
  }"&gt;
  This div is cool.
&lt;/div&gt;
</pre>
    </div>

  3. ### Class binding:
    
    <div>
      <p>
        Cú pháp:
      </p>
      
      <pre class="brush:html;">[class.&lt;className&gt;]="expression"
</pre>
      
      <p>
        Example:
      </p>
      
      <pre class="brush:html;">&lt;button class="btn" [class.active]="isActive"&gt;
    Some button need toggle class active base on isActive variable
&lt;/button&gt;

&lt;button class="btn" [class.active]="tabIndex == 1"&gt;
    Some button need toggle class active base on conditional tabIndex == 1
&lt;/button&gt;</pre>
    </div>

  4. ### NgClass:
    
    <div>
      <p>
        Cú pháp:
      </p>
      
      <pre class="brush:html;">&lt;some-element ngClass="first second"&gt;...&lt;/some-element&gt;     // bind string
&lt;some-element [ngClass]="'first second'"&gt;...&lt;/some-element&gt; // bind string value
&lt;some-element [ngClass]="['first', 'second']"&gt;...&lt;/some-element&gt; // bind array
&lt;some-element [ngClass]="{    // bind object
    'first': true,
    'second': true,
    'third': false
    }"&gt;
    ...
&lt;/some-element&gt;
&lt;some-element [ngClass]="stringExp|arrayExp|objExp"&gt;    // variable
    ...
&lt;/some-element&gt;</pre>
      
      <blockquote>
        <p>
          &#8211; <em><strong>string:</strong> </em>là một list các CSS class, cách nhau bởi dấu cách.
        </p>
        
        <p>
          &#8211; <em><strong>array:</strong> </em>là array string CSS class.
        </p>
        
        <p>
          &#8211; <em><strong>object:</strong></em> key -> value, nếu value = true thì add, ngược lại thì remove.
        </p>
      </blockquote>
      
      <p>
        Example:
      </p>
      
      <pre class="brush:html;">&lt;button class="btn" [ngClass]="'active btn-primary'"&gt;
    String binding
&lt;/button&gt;
// or
&lt;button class="btn" ngClass="active btn-primary"&gt;
    String binding
&lt;/button&gt;
// or
&lt;button class="btn" [ngClass]="['active', 'btn-primary']"&gt;
    Array binding
&lt;/button&gt;
// or
&lt;button class="btn" [ngClass]="{active: tabIndex == 1}"&gt;
    Object binding
&lt;/button&gt;</pre>
    </div>

Video bài học: