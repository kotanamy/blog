---
weight: 1
title: "Road to IOS/Swift dev part 17"
date: 2022-06-09
draft: false
author: "Kotanamy"
description: "Стартовые понятия Swift"

tags: ["Swift/IOS", "Tutorial"]
categories: ["Documentation"]

lightgallery: true
---

Стартовые понятия Swift. Универсальные типы.

<!--more-->

# Road to IOS/Swift dev part 17
## **Стартовые понятия Swift**

<br>

### 28. Универсальные типы (Generics)

```Swift
class A{

}

let array = [1, 2, 3, 4]
let array = ["one", "two", "three"]
let arrayCls = [A(), A(), A()]

// Создадим 2 функции (по сути одинаковые, 
// только работают с разными типами входных параметров)
func paramValue(param : Int) -> String {
    return "\(param)"
}

func paramValue(param : String) -> String {
    return "\(param)"
}

print(paramValue(5)) // 5
print(paramValue("bla")) // bla


// Теперь объединим 2 функции выше в 1
// используя дженерики
func pVal<T>(param : T) -> String {
    return "\(param)"
}

print(pVal(5)) // 5
print(pVal("bla")) // bla

// Generic в структурах
// Сразу 2 типа
struct HelpfulFunctions<T, U> {
    func pValue(param: T, param2 : U) -> String {
        return "\(param) \(param2)"
    }
}

let structExample = HelpfulFunctions<String, Int>()
print(structExample.pValue(param: 5, param2 : "bla")) // 5 bla

// В generics структурках можно указать протоко
// которому будут соответствовать типы
struct stct<T : Comparable, U : Equatable> {}

// Меняем 2 значения
var a = "b"
var b = "a"

func swappy<T>(a: inout T, b: inout T){
    let temp = a
    a = b
    b = temp
}

swappy(&a, &b)
print(a) // a
print(b) // b
```