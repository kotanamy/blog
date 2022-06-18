---
weight: 1
title: "Road to IOS/Swift dev part 9"
date: 2022-05-04
draft: false
author: "Kotanamy"
description: "Стартовые понятия Swift"

tags: ["Swift/IOS", "Tutorial"]
categories: ["Documentation"]

lightgallery: true
---

Стартовые понятия Swift. Наблюдатели свойств. Перечисления

<!--more-->

# Road to IOS/Swift dev part 9 
## **Стартовые понятия Swift**

<br>

### 16. Наблюдатели свойств(property observers)

Наблюдатели свойств - это такие штуковины, которые наблюдают за изменениями значения свойств

willSet - будет изменено

didSet - было изменено

Само значение изменяется где-то между willSet и didSet

```Swift
class SecretLabEmployee {
    var accessLevel = 0 {
        willSet { // неявно willSet(newValue)
            print("willSet")
            print("new access level = \(newValue)")
        }
        didSet { // неявно didSet(oldValue) 
            if accessLevel > 0 {
                accessToDB = true
            } else {
                accessToDB = false
            }
            print("didSet, oldValue \(oldValue)")
        }
    }
    var accessToDB = false
}

let emp = SecretLabEmployee()
emp.accessLevel = 1
// выведет:
// willSet
// new access level = 1
// didSet, oldValue 0
print(emp.accessToDB) // true
```

### 17. Перечисления (enums)

Перечисления - объединяют значения и присваевают ему общий тип. Вы не можете выйти за рамки данного типа. Имеют вычисляемые свойства, методы экземпляров, инициализаторы.

```Swift
// простой enum
enum Movement : String {
    case forward
    case backward
    case left
    case right
}

let moveDir: Movement = .left // аналогично Movement.left
let sMoveDir = Movement.right.rawValue // "right"

enum Numbr : Int {
    case one
    case two = 2
}

let n = Numbr.one.rawValue // 0
let n2 = Numbr.two.rawValue // 2

// Вычисляемые свойства
enum Device {
    case iPad, iPhone, iPodTouch(color: String)

    var year: Int {
        switch self {
            case .iPhone: return 2007
            case .iPodTouch(let color) where color == "black": return 2020
            case .iPodTouch return 2005
            case .iPad return 2010
        }
    }
}

let d = Device.iPad.year // 2010
let ipod = Device.iPodTouch(color: "black").year // 2020

// Вложенные enum'ы
enum Character {
    enum Weapon : Int {
        case sword = 4
        case wand = 1

        var damage: Int{
        return rawValue * 10
        }
    }

    enum CharacterType {
        case knight
        case mage
    }
}

let charWeapon = Character.Weapon.sword.damage // 40

// Индеректрые enum'ы
enum Lunch { // inderect может быть и сам enum
    case salad
    case soup
    indirect case meal(Lunch, Lunch) // индиректный enum
}

let myLunch = Lunch.meal(.salad, .soup)

// swift 5.0 update
enum Planet {
    case earth, mars, neptune
}

func action(onPlanet planet: Planet){
    switch planet{
        case .earth:
            print("Earth")
        case .mars:
            print("Mars")
        case .neptune:
            print("Neptune")
        
        // @unknown заставляет обрабатывать каждый новый кейс,
        // например если добавить в Planet jupiter
        @unknown default:
            print("default")
    }
}
```