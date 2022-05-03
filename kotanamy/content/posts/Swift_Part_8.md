---
weight: 1
title: "Road to IOS/Swift dev part 8"
date: 2022-05-03
draft: false
author: "Kotanamy"
description: "Стартовые понятия Swift"

tags: ["Swift/IOS", "Tutorial"]
categories: ["Documentation"]

lightgallery: true
---

# Road to IOS/Swift dev part 8
## **Стартовые понятия Swift**

<br>

### 13. Вычисляемые свойства

```Swift
class Rectangular {
    let height: Int
    let width: Int
    let depth: Int

    var volume: Int { // вычисляемое свойство (объем)
        return height * width * depth
    }

    init(height: Int, width: Int, depth: Int){
        self.height = height
        self.width = width
        self.depth = depth
    }
}

let rect = Rectangular(height: 2, width: 3, depth: 4)
print(rect.volume) // 24
```

Герттеры и сеттеры

get - возвращает значение

set - устанавливает значение

Сразу примерчик

```Swift
// Еще немного инфы на примерчике
class Person {
    let name: String
    let secondName: String

    init (name: String, secondName: String) {
        self.name = name
        self.secondName = secondName
    }

    var fullName: String {
        get {
            return name + " " + secondName
        }
        set(anotherNewValue) { // по дефолту идет newValue, мы переопределяем на anotherNewValue
            let array = anotherNewValue.components(separatedBy: " ") // функция которая разбивает строку на массив элементов (по пробелу ориентируется)
            name = array[0]
            secondName = array[1]
        }
    }
}

let person = Person(name: "Name", secondName: "secondName")
print(person.fullName) // Name secondName

person.fullName = "Utka Duck"
print(person.name) // Utka
print(person.secondName) // Duck
```


14. Свойства класса(Class properties)

```Swift
class Car {
    // properties
    let products: Int
    let people: Int
    let pets: Int
    class var selfWeight: Int { return 1500 }
    class var maxWeight: Int { return 2000 }

    // computed property
    var totalWeight: Int {
        return products + people + pets + Car.selfWeight
    }

    // Init
    init(products: Int, people: Int, pets: Int){
        self.products = products
        self.people = people
        self.pets = pets
    }
}

let car = Car(products: 30, people: 300, pets: 50)
print(car.totalWight) // 1880

let maxWeight = Car.maxWeight // вызываем свойство КЛАССА, а НЕ объекта
let carWeight = Car.selfWeight // то же самое
let totalWeight = car.totalWeight // car с маленькой буквы, это ОБЪЕКТ

if maxWeight < totalWeight {
    print("All ok")
} else {
    print("Not ok")
}
```

14.2 **class func** и **static func**

Модификаторы static и class позволяют нам "прикрепить" методы к классу, а не к экземплярам класса. **class func** и **static func** (то же самое что и **final class func**) все же имеют различия, вот на примерчике рассмотрим:

```Swift
class A {
    class func f1() -> String {
        return "f1"
    }

    static func f2() -> String {
        return "f2"
    }

    // Функцию f2() можно также объявить как:

    // final class func f2() -> String {
    //     return "f2"
    // }
}


// итого, наследуем
class B : A {
    override class func f1() -> String {
        return "f1 from B"
    }

    // а вот это уже работать не будет, так как
    // модификатор final говорит о том, что
    // эту функцию нельзя переопределять в 
    // классах наследниках
    override static func f2() -> String {
        return "f2 from B"
    }
}
```

14.3 Ленивые свойства (lazy properties)

Ленивое свойство - не инициализируется до момента обращения к нему

```Swift
// Предположим что эта очень ресурсозатратная
func bigDataProcessingFunc() -> String {
    return "Im very big and very strong process, HAHA"
}

class Processing {
    let smallDataProcessing = "Im small process"
    let averageDataProcessing = "Average data processing"
    let bigDataProcessing = bigDataProcessingFunc()
}

class OptimizedProcessing {
    let smallDataProcessing = "Im small process"
    let averageDataProcessing = "Average data processing"
    // ленивое свойство и let -> var, 
    // так как let не даст поменять значение переменной
    lazy var bigDataProcessing = bigDataProcessingFunc() 
}

// инициализирует все 3 свойства сразу
let processing = Processing() 
// инициализирует сразу только 2 свойсва
let optimizedProcessing = OptimizedProcessing() 

// вот тут мы обращаемся к свойству
// и оно инициализируется
// ммм, ляпота ^_^
print(optimizedProcessing.bigDataProcessing) 
```

14.4 Уровни доступа (access level)

Уровни доступа нужны для того чтобы ограничить область видимости метода или свойства, или инкапсулировать (спрятать) реализацию метода или свойства.

Уровни доступа:

1. Открытый доступ (open access) - с таким уровнем доступа можно наследовать, переопределять члены класса. Также можно это делать в другом модуле который импортирует тот модуль, в котором вы определили этот класс
2. Публичный доступ (public access) - с таким уровнем доступа можно наследовать, переопределять члены класса внутри одного модуля
3. Внутренний (internal access) - Этот уровень доступа позволяет использовать объекты внутри любого исходного файла из их определяющего модуля, но не (по умолчанию)
4. Файл-приватный (file private access) - позволяет использовать объект в пределах его исходного файла
5. Приватный (private access) - запрещает использовать методы или свойства которые вы создали внутри класса\структуры\перечесления вне класса\структуры\перечесления

```Swift
// Файл 1

class SomeClass {
    private let property = true // property
    fileprivate let anotherProperty = false // anotherProperty

    func printPandAP(){
        print(property) // можно
        print(anotherProperty) // тоже можно
    }
}

// это расширение класса
// считайте это просто частью класса SomeClass
// подробнее будет в следующих статьях
extension SomeClass { 
    var someAP: Int {
        anotherProperty ? return 20 : return 1 // вызвать anotherProperty позволено в разширениях
    }

    // так нельзя, так как private
    // var someP: Int {
    //    property ? return 10 : return 0
    // }
}

class AnotherClass {
    let someClass = SomeClass()

    func someFunc(){
        // все ок, fileprivate свойство
        // можно вызвать в другом классе в том же файле
        print(someClass.anotherProperty)

        // вызов print(someClass.property) выдаст ошибку
        // потому что уровень доступа не позволяет
        // вызывать свойство вне класса
    }
}
```

```Swift
// Файл 2

extension SomeClass { 
    // это работать не будет, так как в другом файле
    var someAPinFile2: Int {
        anotherProperty ? return 20 : return 1
    }
}
```