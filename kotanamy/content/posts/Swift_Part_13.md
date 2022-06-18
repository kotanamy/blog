---
weight: 1
title: "Road to IOS/Swift dev part 13"
date: 2022-05-29
draft: false
author: "Kotanamy"
description: "Стартовые понятия Swift"

tags: ["Swift/IOS", "Tutorial"]
categories: ["Documentation"]

lightgallery: true
---

Стартовые понятия Swift. ARC в замыканиях.

<!--more-->

# Road to IOS/Swift dev part 13
## **Стартовые понятия Swift**

<br>

### 22. ARC в замыканиях (ARC in closures)

Замыкания умеют захватывать значения. Сразу примерчик:

```Swift
var a = "a"

// closure захватило не значение переменной 'a',
// а ссылку на саму 'a'
let closure: () -> () = {
    print(a)
}

closure() // a

x = "b"

closure() // b
```

Как избежать захвата ссылки?

```Swift
var a = "a"

// Вот здесь захватывается значение
let closure = { [a]() -> () in
    print(a)
}

closure() // a

x = "b"

closure() // a
```
Пам-пам, все готово.

Теперь рассмотрим следующий примерчик.
```Swift
class Person {
    var dog: Dog?

    init(){
        // создаем собаку и пихаем
        // в нее этого человека
        self.dog = Dog(person: self) 
    }

    deinit { // деинициализатор (деструктор) вызывается когда высвобождается память объекта
        print("Deinit Person")
    }
}

class Dog {
    unowned var person: Person

    init(person: Person){
        self.person = person
    }

    deinit {
        print("Deinit Dog")
    }
}

let closure1: () - ()?

if true {
    print("scope start")

    let person = Person()
    let dog = person.dog

    closure1 = {
        print(dog)
    }

    print("scope ended")
}

print("All task ended")

// Вывод:
// scope start
// scope ended
// Deinit Person
// All task ended
```

Почему ARC не отпускает собаку? Потому что ее держит closure1, который создан во "внешней" области видимости. Если просто переписать closure1 как:
```Swift
closure1 = { [dog] in
    print(dog)
}
```
То собаку все равно никто не отпустить. Поэтому, нужно сделать слабой ссылку на собаку:
```Swift
closure1 = { [weak dog] in // при unowned будет ошибка, так как optional dog
    print(dog)
}
```
Теперь все работает как надо!

Еще пара "типов" сильных ссылок. Напишем новый класс Person1

```Swift
class Person1 {
    var dog: Dog?
    var closure2: (() -> ())?

    init(){
        self.dog = Dog(person: self) 
    }

    deinit {
        print("Deinit Person1")
    }
}
```

```Swift
if true {
    print("scope start")

    let person = Person1()
    let dog = person1.dog

    person.closure2 = {
        print("person")
    }

    print("scope ended")
}

print("All task ended")

// Вывод:
// scope start
// scope ended
// All task ended
```
Не отпустился person :(

Укажем лист захвата

```Swift
if true {
    print("scope start")

    let person = Person1()
    let dog = person1.dog

    person.closure2 = { [unowned person] in
        print(person)
    }

    print("scope ended")
}

print("All task ended")

// Вывод:
// scope start
// scope ended
// Deinit Person1
// Deinit Dog
// All task ended
```

И последний вариант сильных ссылок.

```Swift
class Person2 {
    var dog: Dog?
    var closure2: (() -> ())?
    lazy var property: (Bool) -> (Bool) = { (param: Bool) -> Bool in
        print(self.dog)
        return true
    }

    init(){
        self.dog = Dog(person: self) 
    }

    deinit {
        print("Deinit Person1")
    }
}

if true {
    print("scope start")

    let person = Person2()
    let dog = person.dog

    person.property(true)

    print("scope ended")
}

print("All task ended")

// Вывод:
// scope start
// scope ended
// All task ended
```

Опять не отпускается. Что делать? То же, что и всегда, Пинки.


```Swift
class Person3 {
    var dog: Dog?
    var closure2: (() -> ())?
    lazy var property: (Bool) -> (Bool) = { [unowed self](param: Bool) -> Bool in // туть
        print(self.dog)
        return true
    }

    init(){
        self.dog = Dog(person: self) 
    }

    deinit {
        print("Deinit Person1")
    }
}

if true {
    print("scope start")

    let person = Person3()
    let dog = person.dog

    person.property(true)

    print("scope ended")
}

print("All task ended")

// Вывод:
// scope start
// scope ended
// Deinit Person1
// Deinit Dog
// All task ended
```