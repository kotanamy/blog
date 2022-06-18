---
weight: 1
title: "Road to IOS/Swift dev part 14"
date: 2022-05-30
draft: false
author: "Kotanamy"
description: "Стартовые понятия Swift"

tags: ["Swift/IOS", "Tutorial"]
categories: ["Documentation"]

lightgallery: true
---

Стартовые понятия Swift. Опциональные цепочки.

<!--more-->

# Road to IOS/Swift dev part 14
## **Стартовые понятия Swift**

<br>

### 23. Опциональные цепочки (Optional chaining)

Опциональные цепочки - это цепочки опционалов (КЭП).

Чтобы что-то получить, нужно чтобы несколько факторов не были nil.

Пример. Вы спешите на встречу, вам нужно чтобы вы быстро вызвали такси, не попали в пробку и в пути не собрали все красные светофоры. Поехали

```Swift
class Person {
    let job: Job? = Job() // работа
    let workers: [Worker]> = [Worker()] // сотрудники (начальник походу)
}

class Worker { // работник
    let name = "Name"

    func work() {
        perint("Im chill")
    }
}

class Job { // работа
    let salary: Salary? = Salary() // получка (зарплата)
}

class Salary {
    let salary = 300000 // в долларах в наносекунду 

    func showSalary() -> String {
        return "\(salary)"
    }
}

let person = Person()

if let job = person.job() {
    if let salary = job.salary {
        let poluchka = salary.salary
    }
}

// эквивалентная запись if let'ов выше
let poluchka1 = person.job?.salary?.salary

// создаем массив работников
var workersArray = person.workers
workersArray?.append(Worker()) // здесь ? потому что мы можем выполнить
                            // это действие лишь в том случае
                            // если workersArray есть!

for w in workersArray{
    print(w.name) // Name Name
}
```
```
                  | hi im Elvice |
Элвис             |__   _________|
                     \/ 
                     .?
                    <()>
                     /\

Пы.Сы. обещанный Элвис
```