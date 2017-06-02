---
layout: post
title: "TypeScript 핸드북 - Interfaces"
description: "인터페이스에 대해 알아본다."
comments: true
category: Study
tags: [TypeScript]
---

[TypeScript 사이트의 문서에 있는 핸드북](https://www.typescriptlang.org/docs/handbook/interfaces.html)의 내용을 바탕으로 작성했다.

인터페이스는 타 언어에서의 인터페이스와 비슷하다. 값이 갖는 모양에만 초점을 맞춰서 설계를 하는 것이다.

# Our First Interface

```javascript
function printLabel(labelledObj: { label: string }) {
    console.log(labelledObj.label);
}

let myObj = {size: 10, label: "Size 10 Object"};
printLabel(myObj);
```

위 코드에서 printLabel 은 labelledObj 객체를 매개변수로 받고 있는데, label 속성이 있어야 하지만 다른 속성이 없어야 되는건 아니다. printLabel를 호출할때 myObj 인자에 size 속성도 포함되어 있다. 이걸 인터페이스를 사용하여 바꿔보자

```javascript
interface LabelledValue {
    label: string;
}

function printLabel(labelledObj: LabelledValue) {
    console.log(labelledObj.label);
}

let myObj = {size: 10, label: "Size 10 Object"};
printLabel(myObj);
```





