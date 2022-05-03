---
weight: 1
title: "Road to IOS/Swift dev part 7"
date: 2022-05-03
draft: false
author: "Kotanamy"
description: "Стартовые понятия Swift"

tags: ["Swift/IOS", "Tutorial"]
categories: ["Documentation"]

lightgallery: true
---

# Road to IOS/Swift dev part 7
## **Стартовые понятия Swift**

<br>

### 12. Классы (Class)

12.1 Первое знакомство с классами

Класс - это план или шаблон для экземпляра класса. Экземпляр класса называется объект. Классы похожи на структуры, поэтому в статью про структуры я включу описание различий классов и структур.

```Swift
class Human {
    var name = "Dimon" // константы и переменные в классе называются свойствами
    var age: Int? = 30 // у девушек возраст спрашивать нельзя
    var hairs = true

    func description() -> String { // функции в классе называют методами
        if let _age = age { // если вдруг женщина
            return "Im \(name) and age = \(_age)"
        } else {
            return "Im \(name) and i woman"
        }
        
    }
}

let humanOne = Human() // ссылка на объект типа Human humanOne - const
humanOne.name = "Natasha" // так можно
print(humanOne.name)
humanOne.description()

let h2 = Human()
h2.hairs = false
h2.name = "John"
print(h2)

var array = [Human]()
array.append(humanOne)
array.append(h2)

// Попросим рассказать людей о себе

for p in array {
    p.description()
}
```

12.2 Инициализаторы

Инициализаторы нужны для инициализации объекта класса (Спасибо, капитан)

```Swift
class Petuh {
    let id
    var name: String 
    var age: Int

    init(){
        self.id = 0
        self.name = "Petuh"
        self.age = 0 
    }

    init(id: Int, name: String, age: Int){
        self.id = id
        self.name = name
        self.age = age
    }

    func coCo() -> Void {
        print("Co-Co and kudah, \(id) _ \(name) _ age: \(age)")
    }
}

let p1 = Petuh() // инициализатор без параметров
let p2 = Petuh(id: 1, name: "Kukish", age: 2) // инициализатор с параметрами
p1.coCo() // Co-Co and kudah 0 _ Petuh 0
p2.coCo() // Co-Co and kudah 1 _ Kukish 2
```

12.3 Наследование

Классы в Swift могут вызывать или получать доступ к методам, свойствам, индексам, принаджелащим их суперклассам и могут предоставлять свои собственные переписанные версии этих методов, свойств, индексов для усовершенствования или изменения их поведения.

```Swift
// Базовый класс/суперкласс/родительский класс
class Chicken {
    var name: String 

    init(){
        self.name = "Petuh"
    }

    init(name: String){
        self.name = name
    }

    func coCo() -> String {
        return "Co-Co and kudah, \(name)"
    }
}

// класс наследник
class Petuh: Chicken {
    var egg: Int

    init(egg: Int, name: String) {
        // можно здесь вызвать
        // инициализатор базового класса
         super.init(name: name)
         self.egg = egg 
    }

    override init(name: String) {
        super.init(name: name)
        self.egg = 1
    }

    override func coCo() -> String { // override - переопределяем функцию базового класса
        let originalText = super.coCo() // super - метод базового класса
        return originalText + " I have a \(self.egg) egg"
    }

}

let petuh = Petuh(name: "Petuch")
print(petuh.coCo()) // Co-Co and kudah, Petuch I have a 1 egg

let petuh1 = Petuh(egg: 2, name "Petuch1")
print(petuh1.coCo()) // Co-Co and kudah, Petuch1 I have a 2 egg
```