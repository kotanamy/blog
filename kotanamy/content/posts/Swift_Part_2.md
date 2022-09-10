---
weight: 1
title: "Road to IOS/Swift dev part 2"
date: 2022-04-12
draft: false
author: "Kotanamy"
description: "Стартовые понятия Swift"

tags: ["Swift/IOS", "Tutorial"]
categories: ["Documentation"]

lightgallery: true
---

Стартовые понятия Swift. Интерполяция строки, типы коллекций.

<!--more-->

# Road to IOS/Swift dev part 2
## **Стартовые понятия Swift**

<br>

### 4. Интерполяция строки

Интерполяция строки - это когда вы хотите внутрь строки вставить константу или переменную

```Swift
let world = "World"
let sd = "Swift Developer"

let helloWorld = "Hello \(world) or \(sd)" // "Hello World or Swift Developer"
```

### 5. Типы коллекций

Swift обеспечивает три основных типа коллекций: массивы, множества и словари.

#### 5.1. **Массивы**

Массив - это упорядоченная коллекция в которой могут содержатся переменные одного типа

Объявление массива:

```Swift
let array = Array<Int>() // пустой массив
let array1 = [Int]() // тоже пустой массив
let array2: [Int] = [] // тоже пустой массив
let array3 = [0,1,2,3] // а вот это НЕ пустой массив
let array4 = [Int](repeating: 10, count: 6) // [10, 10, 10, 10, 10, 10]
```

Основные действия с массивами:

```Swift
var array: [Int] = []
var array1 = [Int](repeating: 10, count: 6)

array += array1 // array = [10, 10, 10, 10, 10, 10]

array[2] = 5 // [10, 10, 5, 10, 10, 10]
array[3...5] = 7 // [10, 10, 5, 7, 7, 7]
print(array.count) // 6 (количество эл-тов в массиве)

array.append(100) // [10, 10, 5, 7, 7, 7, 100] добавили в конец 100
print(array.count) // 7

array.insert(1, at: 4) // [10, 10, 5, 7, 1, 7, 100] вставили элемент на позицию 4
array.remove(at: 3) // [10, 10, 5, 1, 7, 100] удалили 3 элемент (возвращает значение)

array.removeFirst() // [10, 5, 1, 7, 100] удалили 1ый элемент (возвращает значение)
array.removeLast() //  [10, 5, 1, 7] удалили последний элемент (возвращает значение)
array.reverse() // [7, 1, 5, 10] перевернули

// Самое популярное действие с массивом :)
var array_s = [ 4, 2, 1, 5, 3]
array_s.sort()
// array_s = [ 1, 2, 3, 4, 5]
```

_*Секундочка занудства*_

Наткнуся на пост в [iOS Dev](https://t.me/iOS_Career) про то как работает sort(), раньше не задумывался об этом, так что вот что он там написал.

*sort() работает следующим образом:*
1. Если элементов меньше 20, применяется сортировка вставками
2. Если больше, то интроспективная сортировка (introsort), включающая в себя быструю сортировку до определенной глубины рекурсии 2*floor(log(N))
3. Переключается на пирамидальную сортировку (heapsort или сортировка кучей), когда рекурсия превысит заранее установленный уровень 

#### 5.2 Словари

Словарь - это неупорядоченная коллекция в которой каждый элемент имеет ключ и значение

Объявление словаря:

```Swift
let dict = Dictionary<String, String>() // создали словарь
let dict2 = [String: String]() // еще одна форма записи
let dict3 = [String: String] = [:] // и еще одна форма записи

var dogName = ["Akita": "Victor", "Korgi": "Serega", "Taksa": "Den"]
print(dogName["Akita"]) // Victor
print(dogName.count) // 3
print(dogName.isEmpty) // false
dogName["Taksa"] = "Nikita" // ["Akita": "Victor", "Korgi": "Serega", "Taksa": "Nikita"]

let deleteDogName = dogName.updateValue("Aleksus", forKey: "Korgi") // вернет перед обновлением

dogName["Akita"] = nil // Удалили Victor ["Korgi": "Serega", "Taksa": "Den"]

let deleteDogName2 = dogName.removeValue(forKey: "Taksa") // удалили и вернули значение "Nikita"
//["Korgi": "Serega"]

// Удаляем целиком словарь
dogName.removeAll() // или dogName = [:]
```

#### 5.3 Множества

Множество - это неупорядоченная коллекция уникальных значений

```Swift
let set1 = Set<String>()
let set2: Set<String> = []
var set3: Set = [7,8,9]

set3.insert(10) // вставляем, вернет true, так как 10 нет в сете
set3.insert(7) // вставляем, вернет false, так как 7 есть в сете

print(set3.isEmpty) // false
print(set3.count) // 4
set3.remove(7) // [8,9,10]

set3.contains(9) // true (содержит ли)

let setOne: Set = [1, 2, 3]
let setTwo: Set = [3, 4, 5]

// объединили сеты и отсортировати по порядку
let AllValueSet = setOne.union(setTwo).sorted() // [1,2,3,4,5]

// пересечение сетов
let valuesSet = setOne.intersection(setTwo) // [3]

// все уникальные значение (те которые содержатся в одном, но не содержатся в другом)
let onlyNoDublicationValue = setOne.symmetricDifference(setTwo).sorted() //  [1,2,4,5]

// все элементы первого сета, которые не повторяются во втором
let substractedValArr = setOne.substracting(setTwo).sorted() // [1,2]
```

Небольшая задачка, которую можно решить с помощью множества

Оставить в массиве только уникальные элементы, т.е. убрать все дубликаты.

Решение
```Swift
let array = [1,3,2,1,1,1,3,2,3,3,2]
let unique = Array(Set(array)) // [ 1, 3, 2 ]
```


