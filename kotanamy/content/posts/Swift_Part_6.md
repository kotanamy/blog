---
weight: 1
title: "Road to IOS/Swift dev part 6"
date: 2022-05-01
draft: false
author: "Kotanamy"
description: "Стартовые понятия Swift"

tags: ["Swift/IOS", "Tutorial"]
categories: ["Documentation"]

lightgallery: true
---

Стартовые понятия Swift. Кортежи, опционалы.

<!--more-->

# Road to IOS/Swift dev part 6
## **Стартовые понятия Swift**

<br>

### 10. Кортежи (Tuples)

Кортежи в Swift представляют набор значений, которые рассматриваются как один объект. Для создания кортежа используются скобки, внутри которых записываются все элементы кортежа.

Simple:
```Swift
let one = 1
let two = 2
let three = 3

(one, two, three) // кортеж
```
Вызов элемента кортежа:
```Swift
let boy = (5, "Sergey")
print(boy.0)
print(boy.1)

// Так же можно явно указать тип значений
let cloneBoy: (Int, Strint) = (5, "Sergey(Clone)")
```

Присвоим нескольким константам несколько значений
```Swift
let (a, b, c) = (1, 2, 3)
print(b) // 2
```

```Swift
// Пример поинтереснее
let greenPencil = (color: "green", length:20, weight: 4)
print(greenPencil.length) // 20
let(greenColor, greenLength, greenWeight) = greenPencil

// Еще примерчик
// Кто старше на работе?

let agesAndNames = ["Petya": 30, "Artem": 90, "Nicolay": 157]

var age = 0
var name = ""

for(nameInD, ageInD) in agesAndNames {
    if age < ageInD {
        age = ageInD
        name = nameInD
    }
}

print(age) // 157
print(name) // Nicolay
```

Возврат кортежа из функции

```Swift
func getScoreAndName() -> (name: String, score: Int) {
    let _name = "Nagibator2007"
    let _score = 0
    return (_name, _score)
}
```

### 11. Опционалы (Optionals)

Опционалы - это удобный механизм обработки ситуаций, когда значение переменной может отсутствовать. Значение будет использовано, только если оно есть.

```Swift
var fuel : Int? // ? - показывает что тип опциональный
fuel = 20

print("\(fuel) liters left") // Optional(20) liters left

// Принудительное изменение значения
// если в этот момент fuel будет nil, то приложение крашнется

print("\(fuel!) liters left")

fuel = nil

// Функциональная привязка
// она позволяет "мягко" извлечь значение
// если оно имеется, если nil
// то пишем в else обработчик этого дела

if let availableFuel = fuel {
    print("\(availableFuel) liters left")
} else {
    print("fuel = nil") // выведет это, так как fuel = nil
}

func checkFuel(){
    guard let avalFuel = fuel else {
        print("Все плохо")
        return
    }

    print(avalFuel)
}

checkFuel() // Все плохо
fuel = 10
checkFuel() // 10
```
<br>

```
                  | hi im Elvice |
Элвис будет       |__   _________|          | hi, Elvice  |
                     \/                     | ____________|
после class'ов       .? (-.-) <=============|/

Пы.Сы. в 14 статье 23 главе
```