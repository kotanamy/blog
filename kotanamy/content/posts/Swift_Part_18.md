---
weight: 1
title: "Road to IOS/Swift dev part 18"
date: 2022-06-15
draft: false
author: "Kotanamy"
description: "Стартовые понятия Swift"

tags: ["Swift/IOS", "Tutorial"]
categories: ["Documentation"]

lightgallery: true
---

Стартовые понятия Swift. Функции высшего порядка (map, mapValues, filter, reduce, flatMap, zip).

<!--more-->

# Road to IOS/Swift dev part 18
## **Стартовые понятия Swift**

<br>

### 28. **Функции (методы) высшего порядка (map, mapValues, filter, reduce, sorted, flatMap, zip, allSatisfy)**

**Функция высшего порядка** - это функция, которая принимает другую функцию в качестве аргумента.

Еще одно определение.

**Функция высшего порядка** - это просто функция, которая работает с другими функциями, либо принимая функцию в качестве аргумента, либо возвращая функцию

#### **28.1 map(_:)**

Данный метод позволяет применить переданное в него замыкание для каждого элемента коллекции. В результате его выполнения возвращается новая последовательность, тип элементов которой может отличаться от типа исходных элементов

```Swift
var arr = [2, 4, 5, 7]
var newArr = myArray.map{$0}
print(newArr) // [2, 4, 5, 7]
```

Метод map(_:) принимает замыкание и применяет его к каждому элементу массива arr. Переданное замыкание {$0} не производит каких-либо действий над элементами, поэтому результат, содержащийся в переменной newArr, не отличается от исходного.

На счет {$0} чтобы было понятно, разберу на примере

```Swift
var arr = [1,2,3]
let newArr = array.map({
    (value : Int) -> Int in
    return value
})

// Оптимизируем
let newArr2 = arr.map{ value in value }

// $0 - это название по умолчанию
// поэтому можно еще сократить
let newArr3 = arr.map{ $0 }
```

Еще несколько примеров примеров:
1. Выводим квадрат
```Swift
var arr = [2, 4, 5, 7]
var newArr = myArray.map{$0 * $0}
print(newArr) // [4, 16, 25, 49]
```
2. Выведем массив Bool, условие > 2
```Swift
var arr = [1, 2, 3, 4]
var newArr = myArray.map{$0 > 2}
print(newArr) // [true, true, false, false]
```
3. map может быть применен к словарям
```Swift
let milesToDest = ["Moscow":120.0, "Dubai":50.0, "Paris":70.0]
var kmToDest = milesToDest.map { name, miles in
    [name : miles * 1.6093] 
}
print(kmToDest) // ["Dubai":80.465, "Paris":112.651, "Moscow":193.116]
```

#### **28.2 mapValues(_:)**

В случае, когда нам необходимо обработать все значения элементов словаря, то применение метода map(_:) может привести к неожиданному результату. Несмотря на то что новая коллекция будет сформирована, ее тип не будет соответствовать типу исходного словаря.

```Swift
mappedCloseStars = ["Proxima Centauri": 4.24, "Alpha Centauri A" 4.27, "Alpha Centauri B": 5.37]
let newCollection = mappedCloseStars.map{$0}
print(type(of: newCollection)) // Array<(key: String, value: Double)>.Type
```

Работать с массивом типа Array<(key: String, value: Double)> не очень удобно, так что поховем на помощь mapValues(_:). Он поочередно применяет переданное ему замыкание к значению каждого элемента словаря.

```Swift
mappedCloseStars = ["Proxima Centauri": 4.24, "Alpha Centauri A" 4.27, "Alpha Centauri B": 5.37]
let newCollection = mappedCloseStars.mapValues{ $0 + 1 }
print(newCollection) // ["Proxima Centauri": 5.24, "Alpha Centauri A" 5.27, "Alpha Centauri B": 6.37]
```

#### **28.3 filter(_:)**

**Метод filter(_:) используется, когда требуется отфильтровать элементы коллекции по определенному правилу.

```Swift
let numbers = [1, 4, 10, 15]
// найдем все четные числа
let even = numbers.filter{ $0 % 2 == 0 }
print(even) // [4, 10]
```

Помимо массивов, вы можете производить фильтрацию и других типов коллекций.

