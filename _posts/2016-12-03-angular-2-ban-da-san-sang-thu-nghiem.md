---
id: 131
title: 'Angular 2 &#8211; Bạn Đã Sẵn Sàng Thử Nghiệm'
date: 2016-12-03T00:00:32+00:00
author: Tiep Phan
layout: post
guid: http://www.tiepphan.com/?p=131
permalink: /angular-2-ban-da-san-sang-thu-nghiem/
beans_layout:
  - default_fallback
image: /assets/uploads/2016/11/angular-2.png
categories:
  - Javascript
  - Lập Trình
  - Lập Trình Angular
  - Programming
  - Web
tags:
  - Angular
  - Angular 2
  - ES2015
  - Javascript
  - Nodejs
  - Web Dev
---
<a href="https://angular.io/" target="_blank">Angular 2</a> đã phát hành chính thức, tính tới thời điểm viết bài này, nó đã ra đến phiên bản 2.2.x, hầu hết các blog đều có những lời đánh tiếng về nó và cũng có vô vàn các hướng dẫn để bạn có thể tiếp cận với Angular 2. Vậy bạn đã sẵn sàng để bắt đầu với nền tảng hot này chưa? Hãy cùng mình thử nghiệm với nó nhé.

<!--more-->

<a href="http://www.tiepphan.com/assets/uploads/2016/12/angular-angular.png" target="_blank"><img class="aligncenter wp-image-132 size-full" src="http://www.tiepphan.com/assets/uploads/2016/12/angular-angular.png" alt="Angular 2 repository Github" width="1077" height="275" srcset="http://www.tiepphan.com/assets/uploads/2016/12/angular-angular.png 1077w, http://www.tiepphan.com/assets/uploads/2016/12/angular-angular-300x77.png 300w, http://www.tiepphan.com/assets/uploads/2016/12/angular-angular-768x196.png 768w, http://www.tiepphan.com/assets/uploads/2016/12/angular-angular-1024x261.png 1024w" sizes="(max-width: 1077px) 100vw, 1077px" /></a>

<p style="text-align: center;">
  <em>Hình 1: Repository của Angular 2 trên Github</em>
</p>

* * *

### 1. Giới thiệu.

> <span style="color: #b22222;">Angular is a development platform for building mobile and desktop web applications.</span>

Angular 2 giờ đây là một nền tảng, nó không chỉ là một framework như trước nữa. <a href="https://angular.io/features.html" target="_blank">Giờ đây bạn có thể tạo ứng dụng đa nền tảng như web application, native application, desktop application với Angular 2</a>. Ngoài ra, còn có một cộng đồng lớn mạnh, bạn rất dễ dàng để có thể tiếp cận.

Bằng việc thay đổi lại hoàn toàn kiến trúc của mình, Angular 2 dễ dàng tiếp cận hơn, hiện đại hơn &#8211; component based, hiệu năng cao hơn, nhanh hơn Angular 1. Angular 2 được phát triển từ hơn 5 năm đóng góp ý kiến của cộng đồng người dùng Angular, được viết lại trên <a href="https://www.typescriptlang.org/" target="_blank">TypeScript</a> cho phép các nhà phát triển tận dụng các tính năng nổi bật của ES2015, ES2016+ càng khiến nó trở nên mạnh mẽ hơn trong quá trình phát triển tiếp theo của mình.

Một số tính năng mới của Angular 2 như:

  * Form Builder
  * Change Detection
  * Templating
  * Routing
  * Annotations
  * Observables
  * Shadow DOM

* * *

### 2. Yêu cầu chung.

Trước khi bắt đầu thử nghiệm, chúng ta cần chuẩn bị một số công cụ cho quá trình phát triển phần mềm, trong trường hợp này là tạo các ứng dụng trên nền tảng của Angular 2.

&#8211; <a href="https://nodejs.org/en/" target="_blank">Nodejs:</a>

Các bạn tải về phần cài đặt cho <a href="https://nodejs.org/en/download/" target="_blank">Nodejs tại đây</a>, mình ưu tiên sử dụng bản 6 trở lên và dùng bản LTS. Các bạn tải về và cài đặt theo hệ điều hành đang sử dụng.

