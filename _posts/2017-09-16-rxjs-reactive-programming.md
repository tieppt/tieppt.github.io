---
id: 1002
title: 'Rxjs Và Reactive Programming'
date: 2017-09-16T10:00:00+00:00
author: Tiep Phan
layout: post
guid: http://www.tiepphan.com/?p=1002
permalink: /rxjs-reactive-programming/
description: 'Giới thiệu Rxjs, Reactive Programming và ứng dụng trong Angular'
image: /assets/uploads/2017/09/rxjs-reactive-programming.jpg
categories:
  - Javascript
  - Lập Trình Angular
  - Programming
  - Web
  - Rxjs
tags:
  - Angular
  - Angular Component
  - Lập Trình Angular 2+
  - Rxjs Angular
  - Observable
  - Reactive Programming
---

# Rxjs Và Reactive Programming
{:.no_toc}

Hẳn các bạn vẫn còn nhớ trong một số bài trước chúng ta có nói về **Observable** trong ứng dụng Angular, vậy **Observable** là gì, nó có quan hệ gì với Angular, làm thế nào để sử dụng **Observable** hiệu quả trong ứng dụng của bạn. Trong bài này chúng ta sẽ cùng tìm hiểu về **Observable**, về **Rxjs**, **Reactive Programming**.

* ToC
{:toc}
{:.tp__toc}

