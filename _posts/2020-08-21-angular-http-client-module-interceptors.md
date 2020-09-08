---
id: 2024
title: 'Angular HTTP Client Module - P3. Interceptors'
date: 2020-08-21T00:00:00+00:00
author: Tuan Anh
layout: post
guid: https://www.tiepphan.com/?p=2024
permalink: /angular-http-client-module-interceptor/
description: 'Cùng nhau khám phá về Http Client Interceptor và đi qua một số ví dụ nhé.'
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

Trong bài trước, mình đã giới thiệu sơ qua về `Options` trong `HttpClientModule` và demo ứng dung `upload/download` rồi.  
Hôm nay mình sẽ đi tới phần khá quan trọng mà bất cứ khi nào làm việc với `HttpClientModule` trong các dự án các bạn đều phải nhờ tới nó :))  
Đó chính là `Interceptor`. Yeah yeah, bắt đầu thôi!  

* ToC
{:toc}
{:.tp__toc}

## 1. Mở đầu
{:#post-intro}
Khi lập trình ứng dụng với Angular, chúng ta thường phải làm việc với API để gửi nhận và gửi dữ liệu. Hầu hết API đều yêu cầu xác thực (Authentication) thì dữ liệu mới được trả về. Vậy trên ứng dụng Angular của chúng ta phải làm thế nào để API server biết mình đã đăng nhập rồi.   
Rồi ok. Chúng ta sẽ cho nó biết là chúng ta đã đăng nhập và đang có 1 token và sẽ gửi lên cho nó xác thực  
```typescript
  getData() {
    return this.httpClient.get('YOUR_GET_DATA_API_ENPOINT', {
      headers: new HttpHeaders().set('Authorization', localStorage.getItem('AUTH'))
    });
  }
```
Và đây là kết quá. Hô hô. Chúng ta đã gửi lên cho nó rồi. Giờ chỉ ngồi đợi data nó trả về thôi.  
<img
  src="https://res.cloudinary.com/tuananh-asia/image/upload/v1598025103/HTTP%20CLIENT%20MODULE/get-data-without-interceptor_uro54y.png"
  alt="Get data without interceptor" class="img-responsive"
/>
<br>
<b>Nhưng chả lẽ chỗ nào cần truyền cái `Token` thì mình cũng đều set headers như trên sao? Chưa kể còn nhiều tác vụ khác.</b>  
Oh my god. Làm sao đây? Mọi việc khó đã có Angular lo.  
Từ Angular 4.3 trở đi, Angular đã mang tới cho chúng ta `HttpInterceptor`  
<img
  src="https://res.cloudinary.com/tuananh-asia/image/upload/v1598025952/HTTP%20CLIENT%20MODULE/whatisthat_jy2kl5.gif"
  alt="What is that" class="img-responsive"
/>

## 2.Giới thiệu Interceptor
`Interceptor` là một loại `Angular Service` cho phép chúng ta chặn các `Http Request` & `Http Response`.  
Vậy `Interceptor` hính dáng nó thế nào, chúng ta cùng xem qua nhé.  
```typescript
import { Injectable } from '@angular/core';
import { HttpInterceptor, HttpRequest, HttpHandler } from '@angular/common/http';

@Injectable()
export class TokenInterceptor implements HttpInterceptor {
  intercept(request: HttpRequest<any>, next: HttpHandler) {
    // do something here
    return next.handle(request);
  }
}
```
Bằng cách chặn `Http Request` & `Http Response`, chúng ta có thể làm được rất nhiều việc như:
<ol>
    <li>Gửi kèm thông tin Authorization vào header khi chúng ta đã đăng nhập</li>
    <li>Format data response từ API Server trả về</li>
    <li>Xử lý các lỗi từ API Server trả về</li>
</ol>  
<img src="https://res.cloudinary.com/tuananh-asia/image/upload/v1598028764/HTTP%20CLIENT%20MODULE/it-is-good_hixosk.gif" alt="It is good" class="img-responsive" />  
Ok, Nói tới đây thôi. Làm qua mấy ví dụ cho dễ hiểu nha  
Chúng ta sẽ làm 3 ví dụ trên luôn nhé.  

## 3. Send Http Request With Token using Interceptor
Trong ví dụ này, chúng ta đã đăng nhập và lưu Token của chúng ta ở một nơi nào đó. Như ví dụ ở đầu bài, chúng ta phải đi set lại `Header` trong mỗi function. Làm như vậy chúng ta sẽ rất mất công, gây khó hiểu và khó maintain. Chúng ta sẽ làm cho nó global để trong mỗi request đều chứa Token khi đã login
```typescript
// interceptor.ts
import { Injectable } from '@angular/core';
import { HttpInterceptor, HttpRequest, HttpHandler } from '@angular/common/http';

@Injectable()
export class TokenInterceptor implements HttpInterceptor {
  intercept(request: HttpRequest<any>, next: HttpHandler) {
    const token = localStorage.getItem('AUTH');
    if (token) {
      request = request.clone({
        headers: request.headers.set('Authorization', token)
      });
    }
    return next.handle(request);
  }
}
```