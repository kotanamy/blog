---
weight: 1
title: "Road to IOS/Swift dev part 19"
date: 2022-08-08
draft: false
author: "Kotanamy"
description: "Стартовые понятия Swift"

tags: ["Swift/IOS", "Tutorial"]
categories: ["Documentation"]

lightgallery: true
---

Стартовые понятия Swift. GCD, Async\Await, OperationQueue

<!--more-->

# Road to IOS/Swift dev part 19
## **Стартовые понятия Swift**

<br>

### 29.1. **GCD**

**GCD** (Grand Central Dispatch) - фреймворк, позволяющий работать с потоками.

О GCD написано очень много всего интересного, поэтому здесь соберу просто несколько материалов:
-   [TheSwiftDeveloper](https://youtube.com/playlist?list=PLmTuDg46zmKCKjZqxXqJFjGXwzqGQi4D3)
-   [Статья на хабре](https://habr.com/ru/post/578752/)
-   [Raywenderlich](https://www.raywenderlich.com/5370-grand-central-dispatch-tutorial-for-swift-4-part-1-2/)
-   [Видосик](https://youtu.be/uEeFqlUXJcE) для проверки знаний

Еще можно почитать SwiftBook или Apple доку


### 29.2. **Operation Queue**

Постановка задачи:

Готовим обед, нам нужно почистить овощи, чистка каждого овоща - независимый процесс, чистить овощи можно независимо друг от друга, т.е. асинхронно, но перейти к приготовлению конкретного блюда мы сможем лишь почистив все овощи, т.е. завершив все операции.

У нас, получается, есть несколько независимых процессов, однако их результаты могут быть использованны после завершения всех процессов.

Operation представляет собой законченную задачу и является абстрактным классом, который предоставляет вам потока-безопасную структуру для моделирования состояния операции, ее приоритета, зависимостей(dependencies) от других Operations и управления этой операцией.

OperationQueue (очередь операций) вызывает свои объекты (Operation) в очереди на основе их приоритета и готовности. После добавления операции в очередь она остается в очереди до тех пор, пока операция не завершит свою задачу. Вы не можете напрямую удалить операцию из очереди после ее добавления.

Особенности:
Очереди операций сохраняет в памяти все операции до их завершения, при этом сами очереди сохраняются в памяти до завершения всех операций. Приостановка очереди операций с незавершенными операциями может привести к утечке памяти.

```Swift
class Vegetable {
    let id: Int
    var alreadyChopped = false

    init(id: Int){
        self.id = id
    }

    func chopped(completion: @escaping((Vegetable) -> Void)) {
        print("Start \(id)")
        alreadyChopped = true
        Thread.sleep(forTimeInterval: 2)
        print("End \(id)")
        completion(self)
    }
}

func gatherRawVeggies() -> [Vegetable] {
    [Vegetable(id: 1), Vegetable(id: 2)]
}

// Последовательность операций по чистке овощей
// В этом методе будем создавать очередь процессов
// добавлять блоки операций для каждого овоща
// и реализуем приостановку очереди для синхронизации всех операций
func chopVegetables(completion: @escaping (([Vegetable]) -> Void)) {
    let rawVeggies: [Vegetable] = gatherRawVeggies() // овощи которые надо почистить
    var choppedVeggies: [Vegetable] = [] // почищенные овощи

    // вот так создается очередь операций
    let queue = OperationQueue()

    // Для каждого овоща (элемента массива)
    for rawVeggie in rawVeggies {
        // Создаем отдельный блок операции 
        let operation = BlockOperation {
            // Очищаем овощ
            rawVeggie.chopped { choppedVeggie in // completion раскрываем
                // добавляем в массив очищенных овощей
                choppedVeggies.append(choppedVeggie)
            }
        }
        // добавляем каждый отдельный блок
        // в очередь операций
        queue.addOperation(operation)
    }
    // Чтобы синхронизировать выполнение операции
    // используем Barrier Block
    // он приостанавливает выполнение очередного блока в очереди
    // для завершения ранее добавленных операций
    
    // Получается мы ждем пока дочистится последний овощ
    queue.addBarrierBlock {
        // Возвращаем его через completion
        completion(choppedVeggies)
    }
}

// Пробуем вызвать
chopVegetables { veggies in
    for veggie in veggies {
        print("\(veggie.id) is chopped \(String(veggie.chopped))")
    }
}

// Второй овощ не очитится
// будет утечка памяти
```

Подробнее о Operation и OperationQueue можно прочитать в [этой](https://habr.com/ru/post/335756/) статье. А так же, конечно, можно сунуться в официальную документацию Apple.

### 29.3. **Async\Await**

Решим ту же задачу, что и в пункте 29 (Operation Queue) с помощью Async/Await

```Swift

class Vegetable {
    let id: Int
    var alreadyChopped = false

    init(id: Int){
        self.id = id
    }

    // Этот метод должен выполняться ассинхронно
    // поэтому помечен как async
    // И этот метод может ничего не вернуть (сгенерировать ошибку) (throws)
    // вызывать его надо с "try"
    func chopped() async throws -> Vegetable {
        print("Vegetable \(id) starts chop")
        alreadyChopped = true
        Thread.sleep(forTimeInterval: 2)
        print("End \(id)")
        return self
    }
}

func gatherRawVeggies() -> [Vegetable] {
    [Vegetable(id: 1), Vegetable(id: 2)]
}

// Метод очистки овощей
// тоже ассинхронный
// Возвращаем набор очищенных овощей
func chopVegetable() async throws -> [Vegetable] {
    let rawVeggies: [Vegetable] = gatherRawVeggies() // овощи которые надо почистить
    var choppedVeggies: [Vegetable] = [] // почищенные овощи

    // Чистим овощи
    for rawVeggie in rawVeggies {
        // await используется для вызова async функций
        choppedVeggies.append(try await rawVeggie.chopped()) 
    }
    // возвращаем почищенные овощи
    return choppedVeggies
}

// асинхронные методы вызываем вот так
async {
    let veggies = try await chopVegetable()
    for veggie in veggies {
        print("\(veggie.id) is chopped \(String(veggie.chopped))")
    }
}

// Все четко отработает
// Все почистим
```

Подробнее о Async/Await можно прочитать в [этой](https://habr.com/ru/company/citymobil/blog/571360/) или [этой](https://www.avanderlee.com/swift/async-await/) статьях. А так же, конечно, можно окунуться в оф документацию Apple.
