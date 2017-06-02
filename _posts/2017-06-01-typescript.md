---
layout: post
title: "TypeScript 핸드북 - Basic Type"
description: "설치 후 타입 종류들에 대해 알아본다."
comments: true
category: Study
tags: [TypeScript]
---

[TypeScript 사이트의 문서에 있는 핸드북](https://www.typescriptlang.org/docs/handbook/basic-types.html)의 내용을 바탕으로 작성했다.


# 설치 & 사용법

사용해야되니 설치를 한다.

`npm install -g typescript`

확장자는 **.ts** 이고, 설치를 하면 tsc 명령어를 사용할 수 있다. ts 파일을 js 파일로 변환하려면 명령어를 사용하면 된다.

`tsc test.ts`

코드 확인 방법은

- ts 파일을 js로 변환 후 html 파일 만들어서 확인
- [Playground](https://www.typescriptlang.org/play/index.html)


# Basic Types

타입스크립트에서는 변수에 타입을 설정수 있고, 특별한 경우가 아니면 **var** 는 사용을 안하는게 좋다. (호이스팅 문제라던지 때문에) **let** 을 사용하자.

## Boolean
`let isDone: boolean = false;`

## Number
`let decimal: number = 6;`

## String
`let color: string = "blue";`

string 타입의 경우 backquote(`) 를 사용해서 여러줄의 문자열을 사용할 수 있다. 이 문자열은 ${ expr } 표현식을 사용해서 변수를 대치할 수 있다.

```javascript
let time: number = 2;
let sentence: string = `새벽 ${ time }시인데 
배가 고프다.
첵스를 먹어야겠다.`;
```

## Array

`let list: number[] = [1, 2, 3];`
`let list: Array<number> = [1, 2, 3];`

## Tuple

튜플은 파이썬이나 스위프트처럼 변경 불가능한 자료형이 아니고 여러 타입을 지정할 수 있는 배열이다.

```javascript
let x: [string, number];

x = ["hello", 10]; // OK

x = [10, "hello"]; // Error

console.log(x[0].substr(1)); // OK
console.log(x[1].substr(1)); // Error, 'number' does not have 'substr'
```

배열의 인덱스 범위 밖에서 할당하게 되면 **Union Type** 으로 사용이 되는데 이건 **Advanced Type** 에서 다루겠다.

```javascript
x[3] = "world"; // OK, 'string' can be assigned to 'string | number'

console.log(x[5].toString()); // OK, 'string' and 'number' both have 'toString'

x[6] = true; // Error, 'boolean' isn't 'string | number'
```

## Enum

C# 의 enum과 비슷하다.

```javascript
// 기본값으로 0에서 시작하고
enum Color {Red, Green, Blue}
let c: Color = Color.Green;

// 이렇게하면 1에서 시작한다.
enum Color {Red = 1, Green, Blue}
let c: Color = Color.Green;

// 또는 전부 값을 줄 수 있다.
enum Color {Red = 1, Green = 2, Blue = 4}
let c: Color = Color.Green;

// 값으로 이름을 가져올 수 있다.
enum Color {Red = 1, Green, Blue}
let colorName: string = Color[2];

alert(colorName);
```

## Any

어떠한 타입이든 올 수 있다.

```javascript
let notSure: any = 4;
notSure = "maybe a string instead";
notSure = false; // okay, definitely a boolean
```

Object 와는 다르다.

```javascript
let notSure: any = 4;
notSure.ifItExists(); // okay, ifItExists might exist at runtime
notSure.toFixed(); // okay, toFixed exists (but the compiler doesn't check)

let prettySure: Object = 4;
prettySure.toFixed(); // Error: Property 'toFixed' doesn't exist on type 'Object'.
```

## Void

any 타입의 반대다. 보통 함수의 반환값이 없을때 쓰인다.

```javascript
function warnUser(): void {
    alert("This is my warning message");
}
```

## Null and Undefined

## Never

반환할 수 없는 함수에서 사용하기 위한 서브타입이다.

```javascript
function error(message: string): never {
    throw new Error(message);
}
```

## Type assertions

타입 캐스팅과 비슷하지만 컴파일러에서만 사용된다.

```javascript
let someValue: any = "this is a string";

let strLength: number = (someValue as string).length;

let strLength: number = (<string>someValue).length;
```








