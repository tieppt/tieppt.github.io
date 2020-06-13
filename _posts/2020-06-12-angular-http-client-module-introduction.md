---
id: 2020
title: 'Angular HTTP Client Module - P1. Giới thiệu'
date: 2020-06-12T00:00:00+00:00
author: Tuan Anh
layout: post
guid: https://www.tiepphan.com/?p=2020
permalink: /angular-http-client-module-introduction/
description: 'Cùng nhau khám phá về Http Client Module trong Angular. Giới thiệu về HTTP Client Module'
image: https://res.cloudinary.com/tuananh-asia/image/upload/v1591951988/HTTP%20CLIENT%20MODULE/angular-http-client_t5qody.png
categories:
  - Lập Trình Angular
  - Programming
  - Web Development

tags:
  - Angular
  - Angular 2

---

# Mở đầu
{:.no_toc}

Khi lập trình ứng dụng với Angular, chúng ta thường nhận, gửi dữ liệu bằng các gọi API tới 1 server khác. Vậy làm cách nào chúng ta có thể gửi? Angular đã cung cấp cho chúng ta một module tên là HTTP Client Module.  

Trong bài này, chúng ta sẽ tìm hiểu xem HTTP Client là gì và lợi ích của nó mang lại khi lập trình ứng dụng với Angular như thế nào nhé.  

* ToC
{:toc}
{:.tp__toc}

