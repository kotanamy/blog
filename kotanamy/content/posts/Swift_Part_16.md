---
weight: 1
title: "Road to IOS/Swift dev part 16"
date: 2022-06-02
draft: false
author: "Kotanamy"
description: "Стартовые понятия Swift"

tags: ["Swift/IOS", "Tutorial"]
categories: ["Documentation"]

lightgallery: true
---

Стартовые понятия Swift. Протоколы.

<!--more-->

# Road to IOS/Swift dev part 16
## **Стартовые понятия Swift**

<br>

### 27. Протоколы (Protocols)

Протокол - набор требований, который предъявляется классу, структуре или перечислению. (В других ЯП называется interface)

```Swift
// Какие требования мы хотим предъявить водителю?
protocol Driver {
    var car: Bool { get set }
    var license: Bool { get }

    func toDrive() -> Bool
}

// Класс FirmDriver подписан под протокол Driver
// поэтому все "требования" протокола Driver должны быть "выполнены"
// В терминах протоколов (интерфейсов) говорят что класс FirmDriver должен реализовать
// интерфейс Driver
class FirmDriver : Driver { // похоже на наследование классов, но это не оно :)
    // Должен быть (требование протокола)
    var licence = true
    // Должен быть (требование протокола)
    var car = true

    // Должен быть (требование протокола)
    func toDrive() -> Bool {
        return true
    }
}


let firmDriver = FirmDriver()
print(firmDriver.license) // true
```

Так же можно разгрузить класс FirmDriver используя расширения

```Swift
protocol Driver {
    var car: Bool { get set }
    var license: Bool { get }

    func toDrive() -> Bool
}

// Разгрузили класс :)
extension FirmDriver : Driver {
    var licence: Bool { return true }
    var car: Bool { return true }

    func toDrive() -> Bool{
        return true
    }
}

class FirmDriver {

}


let firmDriver = FirmDriver()
print(firmDriver.license) // true
```

Можно расширить протокол, а так же протокол может быть подписан под протокол

```Swift
protocol Human {
    var name: String { get }
}

// Протокол Driver подписан под протокол Human
protocol Driver : Human {
    var car: Bool { get set }
    var license: Bool { get }

    func toDrive() -> Bool
}

// Расширяем протокол
// Получается что мы пишем дефолтную реализацию протокола
extension Driver {
    var licence: Bool { return true }
    var car: Bool { return true }

    // Реализовано в классе, но здесь тоже можно
    // var name: String {
    //    return "Натаха"
    // }

    func toDrive() -> Bool{
        return true
    }
}

// Так как класс FirmDriver подписан под протокол Driver,
// а Driver подписан под протокол Human
// то следует реализовать все методы из Human в FirmDriver
class FirmDriver : Driver {
    var licence: Bool {
        return false
    }
    
    // Реализуем name
    var name: String {
        return "Натаха"
    }
}


let firmDriver = FirmDriver()
// Здесь изменилось на false
print(firmDriver.license) // false
print(firmDriver.car) // true
```

Классы, структуры, перечисления можно подписать под несколько протоколов

```Swift
class A: Protocol1, Protocol2, Protocol3 {}
```