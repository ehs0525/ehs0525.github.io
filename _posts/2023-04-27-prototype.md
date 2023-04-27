---
layout: single
title: "[JavaScript] 프로토타입"
categories: javascript
tag: [prototype, 생성자]
author_profile: false
search: true
use_math: false
sidebar:
  nav: "counts"
---

# 프로토타입

프로토타입(Prototype)은 자바스크립트에서 객체 지향 프로그래밍의 핵심 개념 중 하나입니다. **모든 객체는 해당 객체의 프로토타입을 가리키는 [[Prototype]]이라는 숨겨진 프로퍼티를 가지고 있으며, 이를 통해 상속과 같은 기능을 구현할 수 있습니다.**

```javascript
function Food(name) {
    this.name = name;
    this.smell = function() {
        console.log(this.name + "냄새가 난다.");
    }
}

let myFood = new Food("로제 파스타");
let myFood2 = new Food("창란젓");

console.log(myFood.smell === myFood2.smell);	// false
```

**myFood.smell 메소드와 myFood2.smell 메소드는 서로 다른 참조를 하고 있었습니다.**

즉 객체를 생성할 때마다 별개의 함수가 계속 만들어진 것입니다.

```javascript
function smell() {
    console.log(this.name + " 냄새가 난다.");
}

function Food(name) {
    this.name = name;
    this.smell = smell;
}

let myFood = new Food("로제 파스타");
let myFood2 = new Food("창란젓");

myFood.smell();	// "로제 파스타 냄새가 난다."
myFood2.smell();	// "창란젓 냄새가 난다."
console.log(myFood.smell === myFood2.smell);	// true
```

이제 두 객체의 메서드는 같은 함수를 참조하게 되었습니다.



# constructor 프로퍼티는 왜, 어떻게 들어가 있는 것인가?

**자바스크립트에서는 생성자의 prototype 프로퍼티를 통해 타입의 특징을 정의합니다.**

constructor 메소드는 Object 타입의 프로퍼티이며 prototype에 의해 정의되었습니다.

`Object.prototype.constructor`

**모든 인스턴스는 내부에 [[Prototype]] 프로퍼티를 가지며 이를 통해 생성자의 prototype 프로퍼티를 추적합니다.**

```javascript
Food.prototype.smell = function() {
    console.log(this.name + " 냄새가 난다.");
}

function Food(name) {
    this.name = name;
}

let myFood = new Food("로제 파스타");
let myFood2 = new Food("창란젓");

myFood.smell();	// "로제 파스타 냄새가 난다."
myFood2.smell();	// "창란젓 냄새가 난다."
console.log(myFood.smell === myFood2.smell);	// true
```

smell 함수를 프로토타입에 등록해두고 나니 더 이상 Food 생성자에 this를 써서 smell 함수를 등록할 필요가 없어졌습니다.

![image-20230428004247371]({{site.url}}/images/2023-04-27-prototype/image-20230428004247371.png)

**이렇게 인스턴스에서 생성자의 [[Prototype]]을 타고 올라가며 프로퍼티를 탐색하는 현상을 프로토타입 체인이라고 합니다.**

![image-20230428005115220]({{site.url}}/images/2023-04-27-prototype/image-20230428005115220.png)
