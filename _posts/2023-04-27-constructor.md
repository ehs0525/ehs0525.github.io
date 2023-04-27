---
layout: single
title: "[JavaScript] 생성자"
categories: javascript
tag: [constructor, 생성자]
author_profile: false
search: true
use_math: false
sidebar:
  nav: "counts"
---

# 생성자

**생성자란 앞에 new 연산자가 붙은 함수를 의미하며 인스턴스를 만들 수 있습니다.**

예를 들어 new Object(), new Array()는 자바스크립트의 내부적으로 존재하는 내장 생성자입니다.

```javascript
let arr = new Array(1, 2, 3);

console.log(arr);	// [1, 2, 3]
```

**이와 마찬가지로 사용자가 직접 새로운 타입을 만들수도 있습니다.**

또한 생성자와 인스턴스의 관계는 `instanceof`와 `constructor` 메소드를 통해 확인할 수 있습니다.

```javascript
function MyOwn() {
    
}

let myObj = new MyOwn();

console.log(myObj instanceof MyOwn);	// true
console.log(myObj.constructor === MyOwn);	// true
```

## 생성자는 왜 만드나요?

생성자의 중요한 기능은 바로 동일한 프로퍼티와 메소드를 가진 객체를 쉽게 대량생산 하는데 있습니다.

```javascript
function Food(taste) {
    this.taste = taste;
    this.smell = function() {
        console.log(this.taste + "냄새가 난다.");
    }
}

let myFood1 = new Food("특제 파스타");
let myFood2 = new Food("해물 파스타");
let myFood3 = new Food("토마토 파스타");

myFood1.smell();	// "특제 파스타 냄새가 난다."
myFood2.smell();	// "해물 파스타 냄새가 난다."
myFood3.smell();	// "토마토 파스타 냄새가 난다."
```

## 생성자의 new 연산자는 매우 중요합니다!

new 연산자가 붙으면 함수의 this는 인스턴스를 참조하게 되며, new 연산자가 자동으로 인스턴스를 반환하기 때문에 함수 안에 return 연산자도 필요없어지게 됩니다.

```javascript
function Food(taste) {
    
    console.log(this);
    console.log(this.constructor);
    
    this.taste = taste;
    this.smell = function() {
        console.log(this.taste + "냄새가 난다.");
    }
}

let myFood1 = new Food("특제 파스타");	// Food {}, function Food(taste) {}
let myFood2 = Food("특제 파스타");	// Window {}, function Window() {}
```

만약 생성자 함수에 new 연산자가 없다면 생성자 함수는 단순히 평범한 함수일 뿐이며, this는 전역 객체를 가르키게 됩니다.