Пример для словаря.
```Swift
mappedCloseStars = ["Proxima Centauri": 4.24, "Alpha Centauri A" 4.27, "Alpha Centauri B": 5.37]
let newCollection = mappedCloseStars.filter{ $0 > 5.0 }
print(newCollection) // ["Alpha Centauri B": 5.37]
```

#### **28.4 reduce(_:_:)**

Метод reduce(_:_:) позволяет объеденить все элементы коллекции в  одно значение в соответствии с переданным замыканием. Помимо самих элементов, метож принимает первоначальное значение, которое служит для выполнения операции с первым элементом коллекции.

Пример, у вас на карте лежит 210 рублей и вам надо положить на нее купюры 10, 50, 100, 500
```Swift
let cash = [10, 50, 100, 500]
// первый аргумент - начальное значение, второй - замыкание
let total = cash.reduce(210, +) // 870
```

Еще пример
```Swift
let cash = [10, 50, 100, 500]
// 210 * 10 * 50 * 100 * 500
let total = cash.reduce(210, { $0*$1 }) // 5250000000
// 210 - 10 - 50 - 100 - 500
let totalTwo = cash.reduce(210, { x,y in x - y}) // -450
```

#### **28.5 sorted(_:)**

Если вызвать sorted() у массива, то он вернет новый массив, отсортированный по возрастанию. Но чтобы это случилось, элементы массива должны реализовавывать протокол **Comparable**.
```Swift
let num : [Int] = [0, 4, 1, 5, 3]
let numSort = num.sorted()

print(num) // [0, 4, 1, 5, 3]
print(numSort) // [0, 1, 3, 4, 5]
```

Если вы хотите точно указать как сортировать массив, то нужно использовать функцию высшего порядка.
```Swift
num.sorted(by: (Int, Int) -> Bool)
```

Сортируем в порядке убывания
```Swift
let num : [Int] = [0, 4, 1, 5, 3]
let numSortDown = num.sorted{ (a, b) -> Bool in
    return a > b
}

print(numSortDown) // [5, 4, 3, 1, 0]
```

Теперь магия. Внимательно смотрите за руками!

Перепишем пример выше с использованием высокоуровневой магии.
```Swift
let num : [Int] = [0, 4, 1, 5, 3]
let numSortDown = num.sorted(by: >) // руки здесь

print(numSortDown) // [5, 4, 3, 1, 0]
```

Раскрытие магии. Залезаем под капот (кликаем по минусам)
```Swift
public func >(lhs: Int, rhs: Int) -> Bool
```

#### **28.6 flatMap(_:)**
**
Метод flatMap(_ :) отличается от map(_ :) тем, что всегда возвращает плоский одномерный массив.

```Swift
let numbers = [1, 2, 3, 4]
let flatMapped = numbers.flatMap{ Array(repeating: $0, count: $0) }
print(flatMapped) // [ 1, 2, 2, 3, 3, 3, 4, 4, 4, 4 ]
```

Ищем в многомерном массиве все попадающие под условие значения

```Swift
let numbers = [[1, 2, 3, 4, 5], [11, 44, 1, 6], [16, 403, 321, 10]]
let flatMapped = numbers.flatMap{ $0.filter{ $0 % 2 == 0} }
print(flatMapped) // [ 2, 4, 44, 6, 16, 10 ]
```

#### **28.7 zip(_ : _ :)**

Функция zip(_ : _ :) предназначена для формирования последовательности пар значений, каждая из которых составлена из элементов двух базовых последовательностей.

```Swift
let c1 = [1, 2, 3]
let c2 = [4, 5, 6]
var zipS = zip(c1, c2) // Zip2Swquence<Array<Int>, Array<Int>>

// генерация массива из сформированной последовательности
Array(zipS) // [(.0 1, .1 4), (.0 2, .1 5), (.0 3, .1 6)]

// генерация словаря на основе последовательности пар значений
let newDict = Dictionary(uniqueKeysWithValues: zipS) // [2: 5, 3: 6, 1: 4]
```

#### **28.7 allSatisfy()**

Проверяет, все ли элементы коллекции соответствуют заданному условию

```Swift
let arr = [10, 20, 30, 5, 8]

let answer = arr.allSatisfy{ $0 >= 15 }
print(answer) // false

let answer2 = arr.allSatisfy{ $0 <= 40 }
print(answer) // true
```