&#8211; <a href="https://www.typescriptlang.org/" target="_blank">TypeScript</a>:

Sau khi cài đặt các bạn mở Command Prompt/Terminal lên và cài đặt TypeScript qua npm.

<pre class="brush:shell;">npm install -g typescript
// or short version
npm i -g typescript</pre>

Quá trình cài đặt sẽ cài đặt TypeScript thành global package, có thể chạy command từ bất kỳ đâu.

&#8211; <a href="https://github.com/angular/angular-cli" target="_blank">Angular CLI</a>:

Angular CLI là command line tool giúp bạn tạo một số thành phần hoặc một project Angular 2 từ đầu. Ngoài ra, còn một số thiết lập để bạn bắt đầu với Angular 2 dễ dàng hơn.

Các bạn có thể sử dụng &#8220;<a href="https://github.com/angular/quickstart" target="_blank">Quick start</a>&#8221; hoặc &#8220;<a href="https://github.com/angular/angular2-seed" target="_blank">Angular 2 seed</a>&#8221; nếu muốn, các phần code cũng sẽ giống nhau. Mình sẽ sử dụng Angular CLI.

Để cài đặt Angular CLI, chúng ta cũng làm tương tự như khi cài đặt TypeScript.

<pre class="brush:shell;">npm install -g @angular/cli
// or
npm i -g @angular/cli</pre>

&#8211; Version Control System: <a href="https://git-scm.com/" target="_blank">Git</a> (không bắt buộc)

Hiện nay, việc dùng Git khi phát triển phần mềm rất phổ biến, bạn có thể quản lý các phiên bản một cách dễ dàng. Nếu chưa biết bạn hoàn toàn có thể học cách sử dụng khá nhanh.

Các bạn có thể tải về bản mới nhất từ trang chủ rồi cài đặt cho máy của mình.

&#8211; Editor:

Các bạn có thể dùng bất cứ editor nào mình thích và quen thuộc để làm việc, mình có một số gợi ý: <a href="https://code.visualstudio.com/" target="_blank">Visual Studio Code</a>, <a href="https://www.sublimetext.com/" target="_blank">Sublime Text</a>, <a href="https://atom.io/" target="_blank">Atom</a>, thậm chí <a href="http://www.vim.org/" target="_blank">Vim</a>.

* * *

### 3. Tiến hành.

Đầu tiên, chúng ta tạo mới một project với sự trợ giúp của Angular CLI. Các bạn chạy lệnh dưới đây trong Command Prompt/Terminal.

<pre class="brush:shell;">ng new &lt;app-name&gt;
// example
ng new contact-application
// use preprocessor instead of css: scss, sass, stylus, less
ng new contact-application --style=scss</pre>

Quá trình khởi tạo lần đầu tiên có thể mất vài (chục) phút, để tạo project và download các thư viện cần thiết để ứng dụng có thể thực thi.

Sau khi hoàn thành việc khởi tạo project, chúng ta sẽ có một project với cấu trúc dạng như sau:

