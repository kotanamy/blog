---
weight: 1
title: "Road to IOS/Swift dev part 15"
date: 2022-05-31
draft: false
author: "Kotanamy"
description: "Стартовые понятия Swift"

tags: ["Swift/IOS", "Tutorial"]
categories: ["Documentation"]

lightgallery: true
---

Стартовые понятия Swift. Обработка ошибок. Subscripts. Расширения.

<!--more-->

# Road to IOS/Swift dev part 15
## **Стартовые понятия Swift**

<br>

### 24. Обработка ошибок (Error handling)

Сразу примерчик. Есть библиотека, в которой можно купить книги.

```Swift
// Перечисление подписанное под протокол Error
// для того чтобы генерировать ошибку
// (подробнее про протоколы в сл статьях)
enum PossibleErrors: Error {
    case notInStock
    case notEnoughtMoney
}

// Структура, если у нее не заполнены дефолтные поля,
// то она сама их заполняет дефолтными значениями, подробнее ниже
struct Book {
    let price: Int
    var count: Int
}

class Library {
    var deposit = 100
    var libraryBooks = ["Book1": Book(price: 10, count: 1),
        "Book2": Book(price: 11, count: 0), "Book3": Book(price: 12, count: 3)] // почленный инициализатор

    // этот метод будет генерировать ошибку
    // для этого нужно написать throws
    // throws пишется до стрелочки
    func getTheBook(withName: String) throws{ 
        // Продается ли в библиотеке такая книга?
        guard var book = libraryBooks(withName) else {
            throw PossibleErrors.notInStock // данной книги нет вналичии
        }

        // Есть ли хотя бы 1 книга вналичии?
        guard book.count > 0 else {
            throw PossibleErrors.notInStock
        }

        // Хватает ли у нас денег на ее покупку?
        guard book.price <= deposit else {
            throw PossibleErrors.notEnoughtMoney
        }

        // Если все хорошо, то:
        deposit -= book.price // платим деньги
        book.count -= 1 // уменьшаем количество книг вналичии
        libraryBooks[withName] = book // обновляем данные о книге в библиотеке
        print("You got the Book \(withName)") // выводим инфу пользователю
    }
}

let library = Library() 
// Эта запись сгенерирует ошибку
library.getTheBook(withName: "Book2") // Call can thow but not marked with 'try'

// Нужно писать через try
try? library.getTheBook(withName: "Book1") // You got the Book1

// использовать try! нужно только в том случае
// если 100% уверены что метод отработает

// обычный try используется для того чтобы
// его поместить в do catch блок
do { // если все впорядке, выполнится этот блок
    try library.getTheBook(withName: "Book2")
} catch PossibleErrors.notInStock { // ловим ошибку notInStock
    print("Book is not in stock")
} catch PossibleErrors.notEnoughtMoney { // ловим ошибку notEnoughtMoney
    print("not enought money")
}
// Выведет:
// Book is not in stock

// альтернативная запись для try? (опциональный try)
func doConnection() throws -> Int {
    return 10
}

// Случай 1
let x = try? doConnection()

// Случай 2
let y: Int?

do {
    y = try doConnection()
} catch {
    y = nil
}
// случаи записи 1 и 2 идентичны
```

**defer** - блок кода, который выполняется после прекращением работы функции. defer'ы выполняются в обратном порядке.

```Swift
var attempt = 0
func whareverFunc(param: Int) -> Int {
    defer { // выполнится 2ым
        attempt += 2
    }

    defer { // выполнится 1ым
        attempt *= 10
    }

    // defer выполнится после switch
    switch param {
        case 0: return 100
        case 1: return 200
        default: return 400
    }
}

let w = whateverFunc(param: 1) 
print(w) // 200
print(attempt) // 2
```

### 25. Subscripts

Subscripts - по сути index

```Swift
let array = [1, 2, 3, 4]
print(array[1]) // array[1] - subscript
```

```Swift
struct WorkPlace {

    var table: String
    var workPlace: [String]

    // Subscript
    subscript(index: Int) -> String? {
        get{
            switch index{
                case 0..<workPlace.count: return workPlace[index]
                case 1..<workPlace.count+1: return[index -1]
                default: return nil
            }
        }

        set {
            switch index {
                case 0: return table = newValue ?? ""
                case 1..<workPlace.count+1: return workPlace[index -1] = newValue ?? "" // заменяющий nil оператор
                default: break
            }
        }
    }
}

var work = WorkPlace(table: "T", workPlace: ["A", "B", "C"])
print(work.workPlace[1]) // B
// Subscript use
// 0-вой элемент - это T
print(work1[1]) // B
```

### 26. Расширения (Extentions)

Расширения используются для того чтобы вы могли сами расширить уже существующий функционал уже существубщего типа. 
_**Примечание.** Раширения не могут иметь свойства хранения, летивые свойства_

```Swift
extension Int { // расширение могут быть подписаны под протоколы
    // Четность числа
    var isEven: Bool {
        return self % 2 == 0 ? true : false
    }

    // Вычисляем степень
    func power(powerValue: Int) -> Int {
        car tempValue = self
        for _ in 1..<powerValue {
            tempValue *= self
        }

        return tempValue
    }
}

let a = 11

print(a.isEven) // false

let b = 2

print(a.power(powerValue: 3)) // 8
```
