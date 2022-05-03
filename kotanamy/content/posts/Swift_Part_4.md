---
weight: 1
title: "Road to IOS/Swift dev part 4"
date: 2022-04-10
draft: false
author: "Kotanamy"
description: "Стартовые понятия Swift"

tags: ["Swift/IOS", "Tutorial"]
categories: ["Documentation"]

lightgallery: true
---

# Road to IOS/Swift dev part 4
## **Стартовые понятия Swift**

<br>

### 7. Циклы

7.1 Цикл for

```Swift
for i in 0...5 {
    print(i)
}

let arr = ["A", "B", "C"]

for i in arr{
    print(i)
}
```

А со словарями следующая ситуация:

```Swift
let peopleAndViews = ["Johe news": 712354, "Marmok": 1410319312, "Joma tech": 5013001]

for (name, view) in peopleAndViews {
    print("Name: \(name) View: \(view)")
}
```

Найдем индекс и число которое делится на 2

```Swift
let arr = [10,20,3,78,41,1]

for (index, value) in arr.enumerated(){
    print(index, value)
}
```

Если нужен не весь диапазон, а часть или нужно пройтись с шагом
```Swift
for i in stride(from: 0, through: 10, by: 5) { // от до шаг
    print(i) // 0 5 10
}
```

7.2 while и repeat while

while

```Swift
var timer = 5
print("Counting start")

while timer > 0 { // цикл будет работать ПОКА timer > 0
    print(timer)
    timer--
}

print("Counting stop")

//
// теперь поменяем на repeat while
//

timer = 5 // возвращаем таймер в исходное положение

repeat { // суть в том, что цикл выполнит по крайней мере 1 итерацию
    print(timer)
    timer--
} while (timer > 0) // проверяем условие тут
```