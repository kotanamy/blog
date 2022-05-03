---
weight: 1
title: "Road to IOS/Swift dev part 5"
date: 2022-04-29
draft: false
author: "Kotanamy"
description: "Стартовые понятия Swift"

tags: ["Swift/IOS", "Tutorial"]
categories: ["Documentation"]

lightgallery: true
---

# Road to IOS/Swift dev part 5
## **Стартовые понятия Swift**

<br>

### 8. Функции

Функции - это самостоятельные фрагменты кода, решающие определенную задачу. Каждой функции присваивается уникальное имя, по которому ее можно идентифицировать и "вызвать" в нужный момент. Каждая функция в Swift имеет тип, описывающий тип параметров функции и тип возвращаемого значения. Тип функции можно использовать аналогично любым другим типам в Swift, т.е. одна функция может быть параметром другой функции либо ее результирующим значением. Функции также могут вкладываться друг в друга, что позволяет икапсулировать определенный алгоритм внутри локального контекста.

8.1. Проставя функция, ничего не принимающая и ничего не возвращающая

```Swift
func simpleFunc() -> () { // еще можно вот так: func simpleFunc() -> Void{} или func simpleFunc(){}
    print("Im func")
}

simpleFunc() // вызываем, выведе "Im func"
```

8.2. Функция, принимающая один параметр

```Swift
func oneParam(a: Int){
    a++
    print(a)
}

oneParam(a: 10) // 11
```

8.3. Функция, не принимающая параметров, но возвращающая значение

```Swift
func noParam() -> Int {
    return 10
}

let a = noParam()
print(a) // 10
```

8.4. Функция, принимающая несколько параметров и возвращающая значение

```Swift
func twoParam(a: Int, b: Int) -> Int{ // сложение (функция "add", так сказать)
    return a+b
}

let c = add(1,2)
print(c) // 3
```

8.5. Функция, принимающая массив в качестве параметра и использующая вложенную функцию для работы

```Swift
// название не каноничное (плохое), придерживаюсь общей концепции, назвал бы arrayCounter
func arrayParam(array: [Int]) -> Int{ // считает количество элементов массив
    var sum = 0

    for i in array {
        sum++
    }

    return sum
}

let a = arrayParam(array: [1,2,3,4,5,6,7,6,5,4,3,2,1,0,-1])
print(a) // 15
```

8.6. Функция, которая принимает переменное число параметров

```Swift
func moreParam(ofInt integers: Int...) -> Int { // ofInt - внешнее имя, используется ниже
    var sum = 0

    for integer in integers {
        sum += integer
    }

    return sum
}

let a = moreParam(ofInt: 1, 2, 3, 4, 5) // ниже - это вот ТУТ (^w^)
print(a) // 15
```

8.7. Имена параметров функции

```Swift
func nameParam(_: Int) -> Int { // _ - значит что параметр можно пропустить
    return 7
}
```

8.8. Функция в качестве возвращаемого значения

Ща будет огонь, но попробую разжевать супер подробно

```Swift
// Функция принимает Bool, а возвращает функцию (Int) -> Int
func WAT(missed: Bool) -> ((Int) -> Int) { 
    func missCountUp(input: Int) -> Int { return input + 1 } // Эти две функции создаем чтобы
    func missCountDown(input: Int) -> Int { return input - 1 } // вернуть их

    return missed ? missCountUp : missCountDown // если missed == true, то вернем missCountUp,
                                                // иначе missCountDown
}

var missedCount = 0
// Тут значит вот такие демоны творятся
// WAT(missed: Bool) возвращает одну из функций в зависимости от значения missed
// Допустим что значение missed = true
// тогда возвращаем функцию missCountUp(input: Int) -> Int
// которая в свою очередь принимает (input: Int)
// т.е. во вторых скобочках мы передаем параметры в missCountUp(input: Int) -> Int
// и вот уже missCountUp возвращает значение
missedCount = WAT(missed: true)(missedCount) // +1 
missedCount = WAT(missed: true)(missedCount) // +1
missedCount = WAT(missed: false)(missedCount) // -1
print(missedCount) // 1
```

### 9. Замыкания (Closure)

Что такое замыкания и с чем их едят?

**Замыкания** - это самодостаточные блоки с определенным функционалом, которые могут быть переданы и использованы в вашем коде. Замыкания могут захватывать и хранить ссылки на любые константы и переменные из контекста, в котором они объявлены. Эта процедура называется *заключение* этих констант и переменных, отсюда и название "замыкание". Swift выполняет всю работу с управлением памятью при захвате за вас.

Вот это closure, но это "нерабочий" пример :)
```Swift
{
    print("Hi closure!")
}
```
Поэтому присваиваем его константе
```Swift
let myClosure = {
    print("Hi closure!")
}

func repeatThreeTimes(closure: () -> ()) { // вызывает замыкание 3 раза
    for _ in 0...2 {
        closure()
    }
}

repeatThreeTimes(closure: myClosure) // вызываем myClosure

repeatThreeTimes(closure: {
    () -> () in     // явное указание типа замыкания
    print("Im closure to")
})

// а еще можно самому передать closure, как в первом "нерабочем" примере
// вот такое прокатывает
// называется последующее замыкание
repeatThreeTimes{
    () -> () in
    print("Im closure to")
}
```

Сортируем массив замыканием, по сути передаем алгоритм того как мы сортируем
```Swift
let unsortedArray = {123, 2, 14, 155, 1224, 5}

let sortedArray = unsortedArray.sorted {
    (number1: Int, number2: Int) -> Bool in
        return number1 < number2 // тут "алгоритм" сортировки
}
```


