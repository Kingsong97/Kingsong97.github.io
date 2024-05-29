---
layout: post
title: Javascript) 프로토타입(prototype)
date: 2024-05-25 12:39 +0900
description: 
image: ../assets/img/jsb.jpg
category: Javascript
tags: Javascript prototype
published: true
sitemap: true
---

# 🔅 자바스크립트의 프로토타입(prototype)

자바스크립트는 객체 지향 프로그래밍 언어로, 다른 객체 지향 언어와는 다르게 클래스(class) 대신 프로토타입(prototype)이라는 개념을 사용합니다.     
이번 글에서는 자바스크립트의 프로토타입이 무엇인지, 어떻게 작동하는지, 그리고 이를 어떻게 사용할 수 있는지 쉽게 설명하겠습니다.

<br>

## 프로토타입이란 무엇인가?
--- 
자바스크립트에서 모든 객체는 프로토타입이라는 숨겨진 속성을 가지고 있습니다.     
이 프로토타입은 다른 객체를 참조하며, 객체가 상속할 수 있는 속성과 메서드를 정의합니다.    
 즉, 객체가 다른 객체의 속성과 메서드를 공유할 수 있도록 하는 메커니즘입니다.    

 <br>

## 왜 프로토타입을 사용하는가?
---

프로토타입을 사용하면 객체 지향 프로그래밍에서 중요한 개념인 상속을 구현할 수 있습니다.    
상속을 통해 코드의 재사용성을 높이고, 중복 코드를 줄일 수 있습니다.    
자바스크립트에서는 프로토타입을 통해 상속을 구현합니다.   

<br>

## 프로토타입 체인
--- 

자바스크립트에서 객체는 프로토타입 `체인(prototype chain)`을 통해 서로 연결됩니다. 
<br>
객체가 특정 속성이나 메서드를 찾을 때, 먼저 자기 자신에게서 찾고, 없으면 프로토타입 체인 상위로 올라가며 찾습니다.    
이 과정은 필요한 속성이나 메서드를 찾을 때까지 계속됩니다.    

<br>

예를 들어, 객체 `a`가 있고, `a`의 프로토타입이 `b`라면, `a`에서 찾는 속성이나 메서드가 없을 경우 `b`에서 찾습니다.     
만약 `b`에서도 찾지 못하면, `b`의 프로토타입에서 계속 찾습니다.     
이렇게 객체 간의 연결이 체인처럼 이어져 있는 구조를 프로토타입 체인이라고 합니다.

<br>
<br>

## 프로토타입 사용 예제

### 객체 생성자 함수와 프로토타입
--- 

자바스크립트에서는 객체 생성자 함수를 사용하여 새로운 객체를 만들 수 있습니다.     
생성자 함수는 일반 함수와 비슷하지만, 객체를 생성하기 위해 `new` 키워드와 함께 사용됩니다.   

```javascript
function Person(name, age) {
    this.name = name;
    this.age = age;
}

const john = new Person('John', 30);
console.log(john.name); // John
console.log(john.age); // 30
```
<br>

위 예제에서 `Person` 함수는 생성자 함수입니다.     
`new Person('John', 30)`을 호출하면 새로운 객체가 생성되고, 그 객체는 `name`과 `age` 속성을 갖게 됩니다.    
    
<br>

### 프로토타입을 이용한 상속
---

생성자 함수의 프로토타입을 이용하여 메서드를 상속할 수 있습니다. 예를 들어, 모든 `Person` 객체에 `greet` 메서드를 추가해 보겠습니다.

<br>

```javascript
Person.prototype.greet = function() {
    console.log(`Hello, my name is ${this.name}`);
};

const jane = new Person('Jane', 25);
jane.greet(); // Hello, my name is Jane
john.greet(); // Hello, my name is John
```

위 예제에서 `Person.prototype.greet`를 정의하여 모든 `Person` 객체가 `greet` 메서드를 상속받도록 했습니다.   
이제 `john`과 `jane` 객체 모두 `greet` 메서드를 사용할 수 있습니다.    

<br>

### 프로토타입 체인의 예제
---

프로토타입 체인이 어떻게 작동하는지 좀 더 명확히 이해하기 위해 예제를 보겠습니다.    

```javascript
function Animal(name) {
    this.name = name;
}

Animal.prototype.speak = function() {
    console.log(`${this.name} makes a noise.`);
};

function Dog(name, breed) {
    Animal.call(this, name);
    this.breed = breed;
}

// Dog의 프로토타입을 Animal의 인스턴스로 설정
Dog.prototype = Object.create(Animal.prototype);
Dog.prototype.constructor = Dog;

Dog.prototype.speak = function() {
    console.log(`${this.name} barks.`);
};

const rex = new Dog('Rex', 'German Shepherd');
rex.speak(); // Rex barks.
```

위 예제에서 
1. `Dog` 생성자 함수는 `Animal` 생성자 함수를 상속받습니다.     
2. `Dog.prototype = Object.create(Animal.prototype)`를 통해 `Dog`의 프로토타입을 `Animal`의 인스턴스로 설정하여 `Animal`의 속성과 메서드를 상속받습니다.    
3. 그런 다음 `Dog.prototype.constructor`를 `Dog`로 설정하여 생성자 참조를 수정합니다. 
4. 이제 `Dog` 객체는 `Animal`의 `speak` 메서드를 재정의하여 고유의 `speak` 메서드를 가질 수 있습니다.

<br>


## 프로토타입의 장점과 단점
--- 

### 장점
1. **코드 재사용**: 프로토타입을 통해 여러 객체가 같은 메서드와 속성을 공유할 수 있어 코드의 재사용성을 높입니다.
2. **메모리 효율성**: 객체마다 동일한 메서드를 개별적으로 가지는 대신, 프로토타입에 정의하여 메모리를 절약할 수 있습니다.

### 단점
1. **복잡성**: 프로토타입 체인은 초보자에게 다소 복잡하게 느껴질 수 있습니다.
2. **디버깅 어려움**: 프로토타입 체인에 따라 상속이 이루어지므로, 디버깅 시 어느 객체에서 문제가 발생했는지 파악하기 어려울 수 있습니다.

<br>
<br>


## 결론
---

자바스크립트의 프로토타입은 클래스 대신 사용되는 중요한 개념입니다.    
이를 통해 객체 지향 프로그래밍의 상속을 구현하고, 코드의 재사용성을 높일 수 있습니다.     
프로토타입 체인을 이해하고 적절히 사용하는 것은 자바스크립트 개발에서 중요한 기술입니다.    