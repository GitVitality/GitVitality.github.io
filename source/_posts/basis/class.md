---
title: javascript 中的 class
date: 2019-05-14 09:00:00
categories: basis
---

## typescript 交叉类型

```javascript
function extend<T, U>(first: T, second: U): T & U {
    let result = <T & U>{};
    for (let id in first) {
        (<any>result)[id] = (<any>first)[id];
    }

    for (let id in second) {
        if (!result.hasOwnProperty(id)) {
            (<any>result)[id] = (<any>second)[id];
        }
    }
    return result;
}

class Person {
    constructor(public name: string) { }
}
interface Loggable {
    log(): void;
}
class ConsoleLogger implements Loggable {
    log() {
        // ...
    }
}
var jim = extend(new Person("Jim"), new ConsoleLogger());
var n = jim.name;
jim.log();
```

## typescript 运行结果

--target ES5, log 方法被拷贝
--target ESNext, log 方法没有被拷贝

## 场景分析

### typescript 编译结果

在 --target ES5 的情况下，编译成 prototype 形式，在 --target ESNext 的情况下，直接编译成 class 格式；

### 原因

class 格式下将 prototype 的 log 属性的 enumerable 值设置成了 false

```javascript
Object.getOwnPropertyDescriptor(ConsoleLogger.prototype, 'log')
```
