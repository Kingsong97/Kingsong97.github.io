---
layout: post
title: TypeScript) 타입스크립트
date: 2024-05-18 09:59 +0900
description: 
image: 
category: javascript
tags: javascript TypeScript
published: true
sitemap: true
---

# 타입스크립트란??
>타입스크립트(TypeScript)는 자바스크립트에 정적 타입을 추가하는 오픈 소스 프로그래밍 언어입니다. Microsoft에 의해 개발되었으며, 자바스크립트를 확장하여 대규모 애플리케이션 개발을 보다 쉽고 관리하기 편하도록 설계되었습니다. 자바스크립트는 동적 타입 언어이지만, 타입스크립트는 정적 타입을 통해 컴파일 시점에 타입 검사를 수행하여, 런타임 에러를 줄이고 코드의 안정성을 높일 수 있습니다.

## 타입스크립트의 주요 특징

### 1. 정적타입 시스템
> 타입스크립트의 가장 큰 특징은 정적 타입 시스템입니다. 변수, 매개변수, 객체 속성, 반환값 등에 명시적으로 타입을 지정할 수 있습니다. 타입스크립트의 타입 시스템은 선택적으로 타입을 명시할 수 있는 것이 특징이며, 이를 통해 더욱 유연한 개발이 가능합니다.

### 2.클래스 기반 객체지향 프로그래밍
> 타입스크립트는 클래스, 인터페이스, 상속 등 자바와 같은 객체지향 프로그래밍 언어에서 볼 수 있는 기능들을 지원합니다. 이를 통해 객체지향적인 코드 설계를 할 때 더욱 직관적이고 체계적으로 접근할 수 있습니다.

### 3. 모듈지원
> 타입스크립트는 ES6의 모듈 시스템을 지원합니다. 각 파일은 모듈로 취급되며, import와 export를 사용하여 모듈 간에 기능을 쉽게 공유할 수 있습니다.

### 4. 최신 자바스크립트 지원
> 타입스크립트는 최신 자바스크립트(ECMAScript) 사양을 지원합니다. 따라서, 최신 자바스크립트 기능을 사용하면서도 타입스크립트의 장점을 누릴 수 있습니다.

### 5. 도구의 지원
> 강력한 타입 시스템 덕분에, 타입스크립트는 자동완성, 코드 리팩토링, 타입 체크 등의 기능을 지원하는 풍부한 개발 도구를 제공합니다. 이는 개발자가 더욱 효율적으로 코드를 작성하고, 오류를 줄일 수 있게 돕습니다.

## 타입스크립트의 주요 문법 요소

**변수 타입 지정**
````typescript
let username: string = "Alice";
let score: number = 85;
let isActive: boolean = true;
````
<br>

**인터페이스**
````typescript
interface User {
    name: string;
    age: number;
}

let user: User = {
    name: "Bob",
    age: 30
};

````
<br>

**모듈**
````typescript
// math.ts
export function add(x: number, y: number): number {
    return x + y;
}

// app.ts
import { add } from "./math";
console.log(add(5, 3));

````

## 타입스크립 사용의 이점

- 에러 감소: 컴파일 시점에 발생할 수 있는 많은 오류를 사전에 감지하여 수정할 수 있습니다. <br>

- 코드 유지 보수성 향상: 명확한 타입 시스템은 대규모 애플리케이션의 관리와 유지 보수를 용이하게 합니다. <br>

- 팀 협업 향상: 명시적인 타입 정의는 팀원 간의 의사소통을 명확하게 하며, 의도치 않은 오류를 줄일 수 있습니다. <br>

## 타입스크립트의 고깁기능

### 제네릭 
> 제네릭은 타입스크립트에서 코드를 보다 유연하고 재사용 가능하게 만드는 기능입니다. 제네릭을 사용하면, 여러 타입에 걸쳐 함수나 클래스를 사용할 수 있어 코드의 중복을 줄이고 타입 안정성을 높일 수 있습니다

````typescript
function identity<T>(arg: T): T {
    return arg;
}

let output1 = identity<string>("myString");
let output2 = identity<number>(100);
````


### 고급 타입 (Advanced Types) 
> 타입스크립트는 유니언 타입, 인터섹션 타입, 맵드 타입, 조건부 타입 등 다양한 고급 타입 기능을 제공합니다. 이를 통해 개발자는 더욱 세밀하고 강력한 타입 체크를 구현할 수 있습니다

````typescript
type StringOrNumber = string | number;
type Text = string & { length: number };
type Record<K extends keyof any, T> = {
    [P in K]: T;
};

// 조건부 타입
type Check<T> = T extends string ? "Yes" : "No";
type TypeCheck = Check<string>;  // "Yes"

````

### 데코레이터 (Decorators)
> 데코레이터는 클래스 선언, 메소드, 액세서, 프로퍼티, 파라미터에 추가적인 동작을 선언하는 데 사용할 수 있는 특별한 종류의 선언적 함수입니다. 데코레이터는 Angular 같은 프레임워크에서 많이 사용됩니다.

````typescript
function sealed(constructor: Function) {
    Object.seal(constructor);
    Object.seal(constructor.prototype);
}

@sealed
class Greeter {
    greeting: string;
    constructor(message: string) {
        this.greeting = message;
    }
    greet() {
        return "Hello, " + this.greeting;
    }
}
````

### 네임스페이스 (Namespaces)
> 네임스페이스는 코드를 더욱 체계적으로 조직할 수 있게 해주며, 외부에서의 접근을 제한하여 이름 충돌을 방지합니다

````typescript
namespace Validation {
    export interface StringValidator {
        isValid(s: string): boolean;
    }

    const lettersRegexp = /^[A-Za-z]+$/;
    export class LettersOnlyValidator implements StringValidator {
        isValid(s: string) {
            return lettersRegexp.test(s);
        }
    }
}

````

## 타입스크립트의 사용 이유

- 코드 품질과 안정성 향상: 컴파일 타임에서 오류를 잡아내어 런타임 에러의 가능성을 줄입니다. <br>

- 개발 도구와의 통합: 코드 완성, 리팩토링 도구, 오류 검출 등이 향상되어 개발 과정이 더욱 효율적이고 즐거워집니다. <br>

- 대규모 프로젝트에 적합: 타입스크립트의 기능들은 대규모 코드베이스를 쉽게 관리하고 협업할 때 특히 유용합니다. <br>