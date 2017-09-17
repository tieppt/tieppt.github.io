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

### 2.4 Array reduce
{:#array-reduce}

Method `reduce` cho phép chúng ta lặp qua tất cả các phần tử và áp dụng một function nào đó vào mỗi phần tử, function này có các tham số:

* `accumulator`: giá trị trả về từ các lần call callback trước.
* `currentValue`: giá trị của phần tử hiện tại trong array.
* `currentIndex`: index của phần tử hiện tại.
* `array`: chính là mảng hiện tại.

Ngoài ra, chúng ta còn có thể cung cấp giá trị ban đầu `initialValue` sau tham số function đầu tiên.

```ts
const arr = [1, 2, 3, 4, 5];

const val = arr.reduce((acc, current) => acc * current, 1);

console.log(val);

// result
120

```

### 2.5 Flatten Array
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

Subscription là một object đại diện cho một nguồn tài nguyên có khả năng hủy được, thông thường trong Rxjs là hủy Observable execution. Subscription có chứa một method quan trọng `unsubscribe` (từ Rxjs 5), khi method này được gọi, execution sẽ bị hủy.

Ví dụ: chúng ta có một đồng hồ đếm thời gian, mỗi giây sẽ gửi đi một giá trị, giả sử sau khi chạy 5s chúng ta cần hủy phần thực thi này.

```ts
const observable = Rx.Observable.interval(1000);
const subscription = observable.subscribe(x => console.log(x));

setTimeout(() => {
  subscription.unsubscribe();
}, 5000);

```

> A Subscription essentially just has an unsubscribe() function to release resources or cancel Observable executions.

Một Subscription có thể chứa trong nó nhiều Subscriptions con, khi Subscription `unsubscribe`, các Subscriptions con cũng sẽ `unsubscribe`.

Ở Subscription cha, chúng ta có thể gọi method `add` để thêm các Subscriptions con mà phụ thuộc Subscription cha này.

```ts
const foo = Rx.Observable.interval(500);
const bar = Rx.Observable.interval(700);

const subscription = foo.subscribe(x => console.log('first: ' + x));
const childSub = bar.subscribe(x => console.log('second: ' + x));

subscription.add(childSub);

setTimeout(() => {
  // Unsubscribes BOTH subscription and childSub
  subscription.unsubscribe();
}, 2000);


```

## 8. Cold Observables vs Hot Observables
{:#rxjs-cold-hot-observables}

> Cold Observables: Producers created **inside**

Một observable là “cold” nếu Producer được tạo ra và quản lý bên trong nó.

Ví dụ:

```ts

const observable = Rx.Observable.create(observer => {
  let x = 5;
  observer.next(x);
  x += 10;
  setTimeout(() => {
	observer.next(x);
	observer.complete();
  }, 1000);
});

const observer = {
  next: value => console.log(value),
  complete: () => console.log('done')
};

observable.subscribe(observer);

setTimeout(() => {
  observable.subscribe(observer);
}, 1000);

```

Kết quả là sau 1s thì lần `subscribe` đầu tiên có in ra 15, và lần `subscribe` thứ 2 in ra 5. Chúng không có cùng giá trị của `x`.

Giờ thay đổi chút bằng việc move khai báo biến `x` ra ngoài `create`:

```ts

let x = 5;

const observable = Rx.Observable.create(observer => {
  observer.next(x);
  x += 10;
  setTimeout(() => {
	observer.next(x);
	observer.complete();
  }, 1000);
});

const observer = {
  next: value => console.log(value),
  complete: () => console.log('done')
};

observable.subscribe(observer);

setTimeout(() => {
  observable.subscribe(observer);
}, 1000);

```

Lần này, sau 1s thì cả 2 execution đều in ra giá trị là 15.

Đây chính là ví dụ về Hot Observables.

> Hot Observables: Producers created **outside**
> 


## 9. Subject
{:#rxjs-subject}

Giả sử chúng ta có đoạn code sau đây:

Ở đây mình mong muốn sau khi observerBaz chạy 1.5s thì observerBar sẽ nhận được giá trị hiện tại mà observerBaz đang nhận.

```ts
const foo = Rx.Observable.interval(500).take(5);

const observerBaz = {
  next: x => console.log('first next: ' + x),
  error: err => console.log('first error: ' + err),
  complete: _ => console.log('first done')
};

const observerBar = {
  next: x => console.log('second next: ' + x),
  error: err => console.log('second error: ' + err),
  complete: _ => console.log('second done')
};

foo.subscribe(observerBaz);

setTimeout(() => {
  foo.subscribe(observerBar);
}, 1500);

```

Kết quả khi chạy chương trình:

```ts
"first next: 0"
"first next: 1"
"first next: 2"
"second next: 0"
"first next: 3"
"second next: 1"
"first next: 4"
"first done"
"second next: 2"
"second next: 3"
"second next: 4"
"second done"

```

Oh well, observerBaz và observerBar tạo ra các execution khác nhau của riêng chúng, chúng ta không kết nối chúng đến với nhau theo cách trên được.

Có cách nào để share execution giữa nhiều Observers không?

Lúc này, chúng ta cần đến một object làm cầu nối ở giữa để làm nhiệm vụ tạo ra Observable execution và share dữ liệu ra cho những Observers khác.

OK, tiến hành thêm một số code nữa.


```ts
const foo = Rx.Observable.interval(500).take(5);

const observerB = {
  observers: [],
  addObserver: function(observer) {
    this.observers.push(observer);
  },
  
  // this part is an Observer
  next: function(x) {
    this.observers.forEach(observer => observer.next(x));
  },
  error: function(err) {
    this.observers.forEach(observer => observer.error(err));
  },
  complete: function() {
    this.observers.forEach(observer => observer.complete());
  }
};

const observerBaz = {
  next: x => console.log('first next: ' + x),
  error: err => console.log('first error: ' + err),
  complete: _ => console.log('first done')
};

const observerBar = {
  next: x => console.log('second next: ' + x),
  error: err => console.log('second error: ' + err),
  complete: _ => console.log('second done')
};

observerB.addObserver(observerBaz);

// only subscribe bridge observer
foo.subscribe(observerB);

setTimeout(() => {
  observerB.addObserver(observerBar);
}, 1500);

```

Object `observerB` có chưa method `addObserver` đây chính là khuôn mẫu của "Observer-Pattern".

Giờ đây chúng ta có thể có được kết quả như mong muốn.

```ts
"first next: 0"
"first next: 1"
"first next: 2"
"second next: 2"
"first next: 3"
"second next: 3"
"first next: 4"
"second next: 4"
"first done"
"second done"

```

Bây giờ, nếu chúng ta thay đổi một chút:

```ts
const foo = Rx.Observable.interval(500).take(5);

const observerB = {
  observers: [],
  subscribe: function(observer) {
    this.observers.push(observer);
  },
  
  // this part is an Observer
  next: function(x) {
    this.observers.forEach(observer => observer.next(x));
  },
  error: function(err) {
    this.observers.forEach(observer => observer.error(err));
  },
  complete: function() {
    this.observers.forEach(observer => observer.complete());
  }
};

const observerBaz = {
  next: x => console.log('first next: ' + x),
  error: err => console.log('first error: ' + err),
  complete: _ => console.log('first done')
};

const observerBar = {
  next: x => console.log('second next: ' + x),
  error: err => console.log('second error: ' + err),
  complete: _ => console.log('second done')
};

observerB.subscribe(observerBaz);

// only subscribe bridge observer
foo.subscribe(observerB);

setTimeout(() => {
  observerB.subscribe(observerBar);
}, 1500);

```

Giờ bạn có thể thấy, `observerB` trông giống như một Observable, nó lại còn giống như Observer, hơn nữa nó có thể gửi signals cho nhiều Observers mà nó đang quản lý. Đây chính là cấu trúc hybrid của Subject.

> A Subject is like an Observable, but can multicast to many Observers. Subjects are like EventEmitters: they maintain a registry of many listeners.
> 

**Mỗi Subject là một Observable**: bạn có thể `subscribe` vào nó, cung cấp cho nó một Observer và bạn có thể nhận data tương ứng.

**Mỗi Subject là một Observer**: bên trong Subject có chứa các method `next`, `error`, `complete` tương ứng để bạn có thể `subscribe` vào Observable chẳng hạn.
Khi cần gửi dữ liệu cho các Observers mà Subject đang quản lý, bạn chỉ cần gọi hàm tương ứng.

Ví dụ:

```ts
const subject = new Rx.Subject();

subject.subscribe({
  next: (v) => console.log('observerA: ' + v)
});
subject.subscribe({
  next: (v) => console.log('observerB: ' + v)
});

subject.next('Hello');
subject.next('Subject');

// result
"observerA: Hello"
"observerB: Hello"
"observerA: Subject"
"observerB: Subject"

```

Hoặc truyền vào một Observable:

```ts
const subject = new Rx.Subject();

subject.subscribe({
  next: (v) => console.log('observerA: ' + v)
});
subject.subscribe({
  next: (v) => console.log('observerB: ' + v)
});

// subject.next('Hello');
// subject.next('Subject');

const observable = Rx.Observable.interval(500).take(3);

observable.subscribe(subject);

// result
"observerA: 0"
"observerB: 0"
"observerA: 1"
"observerB: 1"
"observerA: 2"
"observerB: 2"

```

Với phương pháp kể trên, chúng ta đã cơ bản chuyển đổi từ một unicast Observable execution sang multicast, bằng cách sử dụng Subject.

**unicast**: giống như bạn vào Youtube, mở video nào đó đã được thu sẵn và xem, nó play từ đầu đến cuối video. Một người khác vào xem, Youtube cũng sẽ phát từ đầu đến cuối như thế, hai người không có liên quan gì về thời gian hiện tại của video mà mình đang xem.

**multicast**: cũng là hai người (có thể nhiều hơn) vào xem video ở Youtube, nhưng video đó đang phát Live (theo dõi một show truyền hình, hay một trận bóng đá Live chẳng hạn). Lúc này Youtube sẽ phát video Live, và những người vào xem video đó sẽ có cùng một thời điểm của video đó (cùng thời điểm của trận đấu đang diễn ra chẳng hạn).

### 9.1 BehaviorSubject
{:#rxjs-BehaviorSubject}

Một trong các biến thể của Subject đó là BehaviorSubject, nó là biến thế có khái niệm về "the current value". BehaviorSubject lưu trữ lại giá trị mới emit gần nhất để khi một Observer mới subscribe vào, nó sẽ emit giá trị đó ngay lập tức cho Observer vừa rồi.

> BehaviorSubjects are useful for representing "values over time". For instance, an event stream of birthdays is a Subject, but the stream of a person's age would be a BehaviorSubject.

Hay như sử dụng BehaviorSubject để chia sẻ thông tin user hiện tại đang đăng nhập hệ thống cho các component khác nhau trong Angular chẳng hạn.

Lưu ý: BehaviorSubject yêu cầu phải có giá trị khởi tạo khi tạo ra subject.


```ts

const subject = new Rx.BehaviorSubject(0); // 0 is the initial value

subject.subscribe({
  next: (v) => console.log('observerA: ' + v)
});

subject.next(1);
subject.next(2);

subject.subscribe({
  next: (v) => console.log('observerB: ' + v)
});

subject.next(3);

//result
observerA: 0
observerA: 1
observerA: 2
observerB: 2
observerA: 3
observerB: 3

```

### 9.2 ReplaySubject
{:#rxjs-ReplaySubject}

Một ReplaySubject tương tự như một BehaviorSubject khi nó có thể gửi những dữ liệu trước đó cho Observer mới subscribe, nhưng nó có thể lưu giữ nhiều giá trị (có thể là toàn bộ giá trị của stream từ thời điểm ban đầu).

Tham số đầu vào của ReplaySubject có thể là:

buffer: là số lượng phần tử tối đa có thể lưu trữ.

windowTime: (ms) thời gian tối đa tính đến thời điểm gần nhất emit value.


```ts
const subject = new Rx.ReplaySubject(3); // buffer 3 values for new subscribers

subject.subscribe({
  next: (v) => console.log('observerA: ' + v)
});

subject.next(1);
subject.next(2);
subject.next(3);
subject.next(4);

subject.subscribe({
  next: (v) => console.log('observerB: ' + v)
});

subject.next(5);

// result
"observerA: 1"
"observerA: 2"
"observerA: 3"
"observerA: 4"
"observerB: 2"
"observerB: 3"
"observerB: 4"
"observerA: 5"
"observerB: 5"

```

Hoặc kết hợp buffer với windowTime:

```ts
const subject = new Rx.ReplaySubject(100, 500 /* windowTime */);

subject.subscribe({
  next: (v) => console.log('observerA: ' + v)
});

let i = 1;
const id = setInterval(() => subject.next(i++), 200);

setTimeout(() => {
  subject.subscribe({
    next: (v) => console.log('observerB: ' + v)
  });
}, 1000);

setTimeout(() => {
  subject.complete();
  clearInterval(id);
}, 2000);

// result
"observerA: 1"
"observerA: 2"
"observerA: 3"
"observerA: 4"
"observerA: 5"
"observerB: 3"
"observerB: 4"
"observerB: 5"
"observerA: 6"
"observerB: 6"
...

```

Trong ví dụ trên sau 1s chỉ có giá trị 3, 4 và 5 là được emit trong 500ms gần nhất và nằm trong buffer nên được replay lại cho `observerB`.

### 9.3 AsyncSubject
{:#rxjs-AsyncSubject}

Đây là biến thể mà chỉ emit giá trị cuối cùng của Observable execution cho các observers, và chỉ khi execution complete.

Lưu ý:

Nếu stream không complete thì không có gì được emit cả.


```ts
const subject = new Rx.AsyncSubject();

subject.subscribe({
  next: (v) => console.log('observerA: ' + v)
});

subject.next(1);
subject.next(2);
subject.next(3);
subject.next(4);

subject.subscribe({
  next: (v) => console.log('observerB: ' + v)
});

subject.next(5);
subject.complete();

// result
"observerA: 5"
"observerB: 5"

```

### 9.4 Subject Complete
{:#rxjs-SubjectComplete}

Khi BehaviorSubject complete, thì các Observers subscribe vào sau đó sẽ chỉ nhận được `complete` signal.

Khi ReplaySubject complete, thì các Observers subscribe vào sau đó sẽ được emit tất cả các giá trị lưu trữ trong buffer, sau đó mới thực thi `complete` của Observer.

Kể cả khi AsyncSubject complete rồi, Observer vẫn có thể subscribe vào được và vẫn nhận giá trị cuối cùng.

```ts
const subject = new Rx.BehaviorSubject(0); // 0 is the initial value

subject.subscribe({
  next: (v) => console.log('observerA: ' + v),
  complete: () => console.log('observerA: done')
});

subject.next(1);
subject.next(2);

subject.subscribe({
  next: (v) => console.log('observerB: ' + v),
  complete: () => console.log('observerB: done')
});

subject.next(3);

subject.complete();

subject.subscribe({
  next: (v) => console.log('observerC: ' + v),
  complete: () => console.log('observerC: done')
});

// result
"observerA: 0"
"observerA: 1"
"observerA: 2"
"observerB: 2"
"observerA: 3"
"observerB: 3"
"observerA: done"
"observerB: done"
"observerC: done"
// ===========================================

const subject = new Rx.ReplaySubject(3);

subject.subscribe({
  next: (v) => console.log('observerA: ' + v),
  complete: () => console.log('observerA: done')
});

let i = 1;
const id = setInterval(() => subject.next(i++), 200);

setTimeout(() => {
  subject.complete();
  clearInterval(id);
  subject.subscribe({
    next: (v) => console.log('observerB: ' + v),
    complete: () => console.log('observerB: done')
  });
}, 1000);

// result
"observerA: 1"
"observerA: 2"
"observerA: 3"
"observerA: 4"
"observerA: 5"
"observerA: done"
"observerB: 3"
"observerB: 4"
"observerB: 5"
"observerB: done"

// ===========================================

const subject = new Rx.AsyncSubject();

subject.subscribe({
  next: (v) => console.log('observerA: ' + v),
  complete: () => console.log('observerA: done')
});

subject.next(1);
subject.next(2);
subject.next(3);
subject.next(4);
subject.next(5);

subject.complete();

subject.subscribe({
  next: (v) => console.log('observerB: ' + v),
  complete: () => console.log('observerB: done')
});

// result
"observerA: 5"
"observerA: done"
"observerB: 5"
"observerB: done"

```

## 10. Operators
{:#rxjs-Operators}

Trong Rxjs chúng ta thấy có vô số các method/static method được cung cấp, khiến nó trở nên vô cùng mạnh mẽ và hữu dụng.

Vậy Operator là gì?

> An Operator is a function which creates a new Observable based on the current Observable. This is a pure operation: the previous Observable stays unmodified.

Operator là một pure function. Với cùng một giá trị đầu vào, chúng ta sẽ luôn có cùng một giá trị ở đầu ra.

Ví dụ:

```ts
// pure function
function add(a, b) {
  return a + b;
}

add(5, 6); // 11
add(5, 6); // 11

// impure function

let x = 0;
function addImpure(a, b) {
  x++;
  return a + b + x;
}

add(5, 6); // 12
add(5, 6); // 13

```

Operation nhận đầu vào là một Observable, sau đó xử lý và tạo mới mội Observable để trả về, và giữ Observable đầu vào không bị thay đổi gì.

Ví dụ, chúng ta tạo một Operator nhận đầu vào là một function, giống như Array `map` chẳng hạn.

```ts
function map(observable, fn) {
  const output = Rx.Observable.create(observer => {
    observable.subscribe({
      next: x => observer.next(fn(x)),
      error: err => observer.error(err),
      complete: () => observer.complete()
    });
  });
  
  return output;
}
```

Bây giờ chúng ta có thể tạo một Observable như sau:

```ts
const foo = Rx.Observable.interval(500).take(5);

const amap = map(foo, val => val * 5);

const observerM = {
  next: x => console.log('next: ' + x),
  error: err => console.log('error: ' + err),
  complete: _ => console.log('done')
};

amap.subscribe(observerM);

// result
"next: 0"
"next: 5"
"next: 10"
"next: 15"
"next: 20"
"done"

```

### 9.1 Instance operators vs static operators
{:#InstancevsStaticOperators}

Thông thường **static operators** là các operators được gắn với `class` Observable, chúng được dùng phổ biến để tạo mới Observable.

Ví dụ như các operator `of`, `from`, `interval`, `fromPromise`, `empty`, etc.

**Instance operators**: các operators này gắn với một instance của `class` Observable.

Giả sử chúng ta tạo operator `mmap` như sau, thì kết quả cũng được tương tự như trên:

```ts
Rx.Observable.prototype.mmap = function map(fn) {
  const observable = this;
  const output = Rx.Observable.create(observer => {
    observable.subscribe({
      next: x => observer.next(fn(x)),
      error: err => observer.error(err),
      complete: () => observer.complete()
    });
  });
  
  return output;
}

// invoke

const amap = foo.mmap(val => val * 5);

const observerM = {
  next: x => console.log('next: ' + x),
  error: err => console.log('error: ' + err),
  complete: _ => console.log('done')
};

amap.subscribe(observerM);

```

> Instance operators are functions that use the this keyword to infer what is the input Observable.
> 
> Static operators are pure functions attached to the Observable class, and usually are used to create Observables from scratch.

Operators được chia vào nhiều nhóm khác nhau, mỗi nhóm có một mục đích sử dụng:

* Creation Operators.
* Transformation Operators.
* Filtering Operators.
* Combination Operators.
* Error Handling Operators.
* Utility Operators.
* Multicasting Operators.
* Conditional and Boolean Operators.
* Mathematical and Aggregate Operators
{:.tpc__list}

Sau đây chúng ta sẽ tìm hiểu một số operators chính hay dùng đến.

### 9.2 Creation Operators
{:#rxjs-CreationOperators}

Từ đầu đến giờ chúng ta đã quen với sử dụng `create` operator để tạo ra Observable, ngoài ra chúng ta còn có một số operators khác để tạo Observable từ những common usecase.

Ví dụ để tạo Observable từ một số static values, chúng ta có thể dùng `of`:

```ts
const foo = Rx.Observable.of(100);

foo.subscribe(x => console.log(x)); // 100

```

Hoặc có thể dùng `of` với nhiều tham số đầu vào:

```ts
const foo = Rx.Observable.of(100, 200, 400);

```

Lưu ý: với các static values như ví dụ trên, chúng được gửi đi sync. (mặc đinh nếu không có tác động của Scheduler).

Sau khi gửi đi hết dữ liệu, nó sẽ gửi `complete` signal.

Một số operators tương tự:

* interval: thay vì tự tạo interval với setInterval, chúng ta dùng operator này cho mục đích tương tự.
* timer: giống như setTimeout, sau một khoảng thời gian nào đó thì emit value.
* throw: để bắn ra error.
* empty: không gửi gì khác ngoài `complete` signal.
* never: không gửi bất kỳ loại signals nào.

**Tổng quát cho các loại event** thì chúng ta có `fromEventPattern`:

> Converts any addHandler/removeHandler API to an Observable.

Ví dụ:

```ts

function addClickHandler(handler) {
  document.addEventListener('click', handler);
}

function removeClickHandler(handler) {
  document.removeEventListener('click', handler);
}

var clicks = Rx.Observable.fromEventPattern(
  addClickHandler,
  removeClickHandler
);
clicks.subscribe(x => console.log(x));

```

**Để làm việc với DOM event** chúng ta có `fromEvent` operator, nó là bản chi tiết của `fromEventPattern`:

```ts
const source = Rx.Observable.fromEvent(document, 'click');

const foo = source.map(event => event.clientX)

const subscribe = foo.subscribe(val => console.log(val));

```

Khi click vào trang web, chúng ta có thể thấy console hiển thị tọa độ X ra.

**Để convert từ Promise sang Observable** chúng ta có `fromPromise` operator:

```ts

function getInfo() {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve({
        site: 'tiepphan.com',
        post: 'Rxjs và Reactive Programming'
      });
    }, 300);
  });
}

const source = Rx.Observable.fromPromise(getInfo());

const subscribe = source.subscribe(val => console.log(val));

// result after 300ms
[object Object] {
  post: "Rxjs và Reactive Programming",
  site: "tiepphan.com"
}

```

**Tạo mới một Observable từ Array, array-like object, Promise, iterable object, hoặc Observable-like object** chúng ta dùng `from` operator:

```ts
const array = [5, 10, 15];
const source = Rx.Observable.from(array);
source.subscribe(x => console.log(x));

// result
5
10
15

```

Còn nhiều các operators khác nữa, các bạn vào trang chủ của ReactiveX để xem thêm.

### 9.3 Transformation Operators
{:#rxjs-TransformationOperators}

Transformation Operators dùng để chuyển đổi giá trị của Observable từ dạng này sang sang dạng khác.

Hay gặp nhất có lẽ là `map`, nó tương tự như `mmap` operator mà chúng ta đã tạo ở trên (tạo để tìm hiểu cơ chế hoạt động).

```ts
//emit (1,2,3,4,5)
const source = Rx.Observable.from([1,2,3,4,5]);
//add 10 to each value
const example = source.map(val => val + 10);
//output: 11,12,13,14,15
const subscribe = example.subscribe(val => console.log(val));

```

Các bạn có thể dùng `map` để chuyển đổi theo bất kỳ yêu cầu nào của các bạn. Ví dụ khi làm việc với Response object của HTTP, lúc này các bạn có thể convert từ JSON thành object như sau:

```ts
this.http.get(API_ENDPOINT) // return Observable
  .map(res => res.json());

```

Khi bạn muốn luôn luôn trả về 1 giá trị cho các giá trị được emit, lúc này bạn có thể dùng `mapTo` operator.

```ts
//emit every click on document
const source = Rx.Observable.fromEvent(document, 'click');
//map all emissions to one value
const example = source.mapTo(10);
//output: (click) 10...10...
const subscribe = example.subscribe(val => console.log(val));

```

**scan**: `scan(accumulator: function, seed: any): Observable`

Giống như Array `scan`.

```ts
//emit array as a sequence of values
const arraySource = Rx.Observable.from([1,2,3,4,5]);

const source = arraySource.scan( (acc, current) => acc + current, 0);

const subscribe = source.subscribe(val => console.log(val));

// result
1
3
6
10
15

```

**buffer**: `buffer(closingNotifier: Observable): Observable`

Lưu trữ giá trị được emit ra và đợi đến khi `closingNotifier` emit thì emit những giá trị đó thành 1 array.

```ts
const interval$ = Rx.Observable.interval(1000);

const click$ = Rx.Observable.fromEvent(document, 'click');

const buffer$ = interval$.buffer(click$);


const subscribe = buffer$.subscribe(
  val => console.log('Buffered Values: ', val)
);

// output có dạng
"Buffered Values: "
[0, 1]
"Buffered Values: "
[2, 3, 4, 5, 6]

```

**bufferTime**: `bufferTime(bufferTimeSpan: number, bufferCreationInterval: number, scheduler: Scheduler): Observable`

Tương tự như `buffer`, nhưng emit values mỗi khoảng thời gian `bufferTimeSpan` ms.

```ts
const source = Rx.Observable.interval(500);

const bufferTime = source.bufferTime(2000);

const bufferTimeSub = bufferTime.subscribe(
  val => console.log('Buffered with Time:', val)
);
// output
"Buffered with Time:"
[0, 1]
"Buffered with Time:"
[2, 3]
"Buffered with Time:"
[4, 5]
...

```

Ngoài ra còn rất nhiều operator khác, và một số Operators chúng ta sẽ tìm hiểu ở phần sau "Higher Order Observables".

### 9.4 Filtering Operators
{:#rxjs-FilteringOperators}

Filtering Operators mục đích để filter các giá trị được emit thỏa mãn điều kiện nào đó.



