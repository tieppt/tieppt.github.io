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
{:#intro}

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

![figure-stream](/assets/uploads/2017/09/figure-stream.jpg){:class="img-responsive"}
{:class="text-center"}

Và Rxjs giúp chúng ta có được **reactive** trong lập trình ứng dụng Javascript:

> RxJS is a library for composing asynchronous and event-based programs by using observable sequences.
> 
> Think of RxJS as Lodash (ultility for array/object) for events/streams.
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

## 3. Observable
{:#observable}