## 1. Giới thiệu
{:#post-intro}

Angular đi kèm với một dependency là Rxjs giúp cho nó trở nên **reactive**, một ứng dụng Angular là một **reactive system**.
Dễ thấy nhất ở đây chính là EventEmitter, hay Reactive Forms mà chúng ta đã tìm hiểu trong các bài học trước.

Vậy **Reactive Programming** (RP) là gì? Điều gì khiến nó trở thành một chủ đề hot như vậy...

Hiện tại, có cả tá định nghĩa về RP, nhưng mình thấy định nghĩa sau đây là bao quát tốt vấn đề:

**Reactive programming is programming with asynchronous data streams**

Vâng đúng vậy, đây là phương pháp lập trình xoay quanh **data streams** và nó deal với các vấn đề của **asynchronous**. Nhưng bạn đừng hiểu lầm, nó có thể deal với cả **synchronous** nữa.

Bạn có thể tưởng tượng data streams như hình sau, với `data` được gửi đến trong suốt dòng thời gian của một stream (over time), giống như một array có các phần tử được gửi đến lần lượt theo thời gian.

![data streams](/assets/uploads/2017/09/packages.gif){:class="img-responsive"}
{:class="text-center"}

Và chúng ta có thể coi mọi thứ là stream: single value, array, event, etc.

![everything-is-a-stream](/assets/uploads/2017/09/everything-is-a-stream.jpg){:class="img-responsive"}
{:class="text-center"}

Không những thế, khi thao tác với stream, chúng ta có thể có `value`, `error`, hay `complete` signals. Đây là điều mà các API trước đây của các hệ thống event trong Javascript còn thiếu, chúng có qua nhiều interface khác nhau cho các loại event khác nhau, Observable sinh ra để tổng quát hóa các interface đó lại. 

![figure-stream](/assets/uploads/2017/09/figure-stream.png){:class="img-responsive"}
{:class="text-center"}

Và Rxjs giúp chúng ta có được **reactive** trong lập trình ứng dụng Javascript:

> Rxjs is a library for composing asynchronous and event-based programs by using observable sequences.
> 
> Think of Rxjs as Lodash (ultility for array/object) for events/streams.
> 
> ReactiveX combines the Observer pattern with the Iterator pattern and functional programming with collections to fill the need for an ideal way of managing sequences of events.
> 

Các Concepts nền tảng của Rxjs bao gồm:

* Observable: đại diện cho khái niệm về một tập hợp có thể gọi đến (tập hợp các Observer) của các giá trị hoặc các sự kiện trong tương lai. Khi các giá trị hoặc sự kiện phát sinh trong tương lai, Observable sẽ điều phối nó đến các phần tử trong tập hợp.
* Observer: là một tập hợp các callbacks tương ứng cho việc lắng nghe các giá trị (`value`, `error`, hay `complete`) được gửi đến bởi Observable.
* Subscription: là kết quả có được sau khi thực hiện một Observable, nó thường dùng cho việc hủy việc tiếp tục xử lý.
* Operators: là các pure functions cho phép lập trình functional với Observable.
* Subject: để thực hiện việc gửi dữ liệu đến nhiều Observers (multicasting).
* Schedulers: một scheduler sẽ điều khiển khi nào một subscription bắt đầu thực thi, và khi nào sẽ gửi tín hiệu đi. (Trong bài này chúng ta sẽ không nói về phần này).
{:.tpc__list}

## 2. Array Trong Javascript
{:#array}

Trước khi bắt đầu với Observable, chúng ta sẽ ôn lại một số kiến thức về Array sẽ giúp ích trong việc tiếp cận Observable.

### 2.1 Array forEach
{:#array-foreach}

Array `forEach` là một trong các cách để ta có thể lặp qua lần lượt từng phần tử trong mảng.

```ts
const arr = [1, 2, 3, 4, 5];

arr.forEach((item, index) => {
  console.log(index + ' => ' + item);
});

```

Kết quả nhận được của chúng ta như sau:

```ts
"0 => 1"
"1 => 2"
"2 => 3"
"3 => 4"
"4 => 5"
```

Ngoại trừ một điểm chúng ta cần lưu ý khi các phần tử là kiểu reference thay vì kiểu primitive, thì `forEach` có thể khiến các phần tử của array ban đầu thay đổi giá trị.

```ts
const ref = [
  {
    value: 1
  }, {
    value: 2
  }, {
    value: 3
  }, {
    value: 4
  }, {
    value: 5
  }
];

ref.forEach((item, index) => {
  item.value++;
});

ref.forEach((item, index) => {
  console.log(index + ' => ' + item.value);
});
```

### 2.2 Array map
{:#array-map}

Array `map` cho phép chúng ta lặp qua tất cả các phần tử trong mảng, áp dụng một function nào đó lên các phần tử để biến đổi, sau đó trả về một mảng các giá trị sau khi thực hiện và giữ nguyên mảng cũ không bị ảnh hưởng.

```ts
const arr = [1, 2, 3, 4, 5];

const amp = arr.map((item, index) => {
  return item + 5 + index;
});

console.log(arr, amp);

// result

[1, 2, 3, 4, 5]
[6, 8, 10, 12, 14]
```

### 2.3 Array filter
{:#array-filter}

Array `filter` cho phép chúng ta lặp qua tất cả các phần tử trong mảng, áp dụng một function nào đó lên các phần tử để kiểm tra, sau đó trả về một mảng các giá trị sau khi thực hiện hàm kiểm tra mà thỏa mãn điều kiện (`return true`) và giữ nguyên mảng cũ không bị ảnh hưởng.

```ts
const arr = [1, 2, 3, 4, 5];

const amp = arr.filter((item, index) => {
  return (item + index) % 3 == 0;
});

console.log(arr, amp);

// result

[1, 2, 3, 4, 5]
[2, 5]
```

### 2.4 Flatten Array
{:#flatten-array}

Trong nhiều tình huống, chúng ta có các array, bên trong mỗi phần tử có thể là các array khác, lúc này chúng ta có nhiệm vụ làm giảm số chiều (flatten) đi chẳng hạn, chúng ta có thể có đoạn code xử lý sau trong Javascript.

```ts
Array.prototype.concatAll = function() {
  return [].concat.apply([], this);
};

const arr = [1, [2, 3], [4, 8, 0], [5]];

const flatten = arr.concatAll();

console.log(arr, flatten);

// result
[1, [2, 3], [4, 8, 0], [5]]
[1, 2, 3, 4, 8, 0, 5]

```

Như ở ví dụ trên, chúng ta flat mảng con 2 chiều thành 1 chiều, và chúng ta có thể flat nhiều lần để mỗi lần sẽ giảm đi 1 chiều.

Điều này các bạn sẽ hay gặp khi làm việc với Observable trả về Observable trong các phần tiếp theo.

## 3. Producer vs Consumer, Push vs Pull
{:#producer-consumer-push-pull}

> Pull and Push are two different protocols how a data Producer can communicate with a data Consumer.
> 

OK, chúng ta lại có một số khái niệm mới:

**Producer**: là nguồn sản sinh ra data.

**Consumer**: là nơi `chế biến` data sản sinh từ Producer.

**Pull systems**: Consumer sẽ quyết định khi nào lấy data từ Producer. Producer không quan tâm khi nào data sẽ được gửi đến cho Consumer.

Các `function` trong Javascript là một Pull system. Khi nào lời gọi hàm thì khi đó mới xử lý. Gọi `n` lần thì xử lý `n` lần.

Lưu ý: function chỉ trả về 1 giá trị sau khi lời gọi hàm được thực hiện. (một mảng cũng chỉ coi là 1 giá trị, vì nó được trả về 1 lần).

**Push systems**: Producer sẽ quyết định khi nào gửi dữ liệu cho Consumer. Consumer không quan tâm khi nào nhận được data.

Promise, DOM events là các Push systems. Chúng ta register các callbacks và khi event phát sinh, các callbacks sẽ được gọi với dữ liệu từ Producer truyền vào.

Chúng ta có một bảng so sánh như sau:

&nbsp; | **Producer** | **Consumer**
---    | ---      | ---
**Pull** | **Passive**: produces data when requested. | **Active**: decides when data is requested.
**Push** | **Active**: produces data at its own pace. | **Passive**: reacts to received data.
{:class="table table-striped table-hover"}

Ví dụ:

**Pull**

```ts
const arr = [1,2,3,4];
const iter = arr[Symbol.iterator]();

iter.next();

> {value: 1, done: false}

iter.next();

> {value: 2, done: false}

iter.next();

> {value: 3, done: false}

iter.next();

> {value: 4, done: false}

iter.next();

> {value: undefined, done: true}

```

**Push**

```ts
var button = document.querySelector('button');
button.addEventListener('click', () => console.log('Clicked!'));
```

## 4. Observable
{:#observable}

Vậy Observable là gì?

**Observable** chỉ là một function (class) mà nó có một số yêu cầu đặc biệt. Nó nhận đầu vào là một Observer và trả về một function để có thể thực hiện việc cancel quá trình xử lý. Thông thường (Rxjs 5) chúng ta đặt tên function đó là `unsubscribe`.

**Observer**: một object có chứa các phương thức `next`, `error` và `complete` để xử lý dữ liệu tương ứng với các `signals` được gửi từ Observable.

> Observables are functions that tie an observer to a producer. That’s it. They don’t necessarily set up the producer, they just set up an observer to listen to the producer, and generally return a teardown mechanism to remove that listener. The act of subscription is the act of “calling” the observable like a function, and passing it an observer.
> 

Nói theo cách lý thuyết hơn:

> Observables are lazy Push collections of multiple values.

Như vậy, chúng ta có thể thấy Observable là lazy computation, giống như function, nếu chúng ta tạo chúng ra mà không gọi, thì không có gì thực thi cả.

Tạm thời bỏ qua các chi tiết về hàm `create` dưới đây, chúng ta sẽ gặp lại nó khi tìm hiểu về **Operator**:

```ts
function getDetail() {
  console.log('tiepphan.com');
  return 100;
}

const observable = Rx.Observable.create(function (observer) {
  console.log('Rxjs và Reactive Programming');
  observer.next(1);
  observer.next(2);
  observer.next(3);
  setTimeout(() => {
    observer.next(4);
    observer.complete();
  }, 1000);
});

```


Đến đây, nếu chúng ta không gọi hàm `getDetail`, hoặc không `invoke` đến `observable` thì chẳng có gì xảy ra cả.

Để thực thi, chúng ta sẽ làm như sau:

```ts

const ret = getDetail();
console.log(ret);

// and

console.log('before subscribe');
observable.subscribe({
  next: val => console.log('next: ' + val),
  error: err => console.error('error: ' + err),
  complete: () => console.log('done'),
});
console.log('after subscribe');

```

Và sau đây là kết quả chúng ta nhận được:

```ts
"tiepphan.com"
100
"before subscribe"
"Rxjs và Reactive Programming"
"next: 1"
"next: 2"
"next: 3"
"after subscribe"
"next: 4"
"done"

```

Observable có thể deal với cả sync và async.

> Observables are able to deliver values either synchronously or asynchronously.
> 

## 5. Làm Quen Với Observable
{:#anatomy-observable}

Với Observable chúng ta sẽ quan tâm đến các thao tác như sau:

* **Creating** Observables
* **Subscribing** to Observables
* **Executing** the Observable
* **Disposing** Observables
{:.tpc__list}


### 5.1 Creating Observables
{:#creating-observables}

`Rx.Observable.create` là một operator, nó chỉ là một alias cho `Observable` constructor, chúng ta hoàn toàn có thể thay thế tương ứng bằng cách gọi constructor cũng cho kết quả tương tự.

Đầu vào của `constructor` yêu cầu một hàm gọi là `subscribe` mà hàm này có đầu vào là một `observer` object.

```ts
const observable = new Rx.Observable(function subscribe(observer) {
  const id = setInterval(() => {
    observer.next('Hello Rxjs');
  }, 1000);
});

// same

/*
const observable = Rx.Observable.create(function subscribe(observer) {
  const id = setInterval(() => {
    observer.next('Hello Rxjs');
  }, 1000);
});

*/

```

Bạn hoàn toàn có thể sử dụng `new` hoặc `Rx.Observable.create`.

Ngoài operator như `create`, Rxjs mang đến cho bạn nhiều lựa chọn khác nhau để tạo mới một Observable như các operators: `of`, `from`, `interval`, etc. Chúng được đặt trong nhóm **creation operators**.

Ví dụ, bạn muốn tạo Observable cho một mảng các giá trị, lúc này bạn không cần dùng `Rx.Observable.create` rồi lặp qua các phần tử, xong gọi `next` nữa. Rxjs có cách dùng khác, vì đây là một usecase rất hay dùng và `create` là một low-level API.

```ts
const arr = [1, 2, 3, 4];

const fromArrayObservable = Rx.Observable.from(arr);

```

### 5.2 Subscribing to Observables
{:#subscribing-observables}

Sau khi đã tạo xong một Observable, chúng ta cần `invoke` bằng cách `subscribe` vào như sau:

```ts
observable.subscribe(val => console.log(val));

```

> Subscribing to an Observable is like calling a function, providing callbacks where the data will be delivered to.
> 

Vậy nên chúng ta call một function `n` lần, chúng ta sẽ có `n` lần thực thi. Tương tự như thế, khi chúng ta `subscribe` vào một Observable `m` lần, thì có `m` lần thực thi, một lời gọi `subscribe` giống như một cách để Observable bắt đầu thực thi.

### 5.3 Executing Observables
{:#executing-observables}

Phần code khi chúng ta khởi tạo Observable `Rx.Observable.create(function subscribe(observer) {...})` chính là "Observable execution". Giống như khai báo một function, phần code này để thực hiện một số hành động, xử lý nào đó; chúng là `lazy computation` - chỉ thực thi khi Observer thực hiện subscribe.

Có ba kiểu giá trị mà một Observable Execution có thể gửi đi:

* "Next" notification: gửi đi một giá trị, có thể là bất kỳ kiểu dữ liệu nào như Number, a String, an Object, etc.
* "Error" notification: gửi đi một JavaScript Error hoặc exception.
* "Complete" notification: không gửi đi một giá trị nào, nhưng nó gửi đi một tín hiệu để báo rằng stream này đã completed, mục đích để Observer có thể thực hiện một hành động nào đó khi stream completed.
{:.tpc__list}

Next notifications thường được sử dụng rộng rãi, nó cực kỳ quan trọng, vì nó gửi đi dữ liệu cần thiết cho một Observer.

Error và Complete notifications có thể chỉ xảy ra duy nhất một lần trong một **Observable Execution**.
Lưu ý rằng, chỉ có 1 trong 2 loại tín hiệu trên được gửi đi, nếu đã complete thì không có error, nếu có error thì không có complete. (Chúng không thuộc về nhau :D). Và nếu đã gửi đi complete, hoặc error signal, thì sau đó không có dữ liệu nào được gửi đi nữa. Tức là stream đã close.

> In an Observable Execution, zero to infinite Next notifications may be delivered. If either an Error or Complete notification is delivered, then nothing else can be delivered afterwards.
> 

Ví dụ:

```ts
const observable = Rx.Observable.create((observer) => {
  observer.next(1);
  observer.next(2);
  observer.next(3);
  observer.complete();
  observer.next(4); // Is not delivered because it would violate the contract
});

```
Khi Subscribe vào observable được tạo ở trên, bạn có thể thấy được kết quả như sau:

```ts
observable.subscribe(
  val => console.log(val),
  err => console.log(err),
  () => console.log('done')
);

// result
1
2
3
"done"

```

Lưu ý rằng trong ví dụ trên, tôi đã `invoke` Observable bằng cách `subscribe` với 3 callbacks riêng biệt cho 3 loại signals tương ứng, về mặt xử lý phía sâu bên trong sẽ convert về Observer object có dạng.

```ts

const observer = {
  next: val => console.log(val),
  error: err => console.log(err),
  complete: () => console.log('done')
}

// sử dụng để subscribe

observable.subscribe(observer);

```

### 5.4 Disposing Observable Executions
{:#disposing-observables}

Bởi vì quá trình thực thi Observable có thể lặp vô hạn, hoặc trong trường hợp nào đó bạn muốn thực hiện hủy việc thực thi vì việc này không còn cần thiết nữa - dữ liệu đã lỗi thời, có dữ liệu khác thay thế. Các bạn có thể liên tưởng tới việc close websocket stream, removeEvenListener cho một element nào đó đã bị loại bỏ khỏi DOM chẳng hạn.

Observable có cơ chế tương ứng, cho phép chúng ta hủy việc thực thi. Đó là khi `subscribe` được gọi, một Observer sẽ bị gắn với một Observable execution mới được tạo, sau đó nó sẽ trả về một object thuộc type Subscription. Kiểu dữ liệu này có một method `unsubscribe` khi chúng ta gọi đến, nó sẽ thực hiện cơ chế để hủy việc thực thi.

Lưu ý: nếu bạn tạo Observable bằng `create` hoặc `new` thì bạn phải tự thiết lập cơ chế để hủy.

> When you subscribe, you get back a Subscription, which represents the ongoing execution. Just call unsubscribe() to cancel the execution.

Ví dụ:

```ts
const observable = Rx.Observable.create(function subscribe(observer) {
  
  let value = 0;
  // Keep track of the interval resource
  const intervalID = setInterval(() => {
    observer.next(value++);
  }, 1000);

  // Provide a way of canceling and disposing the interval resource
  return function unsubscribe() {
    clearInterval(intervalID);
  };
});

const subscription = observable.subscribe(x => console.log(x));

setTimeout(() => {
  subscription.unsubscribe();
}, 5000);
```

## 6. Observer
{:#rxjs-observer}

Observer là một Consumer những dữ liệu được gửi bởi Observable. Observer là một object chứa một tập 3 callbacks tương ứng cho mỗi loại notification được gửi từ Observable: `next`, `error`, `complete`.

Một Observer có dạng như sau:

```ts
const observer = {
  next: x => console.log('Observer got a next value: ' + x),
  error: err => console.error('Observer got an error: ' + err),
  complete: () => console.log('Observer got a complete notification'),
};

```

Observer được cung cấp là tham số đầu vào của `subscribe` để kích hoạt Observable execution.

```ts
observable.subscribe(observer);

```

> Observers are just objects with three callbacks, one for each type of notification that an Observable may deliver.

Observe có thể chỉ có một số callbacks trong bộ 3 callbacks kể trên (có thể là một object không có callback nào trong bộ kể trên, trường hợp này ít dùng đến).

Như tôi đã đề cập từ trước, `observable.subscribe` sẽ chuẩn hóa các callbacks thành Observer object tương ứng, **bạn có thể truyền vào các hàm rời rạc nhau, nhưng cần lưu ý truyền đúng thứ tự callback**.

```ts
observable.subscribe(
  x => console.log('Observer got a next value: ' + x),
  err => console.error('Observer got an error: ' + err),
  () => console.log('Observer got a complete notification')
);

// tương đương với

const observer = {
  next: x => console.log('Observer got a next value: ' + x),
  error: err => console.error('Observer got an error: ' + err),
  complete: () => console.log('Observer got a complete notification'),
};

observable.subscribe(observer);

```

Lưu ý: Nếu bạn không muốn truyền error handler function vào, hãy truyền `null`/`undefined`:

```ts
observable.subscribe(
  x => console.log('Observer got a next value: ' + x),
  null,
  () => console.log('Observer got a complete notification')
);

```

## 7. Subscription
{:#rxjs-subscription}