[<img class="aligncenter wp-image-147 size-full" src="http://www.tiepphan.com/assets/uploads/2016/12/1-project-structure.png" alt="Project Structure" width="608" height="861" srcset="http://www.tiepphan.com/assets/uploads/2016/12/1-project-structure.png 608w, http://www.tiepphan.com/assets/uploads/2016/12/1-project-structure-212x300.png 212w" sizes="(max-width: 608px) 100vw, 608px" />](http://www.tiepphan.com/assets/uploads/2016/12/1-project-structure.png)

<p style="text-align: center;">
  <em>Hình 2: Project structure</em>
</p>

**Vậy chúng ta phải bắt đầu từ đâu?**

Một ứng dụng Angular 2 xây dựng trên vô số Component.

> _A component controls a patch of screen called a view._

Để dễ hiểu thì Component là tất cả những gì mà end-user có thể nhận biết, nó có thể được sử dụng lại nhiều lần trong một ứng dụng. Sau khi tạo xong một project với Angular CLI, chúng ta đã có một Component có tên: _&#8220;AppComponent&#8221;_ trong file _&#8220;src/app/app.component.ts&#8221;_.

[<img class="aligncenter wp-image-150 size-full" src="http://www.tiepphan.com/assets/uploads/2016/12/2-app-comp.png" alt="AppComponent" width="721" height="365" srcset="http://www.tiepphan.com/assets/uploads/2016/12/2-app-comp.png 721w, http://www.tiepphan.com/assets/uploads/2016/12/2-app-comp-300x152.png 300w" sizes="(max-width: 721px) 100vw, 721px" />](http://www.tiepphan.com/assets/uploads/2016/12/2-app-comp.png)

<p style="text-align: center;">
  <em>Hình 3: AppComponent</em>
</p>

Đoạn code trên có ý nghĩa gì?

* Dòng 1: khi bạn muốn dùng một, một số module, thành phần nào đó từ module khác, bạn import chúng vào, đây là công việc để lấy về các thành phần mà module của bạn phụ thuộc vào. Tính năng này có trong ES2015: modules import. <a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import" target="_blank">Bạn có thể tìm hiểu thêm tại đây</a>.

* Dòng 3 &#8211; 7: _@Component_ để làm gì?

Nó là một khai báo (Decorator) cho class ngay sau đó, để chỉ ra rằng class đó là một Angular component, bên cạnh đó nó còn cung cấp các configuration metadata để Angular 2 biết cách tạo ra Component tương ứng.

[<img class="aligncenter wp-image-152 size-full" src="http://www.tiepphan.com/assets/uploads/2016/12/3-at-component.png" alt="@Component" width="1249" height="662" srcset="http://www.tiepphan.com/assets/uploads/2016/12/3-at-component.png 1249w, http://www.tiepphan.com/assets/uploads/2016/12/3-at-component-300x159.png 300w, http://www.tiepphan.com/assets/uploads/2016/12/3-at-component-768x407.png 768w, http://www.tiepphan.com/assets/uploads/2016/12/3-at-component-1024x543.png 1024w" sizes="(max-width: 1249px) 100vw, 1249px" />](http://www.tiepphan.com/assets/uploads/2016/12/3-at-component.png)

<p style="text-align: center;">
  <em>Hình 4: @Component</em>
</p>

_@Component _yêu cầu tối thiểu phải truyền vào một Javascript object với ít nhất thuộc tính: _selector_ và &#8211; _templateUrl_ hoặc _template_.

Dòng 4: là _selector_ hay mục đích chỉ ra rằng, khi trong template có một thẻ dạng như thẻ HTML có tên _&#8220;app-root&#8221;_ thì Angular sẽ hiển thị Component khai báo bên dưới vào đó. Bạn có thể nhìn thấy thẻ này trong file _&#8220;src/index.html&#8221;_. Trong trường hợp component này, khi render sẽ thay thế phần Text Node &#8220;Loading&#8230;&#8221;.

[<img class="size-full wp-image-159 aligncenter" src="http://www.tiepphan.com/assets/uploads/2016/12/app-root.png" alt="app-root" width="935" height="367" srcset="http://www.tiepphan.com/assets/uploads/2016/12/app-root.png 935w, http://www.tiepphan.com/assets/uploads/2016/12/app-root-300x118.png 300w, http://www.tiepphan.com/assets/uploads/2016/12/app-root-768x301.png 768w" sizes="(max-width: 935px) 100vw, 935px" />](http://www.tiepphan.com/assets/uploads/2016/12/app-root.png)

<p style="text-align: center;">
  <em>Hình 5: Custom tag <app-root> &#8211; selector</em>
</p>

Dòng 5: _templateUrl_ sử dụng để link đến phần template tương ứng của component này. Trong một số trường hợp, các bạn có thể sử dụng inline template mà không cần tạo file html template riêng lẻ, khi đó bạn thay thế việc sử dụng property _templateUrl_ thành property _template_.

Lưu ý, nếu bạn dùng property _template_, bạn có thể sử dụng _multi-line string_ bằng việc bao đóng string trong cặp dấu ` `` `.

[<img class="size-full wp-image-161 aligncenter" src="http://www.tiepphan.com/assets/uploads/2016/12/templateUrl.png" alt="templateurl" width="461" height="279" srcset="http://www.tiepphan.com/assets/uploads/2016/12/templateUrl.png 461w, http://www.tiepphan.com/assets/uploads/2016/12/templateUrl-300x182.png 300w" sizes="(max-width: 461px) 100vw, 461px" />](http://www.tiepphan.com/assets/uploads/2016/12/templateUrl.png)

<p style="text-align: center;">
  <em>Hình 6: templateUrl content</em>
</p>

[<img class="size-full wp-image-162 aligncenter" src="http://www.tiepphan.com/assets/uploads/2016/12/template.png" alt="template" width="532" height="302" srcset="http://www.tiepphan.com/assets/uploads/2016/12/template.png 532w, http://www.tiepphan.com/assets/uploads/2016/12/template-300x170.png 300w" sizes="(max-width: 532px) 100vw, 532px" />](http://www.tiepphan.com/assets/uploads/2016/12/template.png)

<p style="text-align: center;">
  <em>Hình 7: inline template content</em>
</p>

Dòng 6: _styleUrls_ để link đến phần style tương ứng cho component này, giá trị của nó là 1 mảng các files style. Tương tự như template, style có thể sử dụng inline style bằng cách thay vì dùng _styleUrls_ bạn sẽ dùng _styles_ property, nó cũng nhận giá trị là một mảng các string khai báo rule, bạn có thể sử dụng multi-line string như _template_ property.

Khi mới tạo project, component bằng Angular CLI, nội dung trong file style thường không có gì, các bạn có thể tùy ý thêm các rules của mình cho ứng dụng.

Dưới đây là list đầy đủ các properties của object trên. Mình sẽ không giải thích hết ý nghĩa của các properties này, các bạn tìm hiểu xem nhé.

<pre class="brush:js;">{
	animations: "list of animations of this component",
	changeDetection: "change detection strategy used by this component",
	encapsulation: "style encapsulation strategy used by this component",
	entryComponents: "list of components that are dynamically inserted into the view of this component",
	exportAs: "name under which the component instance is exported in a template",
	host: "map of class property to host element bindings for events, properties and attributes",
	inputs: "list of class property names to data-bind as component inputs",
	interpolation: "custom interpolation markers used in this component's template",
	moduleId: "ES/CommonJS module id of the file in which this component is defined",
	outputs: "list of class property names that expose output events that others can subscribe to",
	providers: "list of providers available to this component and its children",
	queries: " configure queries that can be injected into the component",
	selector: "css selector that identifies this component in a template",
	styleUrls: "list of urls to stylesheets to be applied to this component's view",
	styles: "inline-defined styles to be applied to this component's view",
	template: "inline-defined template for the view",
	templateUrl: "url to an external file containing a template for the view",
	viewProviders: "list of providers available to this component and its view children",
}</pre>

* Dòng 8 &#8211; 10:

Định nghĩa class tên là _AppComponent_, với property _title_ sau đó export cho module khác sử dụng, đây là tính năng <a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/export" target="_blank">module export</a> và <a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/class" target="_blank">class</a> trong ES2015.

**Chúng ta vừa hoàn thành duyệt qua một vòng về Component, vậy làm gì tiếp theo đây?**

Câu trả lời là <a href="https://angular.io/docs/ts/latest/guide/ngmodule.html" target="_blank">Angular Module</a> hay NgModule.

> <span style="color: #b22222;"><strong>Angular Modules</strong> help organize an application into cohesive blocks of functionality.</span>
> 
> <span style="color: #b22222;">An Angular Module is a class adorned with the <strong>@NgModule</strong> decorator function. <code>@NgModule</code> takes a metadata object that tells Angular how to compile and run module code. It identifies the module&#8217;s own components, directives and pipes, making some of them public so external components can use them. It may add service providers to the application dependency injectors. And there are many more options covered here.</span>

Trong quá trình phát triển ứng dụng, bạn cũng sẽ thường xuyên gặp các thư viện của Angular là các NgModule chẳng hạn: `FormsModule`, `ReactiveFormsModule`, `HttpModule`, `RouterModule`, etc.

Mỗi ứng dụng Angular đều có ít nhất một Module đó là root Module, là nơi để khởi chạy ứng dụng. Module có thể chứa các components, pipes, directives, services, &#8230;

Thông thường nó được đặt tên là _AppModule_, nhưng bạn hoàn toàn có thể đặt bất kỳ tên nào nếu muốn. Bạn có thể tìm thấy Module đó trong file _&#8220;src/app/app.module.ts&#8221;_.

[<img class="aligncenter wp-image-189 size-full" src="http://www.tiepphan.com/assets/uploads/2016/12/NgModule-root-Module-1.png" alt="NgModule root Module" width="899" height="457" srcset="http://www.tiepphan.com/assets/uploads/2016/12/NgModule-root-Module-1.png 899w, http://www.tiepphan.com/assets/uploads/2016/12/NgModule-root-Module-1-300x153.png 300w, http://www.tiepphan.com/assets/uploads/2016/12/NgModule-root-Module-1-768x390.png 768w" sizes="(max-width: 899px) 100vw, 899px" />](http://www.tiepphan.com/assets/uploads/2016/12/NgModule-root-Module-1.png)

<p style="text-align: center;">
  <em>Hình 8: AppModule &#8211; root Module</em>
</p>

Trong class này chúng ta cũng nhìn thấy phần quen thuộc như trong định nghĩa của một Component: các phần import, export.

Ở đây, mình muốn các bạn chú ý đến @NgModule, nó là một decorator tương tự như @Component, nhưng object truyền vào thì có các properties khác với object của @Component.

Như hình 8, các properties _imports_, _declarations_, _providers_, _bootstrap _có ý nghĩa gì?

  * **The imports array:**

Như mình đã nói, Angular chia thành nhiều Module tương ứng với các tính năng chẳng hạn. Khi một Module cần sử dụng đến tính năng đó, chúng ta cần nói cho Angular biết và import vào khi tạo Module.

Chẳng hạn, ứng dụng mình đã tạo ở trên, mình làm việc với platform là trình duyệt &#8211; browser, nên mình cần import BrowserModule, còn FormsModule để làm việc với form, HttpModule để làm việc với Http như call AJAX, &#8230;

**Lưu ý:**

> <span style="color: #b22222;"><strong>Only <code>NgModule</code> classes</strong> go in the <code>imports</code> array. Don&#8217;t put any other kind of class in <code>imports</code>.</span>
> 
> <span style="color: #b22222;">Don&#8217;t confuse the <code>import</code> statements at the top of the file with the Angular module&#8217;s <code>imports</code> array. They have different jobs.</span>
> 
> <span style="color: #b22222;">The <em>JavaScript</em> <code>import</code> statements give you access to symbols <em>exported</em> by other files so you can reference them within <em>this</em> file. They have nothing to do with Angular and Angular knows nothing about them.</span>
> 
> <span style="color: #b22222;">The <em>module&#8217;s</em> <code>imports</code> array tells Angular about specific Angular modules — classes decorated with <code>@NgModule</code> — that the application needs to function properly.</span>

  * **The declarations array:**

Dùng để khai báo các thành phần: components, directives and pipes mà nó thuộc về Module đó. Tất cả các loại class khác không thuộc ba nhóm trên đều không được cho vào array này.

  * **The providers array:**

Khi làm việc với Dependency injection, chúng ta cần khai báo các Services cho Injector thực hiện việc nạp các dependencies.

  * **The bootstrap array:**

Khi định nghĩa root Module, chúng ta cần nói cho Angular biết chương trình cần bắt đầu ở đâu. Trong hình 8, chúng ta muốn Angular khởi chạy _AppComponent_.

Ngoài ra còn có **exports array** là list các component mà Module này trả về cho Module khác. Các bạn có thể xem thêm về <a href="https://angular.io/docs/ts/latest/guide/architecture.html" target="_blank">kiến trúc của Angular tại đây</a>.

&nbsp;

**Chúng ta đã có Component, có Module, vậy khởi chạy chúng như thế nào?**

Các bạn có thể tìm thấy câu trả lời trong file _&#8220;src/main.ts&#8221;_.

[<img class="aligncenter wp-image-190 size-full" src="http://www.tiepphan.com/assets/uploads/2016/12/main-1.png" alt="Angular 2 main" width="1135" height="767" srcset="http://www.tiepphan.com/assets/uploads/2016/12/main-1.png 1135w, http://www.tiepphan.com/assets/uploads/2016/12/main-1-300x203.png 300w, http://www.tiepphan.com/assets/uploads/2016/12/main-1-768x519.png 768w, http://www.tiepphan.com/assets/uploads/2016/12/main-1-1024x692.png 1024w" sizes="(max-width: 1135px) 100vw, 1135px" />](http://www.tiepphan.com/assets/uploads/2016/12/main-1.png)

<p style="text-align: center;">
  <em>Hình 8: main.ts</em>
</p>

Ở đây, chúng ta đã sử dụng platformBrowser, sau đó bootstrapModule là root Module &#8211; AppModule.

Vậy là tất cả đã rõ, chúng ta có Component, có root Module, có nơi thực hiện bootstrap. Bây giờ, chúng ta sẽ chạy thử chương trình để xem nó hình thù thế nào.

Các bạn vào Command Prompt/Terminal sau đó gõ lệnh _ng serve_ và nhấn enter để Angular CLI thực hiện công việc của nó.

[<img class="size-full wp-image-181 aligncenter" src="http://www.tiepphan.com/assets/uploads/2016/12/ng-serve.png" alt="ng serve" width="583" height="324" srcset="http://www.tiepphan.com/assets/uploads/2016/12/ng-serve.png 583w, http://www.tiepphan.com/assets/uploads/2016/12/ng-serve-300x167.png 300w" sizes="(max-width: 583px) 100vw, 583px" />](http://www.tiepphan.com/assets/uploads/2016/12/ng-serve.png)

<p style="text-align: center;">
  <em>Hình 10: ng serve</em>
</p>

Sau đó, các bạn mở trình duyệt vào truy cập vào địa chỉ <a href="http://localhost:4200/" target="_blank">http://localhost:4200/</a>. Kết quả có dạng như sau.

[<img class="size-full wp-image-182 aligncenter" src="http://www.tiepphan.com/assets/uploads/2016/12/first-app.png" alt="first app" width="957" height="526" srcset="http://www.tiepphan.com/assets/uploads/2016/12/first-app.png 957w, http://www.tiepphan.com/assets/uploads/2016/12/first-app-300x165.png 300w, http://www.tiepphan.com/assets/uploads/2016/12/first-app-768x422.png 768w" sizes="(max-width: 957px) 100vw, 957px" />](http://www.tiepphan.com/assets/uploads/2016/12/first-app.png)

<p style="text-align: center;">
  <em>Hình 11: first app</em>
</p>

Bây giờ, chúng ta thay đổi chút, mình sẽ thay đổi title trong AppComponent thành một chuỗi khác, và xem kết quả.

[<img class="aligncenter wp-image-191 size-full" src="http://www.tiepphan.com/assets/uploads/2016/12/change-app-component.png" alt="Change AppComponent" width="732" height="276" srcset="http://www.tiepphan.com/assets/uploads/2016/12/change-app-component.png 732w, http://www.tiepphan.com/assets/uploads/2016/12/change-app-component-300x113.png 300w" sizes="(max-width: 732px) 100vw, 732px" />](http://www.tiepphan.com/assets/uploads/2016/12/change-app-component.png)

<p style="text-align: center;">
  <em>Hình 12: edit AppComponent</em>
</p>

[<img class="aligncenter size-full wp-image-184" src="http://www.tiepphan.com/assets/uploads/2016/12/result.png" alt="result" width="959" height="526" srcset="http://www.tiepphan.com/assets/uploads/2016/12/result.png 959w, http://www.tiepphan.com/assets/uploads/2016/12/result-300x165.png 300w, http://www.tiepphan.com/assets/uploads/2016/12/result-768x421.png 768w" sizes="(max-width: 959px) 100vw, 959px" />](http://www.tiepphan.com/assets/uploads/2016/12/result.png)

<p style="text-align: center;">
  <em>Hình 13: result</em>
</p>

* * *

### 4. Lời kết:

Angular 2 đang từng bước hoàn thiện và độ nóng của cô nàng này cũng có thừa. Hi vọng với những cái nhìn ban đầu sẽ có nhiều cơ hội để chúng ta từng bước cùng nhau chinh phục.

Hẹn gặp lai các bạn trong bài tiếp theo.

Thân ái!

&nbsp;