## 1. Giới thiệu
{:#post-intro}
Http Client là một Service Module(*) được cung cấp bởi Angular giúp chúng ta thực hiện những yêu cầu Http, dễ dàng custom các request option và handle error một cách dễ dàng.  
Bản chất nó được gọi là Service Module vì nó chỉ init các services (http client, http backend, etc) và không export bất cứ component hay directive nào.  
<img class="img-responsive" src="//res.cloudinary.com/tuananh-asia/image/upload/v1591952900/HTTP%20CLIENT%20MODULE/http-client-module-core_rbhuy6.png" alt="Video Gif"/>  
  
Đây là nơi khai báo Http Client Module trong core của Angular.  -> <a href="//github.com/angular/angular/blob/fe0782afa9d23b328315b07a65f79f5a4f23074e/packages/common/http/src/module.ts#L166">Refer here</a>

<img class="img-responsive" src="/assets/uploads/2020/06/resolver-diagram.png" alt="Resolver Flow"/>  
  
Đây là flow của một lần navigation sử dụng resolver.  

{:toc}
## 2. Tạo một resolver đơn giản
{:#create-simple-resolver}
Một Resolver đơn giản để fetch chi tiết của một bài viết:  
```typescript
  import { Injectable } from "@angular/core";
  import { Resolve, ActivatedRouteSnapshot, RouterStateSnapshot, Router } from '@angular/router';
  import { BlogItem } from '../models/blog-item.model';
  import { BlogService } from '../services/blog.service';
  import { delay, catchError } from 'rxjs/operators';
  import { of } from 'rxjs';

  @Injectable()
  export class BlogDetailResolver implements Resolve<BlogItem> {
    constructor(private blogService: BlogService, private router: Router) {}

    resolve(route: ActivatedRouteSnapshot, state: RouterStateSnapshot) {
      return this.blogService.getBlogDetail(route.params['id']).pipe(
        delay(2000),
        catchError(error => {
          this.router.navigateByUrl('/404');
          return of(null);
        }
      ));
    }
  }
```  

Tiếp theo, chúng ta config routing cho blog module  

```typescript  
  import { NgModule } from '@angular/core';
  import { Routes, RouterModule } from '@angular/router';
  import { BlogListComponent } from './pages/blog-list/blog-list.component';
  import { BlogDetailComponent } from './pages/blog-detail/blog-detail.component';
  import { BlogDetailResolveComponent } from './pages/blog-detail-resolve/blog-detail-resolve.component';
  import { BlogDetailResolver } from './resolvers/blog-detail.resolve';

  const routes: Routes = [
    {
      path: '',
      component: BlogListComponent
    },
    {
      path: ':id',
      component: BlogDetailComponent
    },
    {
      path: 'resolve/:id',
      component: BlogDetailResolveComponent,
      resolve: {
        blog: BlogDetailResolver
      }
    }
  ];

  @NgModule({
    imports: [RouterModule.forChild(routes)],
    exports: [RouterModule]
  })
  export class BlogRoutingModule { }
```  
 
Và đây là component cho chi tiết bài viết: 
```typescript
  import { Component, OnInit, OnDestroy } from '@angular/core';
  import { ActivatedRoute } from '@angular/router';
  import { BlogItem } from '../../models/blog-item.model';
  import { distinctUntilChanged, takeUntil } from 'rxjs/operators';
  import { Subject } from 'rxjs';

  @Component({
    selector: 'app-blog-detail-resolve',
    templateUrl: './blog-detail-resolve.component.html',
    styleUrls: ['./blog-detail-resolve.component.scss']
  })
  export class BlogDetailResolveComponent implements OnInit, OnDestroy {
    blog: BlogItem;
    private onDestroy$: Subject<boolean> = new Subject<boolean>();
    constructor(private readonly route: ActivatedRoute) {
    }

    ngOnInit(): void {
      console.log("Go here");
      this.route.params.pipe(
        takeUntil(this.onDestroy$),
        distinctUntilChanged()
      ).subscribe(params => {
        this.blog = this.route.snapshot.data.blog;
      })
    }

    ngOnDestroy(): void {
      this.onDestroy$.next(true);
      this.onDestroy$.complete();
    }
  }

```

Đây là component khi resolve xong. Hãy để ý tới `console.log("Go here")` ở trong ngOnInit nhé.  
Và đây là kết quả:  
<img class="img-responsive" src="//media.giphy.com/media/QtvNKPx7Ogucu7fdMS/giphy.gif" alt="Video Console"/>  
  
Khi mình bấm vào title của post để vào router detail. phải chờ resolver xong thì component mới init.  
Bây giờ hãy tưởng tượng một HTTP  request response data trong 5 giây. Sau khi nhấp vào một liên kết trong ứng dụng, HTTP request bắt đầu. Việc định tuyến sẽ không được hoàn thành trước khi HTTP request đó response dữ liệu. Do đó, nếu HTTP mất 5 giây, thì cũng sẽ mất 5 giây để quá trình định tuyến hoàn tất.  
  

Vậy là các bạn đã hiểu về resolver rồi chứ ??  
  
{:toc}
## 3. Nếu không xài resolver?
{:#if-not-use-resolver}
  
Ôi vậy còn cách như bình thường hay xài (Fetch dữ liệu chi tiết của bài viết trong hook ngOnInit trong component mà không xài resolver) thì sao nhỉ? 
Lúc này. Điều hướng sẽ chạy ngay qua trang chi tiết blog mà không gặp bất cử trở ngại nào. Thêm nữa, component của chúng ta nên thêm loading indicator, skeleton loading, etc cho mục đích trải nghiệm người dùng. 

Đây là component mình không xài resolver. Mình thêm loading để user biết và chờ load dữ liệu xong.  

```typescript
  import { Component, OnInit, OnDestroy } from '@angular/core';
  import { BlogService } from '../../services/blog.service';
  import { Observable, Subject } from 'rxjs';
  import { BlogItem } from '../../models/blog-item.model';
  import { ActivatedRoute } from '@angular/router';
  import { takeUntil } from 'rxjs/operators';

  @Component({
    selector: 'app-blog-detail',
    templateUrl: './blog-detail.component.html',
    styleUrls: ['./blog-detail.component.scss']
  })
  export class BlogDetailComponent implements OnInit, OnDestroy {
    blog$: Observable<BlogItem>;
    private onDestroy$: Subject<boolean> = new Subject<boolean>();

    constructor(private readonly blogService: BlogService, private readonly route: ActivatedRoute) { }

    ngOnInit(): void {
      this.route.params.pipe(takeUntil(this.onDestroy$)).subscribe((params: any) => {
        const { id } = params;
        this.blog$ = this.blogService.getBlogDetail(id);
      });
    }

    ngOnDestroy(): void {
      this.onDestroy$.next(true);
      this.onDestroy$.complete();
    }
  }

```  
Và kết quả là: Mình đợi get dữ liệu từ API xong rồi hiển thị ra cho người dùng. Để cho họ biết là mình đang lấy dữ liệu lên, mình sẽ hiển thị một dòng text `Loading....`  
Không nên để trang trắng nhé. Người dùng cữ ngỡ là trang này đang bị lỗi mà không hề hay biết mình đang get dữ liệu ở dưới.  

<img class="img-responsive" src="//media.giphy.com/media/S3tsS08K8ub0TkTIed/giphy.gif" alt="Video No Resolver"/>  

Flow khi không có Resolver:  
<img class="img-responsive" src="/assets/uploads/2020/06/no-resolver-diagram.png" alt="Resolver Flow"/>  

Đấy. Độ trễ khi navigation gần như không có. Trải nghiệm người dùng tốt hơn nhiều so với đợi 1 khoảng thời gian rồi mới navigation (tưởng bị lỗi chứ :3)

## 4. Kết luận
{:#conclusion}

Vậy là các bạn đã hiểu `Resolver` rồi chứ ?  
Một trang SPA theo mình nghĩ là nên luôn luôn phải react fast và dữ liệu nên tải không đồng bộ khi chạy. Nên các bạn cân nhắc kỹ trước khi xài `Resolver` nhé.

Bản thân mình không phải fan của Resolver và cũng không xài Resolver trong các dự án của mình. Mình viết lên đây nhằm mục đích chia sẻ kiến thức và được học hỏi, nhận những comment góp ý của mọi người. Xin cảm ơn.

Đây là github repo cho bài viết này nhé. Các bạn có thể clone về và trải nghiệm. -> [Resolver Angular](https://github.com/tuananhitoct/angular-resolver)  
  
Link tham khảo:  
    + <a href="https://angular.io/api/router/Resolve">https://angular.io/api/router/Resolve</a>  
    + <a href="https://www.digitalocean.com/community/tutorials/angular-route-resolvers">https://www.digitalocean.com/community/tutorials/angular-route-resolvers</a>  
    + <a href="https://codeburst.io/understanding-resolvers-in-angular-736e9db71267">https://codeburst.io/understanding-resolvers-in-angular-736e9db71267</a>  
    + <a href="https://dzone.com/articles/understanding-angular-route-resolvers-by-example">https://dzone.com/articles/understanding-angular-route-resolvers-by-example</a>  
