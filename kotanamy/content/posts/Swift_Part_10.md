---
weight: 1
title: "Road to IOS/Swift dev part 10"
date: 2022-05-21
draft: false
author: "Kotanamy"
description: "Стартовые понятия Swift"

tags: ["Swift/IOS", "Tutorial"]
categories: ["Documentation"]

lightgallery: true
---

Стартовые понятия Swift. Структуры.

<!--more-->

# Road to IOS/Swift dev part 10
## **Стартовые понятия Swift**

<br>

### 18. Структуры (Structures)

**Структура** - это контейнер, который хранит данные в определенном макете. Этот "макет" позволяет структуре данных быть эффективной в некоторых операциях и неэффективной в других.

Еще одно определение:

**Структура** - это пользовательский тип данных, который используется наряду с классами и может содержать какие-либо данные и методы.

Отличия класса от структуры:

1. Наследование не работает в структурах
2. Класс - это ссылочный тип, а структура - тип значения

_**Примечание.** Структура не поддерживает наследование, но может реализовывать протокол. Т.о. можно симулировать наследование :D_

_**Примечание.** Оператор тождественного равенства (===) проверяет принадлежность экземпляра класса к одной ячейки памяти, т.е. является ли один экземпляр класса ссылкой другого экземпляра класса._

Пример с классом
```Swift
class Person {
    var fullname = "Yuu Kamiya"

    let ranobe = "no game no life"
    let followersToday = 999999999
    let fallowersYesterday = 12389028190

    var followersTommorow: Int {
        return (followersToday + fallowersYesterday) / 2
    } 

    lazy var someLazyProperty = 1 + 5

    func description() -> String {
        return "Im \(fullname)! Today we have \(followersToday) followers, yesterday: \(fallowersYesterday). Tommorow we will have about \(followersTommorow) followers"
    }
}

let person1 = Person()

func changePersonFullname(person: Person) -> Person {
    person.fullname = "Asagiri Kafuka"
    return person
}

changePersonFullname(person1)
print(person1.fullname) // Asagiri Kafuka

let person2 = Person()
let person3 = Person()
let person4 = person2

// person2 !=== person3, хотя один и одинаковые
// но лежат в двух разных ячейках памяти

// person2 === person4 так как person4 и person2
// ссылаются на одну ячейку памяти
```

Теперь попробуем переписать это все в структуру
```Swift 
struct Person {
    var fullname = "Yuu Kamiya"

    let ranobe = "no game no life"
    let followersToday = 999999999
    let fallowersYesterday = 12389028190

    var followersTommorow: Int {
        return (followersToday + fallowersYesterday) / 2
    } 

    lazy var someLazyProperty = 1 + 5

    func description() -> String {
        return "Im \(fullname)! Today we have \(followersToday) followers, yesterday: \(fallowersYesterday). Tommorow we will have about \(followersTommorow) followers"
    }
}

let person = Person()

// неработающий вариант
// func changePersonFullname(person: Person) -> Person {
//    person.fullname = "Asagiri Kafuka" // это выведет ошибку
//                                    // так как person константа (let)
//    return person
// }

func changePersonFullname(p: Person) -> Person {
    var p = p // p которая передается в функцию - переменная
    p.fullname = "Asagiri Kafuka"
    return p
 }

changePersonFullname(person)
// НЕ ИЗМЕНИТСЯ
print(person.fullname) // Yuu Kamiya
```

_**Примечание.** Оператор === не применим к структурам так как это НЕ ссылочный тип данных, а тип значения, поэтому они всегда находятся в разных ячейках памяти._