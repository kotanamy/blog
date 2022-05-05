---
weight: 1
title: "Road to IOS/Swift dev part 3"
date: 2022-04-13
draft: false
author: "Kotanamy"
description: "Стартовые понятия Swift"

tags: ["Swift/IOS", "Tutorial"]
categories: ["Documentation"]

lightgallery: true
---

Стартовые понятия Swift. Условные операторы.

<!--more-->

# Road to IOS/Swift dev part 3
## **Стартовые понятия Swift**

<br>

### 6. Условные операторы

6.1 if

Конструкция if проверяет истинность некоторого условия и в зависимости от результатов проверки выполняет отпределенный код

Условия бывают:
1. if a != b
2. if a <= b
3. if a >= b
4. if a > b
5. if a < b
6. if a ~= b // где a - диапазон

```Swift
let a = 1
let b = 2

if(a == b){
    print("а равно b")
} else if(a > b){
    print("a больше b")
} else {
    print("в другом случае")
}

// можно просто Bool подставить в условие
let boolVal = true

if boolVal {
    print("true")
} else {
    print("false")
}

// условие с диапазоном
if 1...5 ~= b {
    print("b принадлежит диапазону от 1 до 5")
}

// Условия могут быть составными
if a == 1 && b != 10 {
    print("a равно 1 и b НЕ равно 10")
}
```

Тернарный оператор аналогичен простой конструкции if и имеет следующий синтаксис:

```
[первый операнд - условие] ? [второй операнд - если условие = true] : [третий операнд - если условие = false]
```

```Swift
let a = true

let b = a ? "true" : "false" // b = true  
```

6.2 guard

Функции будут описаны позднее, так что на func следует просто принимать за абстрактный блок кода в вакууме

```Swift
func someFunc(a: Int, b: Int){
    // guard всегда требует else
    guard a == b else { return } // если a == b, то выполнение блока кода прерывается

    // аналогия с использованием if
    if a == b {
        // тут кодес
    } else {
        return
    }
}
```

6.3 switch

switch нужет чтобы if else не городить :)

```Swift
let total = 10

switch total{
    case 5...10:
        print("You win")
        fallthrough // провалиться вниз чтобы еще отыграл еще один case
    case 0, -1:
        print("******* mount of this casion")
    case 1...5:
        print("Great")
    default:
        print("Casino broken")
}
```

