---
id: 2023
title: 'Angular HTTP Client Module - P2. Http Options và Demo Upload Progress'
date: 2020-06-23T00:00:00+00:00
author: Tuan Anh
layout: post
guid: https://www.tiepphan.com/?p=2023
permalink: /angular-http-client-module-option/
description: 'Cùng nhau khám phá về Http Client Options và demo upload file có progress nhé.'
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

Trong bài trước, mình đã giới thiệu sơ qua về `HttpClientModule` và những ứng dụng thiết yếu của nó.  
Học thì phải đi đôi với hành phải không nào? Hôm nay mình sẽ tiếp tục đi tiếp với Http Options và Demo 1 tính năng đó là `UPLOAD FILE WITH PROGRESS` nhé.  
Các bạn vừa đọc vừa mở IDE lên code cùng mình nhé.:)  

Nào! bắt đầu thôi  

* ToC
{:toc}
{:.tp__toc}

## 1. Http Options
{:#post-intro}
Trong bài trước, mình có viết mẫu một `Post Service` để handle những việc nhận - gửi request liên quan tới `Post`
```typescript
@Injectable({
    providedIn: 'root'
})
export class PostService {
    constructor(private httpClient: HttpClient) { }

    getListPosts(): Observable<PostEntityModel[]> {
        return this.httpClient.get<PostEntityModel[]>('https://jsonplaceholder.typicode.com/posts');
    }

    createPost(post: PostEntityModel): Observable<PostEntityModel> {
        return this.httpClient.post<PostEntityModel>('https://jsonplaceholder.typicode.com/posts', post);
    }

    updatePost(postId: number, post: PostEntityModel): Observable<PostEntityModel> {
        return this.httpClient.put<PostEntityModel>(`https://jsonplaceholder.typicode.com/posts/${ postId }`, post);
    }

    updateOptionPost(postId: number, post: Partial<PostEntityModel>): Observable<PostEntityModel> {
        return this.httpClient.patch<PostEntityModel>(`https://jsonplaceholder.typicode.com/posts/${ postId }`, post);
    }

    deletePost(postId: number): Observable<any> {
        return this.httpClient.delete(`https://jsonplaceholder.typicode.com/posts/${ postId }`);
    }
}
```  
Trong các method như get, post, put, có params là các option khi thực hiện 1 request http. Nó như sau:  
```typescript
options: {
    headers?: HttpHeaders | {[header: string]: string | string[]},
    observe?: 'body' | 'events' | 'response',
    params?: HttpParams|{[param: string]: string | string[]},
    reportProgress?: boolean,
    responseType?: 'arraybuffer'|'blob'|'json'|'text',
    withCredentials?: boolean,
}
```  
Chúng ta cùng tìm hiểu và sử dụng các option này nhé.  
  
### 1.1 Headers
`headers` trong `http options` là header configuration cho 1 `Http Request`.  Nó có thể là 1 `HttpHeaders` hoặc 1 object có `key` là `string` và giá trị là 1 `string` hoặc 1 `mảng string`.  
ví dụ hàm `GET`:  
```typescript
getListPosts(): Observable<PostEntityModel[]> {
    return this.httpClient.get<PostEntityModel[]>(
        'https://jsonplaceholder.typicode.com/posts',
        { headers: { angularVN: 'Angular Việt Nam' }}
    );
}
```  
Hoặc sử dụng `HttpHeaders`:  
```typescript
getListPosts(): Observable<PostEntityModel[]> {
    return this.httpClient.get<PostEntityModel[]>(
        'https://jsonplaceholder.typicode.com/posts',
        { headers: new HttpHeaders({ angularVN: 'Angular Viet Nam' })}
    );
}
```  
Kết quả:  
<img src="https://res.cloudinary.com/tuananh-asia/image/upload/v1592901983/HTTP%20CLIENT%20MODULE/request-header-when-reassign-header-instance_fb5qq7.png" alt="Header are shown" />  

Các bạn nên nhớ. Instance của một `HttpHeaders` là immutable nhé. Nghĩa là mỗi lần modify là trả về 1 `cloned instance` kèm thèm giá trị vừa modify nhé.  
Ví dụ:  
```typescript
getListPosts(): Observable<PostEntityModel[]> {
    const headers: HttpHeaders = new HttpHeaders();
    headers.set('angularVN', 'Angular Viet Nam');
    return this.httpClient.get<PostEntityModel[]>(
        'https://jsonplaceholder.typicode.com/posts',
        { headers }
    );
}
```
Kết quả:
<img src="https://res.cloudinary.com/tuananh-asia/image/upload/v1592901768/HTTP%20CLIENT%20MODULE/request-header-is-not-append-key_dhydwg.png" alt="Not working" />  

Ở đây `Request Header` không có key `angularVN`. Như đã nói ở trên. `mỗi lần modify là trả về 1 cloned instance kèm thèm giá trị vừa modify`. Giải quyết bằng cách:  
```typescript
getListPosts(): Observable<PostEntityModel[]> {
    let headers: HttpHeaders = new HttpHeaders();
    headers = headers.set('angularVN', 'Angular Viet Nam'); // --> gán lại cho biến headers
    return this.httpClient.get<PostEntityModel[]>(
        'https://jsonplaceholder.typicode.com/posts',
        { headers}
    );
}
```
Và kết quả là:  
<img src="https://res.cloudinary.com/tuananh-asia/image/upload/v1592901983/HTTP%20CLIENT%20MODULE/request-header-when-reassign-header-instance_fb5qq7.png" alt="Header are shown" />  
  
### 1.2 Observe
`observe` là một giá trị kiểu string thuộc enum `'body' | 'events' | 'response'`. Nó báo cho `HttpClient` biết mình muốn lấy response ở mức độ nào. Mặc định nếu không set trong options thì sẽ lấy observe là `'body'`.  
Chúng ta cùng điểm qua các `observe` value nhé.  
#### 1.2.1 Observe 'body'
Mặc định khi gửi 1 `Http Request` và nhận lại một response. Chúng ta sẽ nhận được data là body của `HttpResponse`
```typescript
getListPosts(): Observable<PostEntityModel[]> {
    return this.httpClient.get<PostEntityModel[]>(
        'https://jsonplaceholder.typicode.com/posts',
        { observe: 'body' });
}
```  
#### 1.2.2 Observe 'response'
Khi xử lý 1 `Http Request`, bạn có thể cần nhiều thông tin hơn về response như `status`, `statusText`, `headers`. Chúng ta sử dụng option `observe: 'response'` để hiển thị thêm thông tin của `Http Response` đó.  
```typescript
getListPosts(): Observable<HttpResponse<PostEntityModel[]>> {
    return this.httpClient.get<PostEntityModel[]>(
        'https://jsonplaceholder.typicode.com/posts',
        { observe: 'response' });
}
```  
Kết quả:  
<img src="https://res.cloudinary.com/tuananh-asia/image/upload/v1592907735/HTTP%20CLIENT%20MODULE/observe-response-result_fgwn81.png" alt="Observe Response" />  
Chúng ta đã có thêm thông tin để xử lý tiếp rồi:)

#### 1.2.3 Observe 'events'
Khi sử dụng `observe: 'events'`, chúng ta sẽ nhận được full event của 1 `Http Request`. Thực tế là nó sẽ đi từ bắt đầu gọi request tới lúc request response hoàn chỉnh
Hãy để ý nhé:  
```typescript
createPost(post: Partial<PostEntityModel>): Observable<HttpEvent<PostEntityModel>> {
    return this.httpClient.post<PostEntityModel>(
        'https://jsonplaceholder.typicode.com/posts',
        post,
        { observe: 'events' }
    );
}
```  
<img src="https://res.cloudinary.com/tuananh-asia/image/upload/v1592910602/HTTP%20CLIENT%20MODULE/http-obser-event-angular_zaspdq.png" alt="Observer events" />
Các bạn thấy đấy, từ `{type: 0}` cho đến `{type: 4}` là lúc request hoàn thành rồi. Lát nữa chúng ta sẽ demo có sự góp mặt của thèn này


