---
weight: 1
title: "Road to IOS/Swift dev part 11"
date: 2022-05-22
draft: false
author: "Kotanamy"
description: "Стартовые понятия Swift"

tags: ["Swift/IOS", "Tutorial"]
categories: ["Documentation"]

lightgallery: true
---

Стартовые понятия Swift. Приведение типа. Проверка типа. Any. Anyobject.

<!--more-->

# Road to IOS/Swift dev part 11
## **Стартовые понятия Swift**

<br>

### 19.  Приведение типа. Проверка типа. (Type casting. Type checking)

**Приведение типа** - это способ проверить тип экземпляра и/или способ обращения к экземпляру так, как если бы он был экземпляром суперкласса или подкласса откуда-либо из своей собственной классовой иерархии.

Приведение типов в Swift реализуется с помощью операторов is и as. Эти два оператора предоставляют простой способ проверки типа значения или преобразование значения к другому типу.

```Swift
// Базовый класс мебель
class Furniture {
    let material: String

    init(material: String){
        self.material = material
    }
}
// Класс наследник кровать
class Bed: Furniture {
    let numberOfPlaces: Int

    init(numberOfPlaces: Int, material: String) {
        self.numberOfPlaces = numberOfPlaces
        super.init(material: material)
    }
}
// Класс наследник шкаф
class Cupboard: Furniture {
    let numberOfShelves: Int

    init(numberOfShelves: Int, material: String) {
        self.numberOfShelves = numberOfPlaces
        super.init(material: material)
    }
}

// создаем массив и добавляем в него несколько шкафов и кроватей
var arrayOfFurniture = [Furniture]()

arrayOfFurniture.append(Bed(numberOfPlaces:2, material: "Wood"))
arrayOfFurniture.append(Bed(numberOfPlaces:1, material: "Steel"))
arrayOfFurniture.append(Bed(numberOfPlaces:2, material: "Wood"))

arrayOfFurniture.append(Cupboard(numberOfShelves:1, material: "Wood"))
arrayOfFurniture.append(Cupboard(numberOfShelves:5, material: "Steel"))
arrayOfFurniture.append(Cupboard(numberOfShelves:2, material: "Steel"))
arrayOfFurniture.append(Cupboard(numberOfShelves:3, material: "Wood"))

// Сюда будем считать кровати\шкафы
var bed = 0
var cupboard = 0

for item in arrayOfFurniture {
    if item is Bed { // is проверяет, является ли предмет (item) кроватью (bed)
        bed += 1
    } else {
        cupboard += 1
    }
}

print(bed) //3
print(cupboard) // 4

// Сюда будем пушить кровати\шкафы
var arrayOfBed = [Bed]()
var arrayOfCupboard = [Cupboard]()

for item in arrayOfFurniture{ // отделяем котлеты от мух и складываем их по разным коробкам
    if item is Bed {
        let _bed = item as! Bed // приводим к типу Bed ПРИНУДИТЕЛЬНО
        arrayOfBed.append(_bed)
    } else if item is Cupboard {
        let _cupboard = item as! Cupboard // приводим к типу Cupboard ПРИНУДИТЕЛЬНО
        arrayOfBed.append(_cupboard)
    }
}

// верхний алгоритм можно переписать и так
/*
for item in arrayOfFurniture{ // отделяем котлеты от мух и складываем их по разным коробкам
    if let _bed = item as? Bed { // приводим к типу Bed ОПЦИОНАЛЬНО
        arrayOfBed = _bed
    }
    if let _cupboard = item as? Bed {
        arrayOfCupboard = _cupboard // приводим к типу Cupboard ОПЦИОНАЛЬНО
    }
}
*/
```

### 20. Any. Anyobject.

- **Any** - может представлять экземпляр любого типа (но не nil)
- **AnyObject** - может представлять экземпляр любого класса

```Swift
class A{}
class B{}
class C{}

let a = A()
let b = A()
let c = A()
let d = B()
let e = C()

struct D {}

let f = D()

enum E {
    case a
    case b
}

let g = E.b

let array: [AnyObject] = [a, b, c, d, e] // мы не можем добавить в массив
                                    // структуру f и перечисление g
                                    // потому что AnyObject обозначает
                                    // тип любого класса

let arrayTwo: [Any] = [a, b, c, d, e, f, g, true, "", 0.95, 4] // Any может содержать значение типа
                                        // класс, структура, перечисление,
                                        // стандартных типов и тд, но НЕ может содержать nil


// выводим тип в консоль
for i in arrayTwo {
    switch i {
        case let item as Int: // приведение типов в switch используется без ? или !
            print("Int")
        case let item as Float:
            print("Float")
        // и так далее :D
        default:
            print("other")
    }
}
```

_**Примечание.** Есть ли разница между class и AnyObject?_

```Swift
protocol A: class{

}

protocol B: AnyObject{
    
}
```