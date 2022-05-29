---
weight: 1
title: "Road to IOS/Swift dev part 12"
date: 2022-05-23
draft: false
author: "Kotanamy"
description: "Стартовые понятия Swift"

tags: ["Swift/IOS", "Tutorial"]
categories: ["Documentation"]

lightgallery: true
---

Стартовые понятия Swift. Автоматический подсчет ссылок.

<!--more-->

# Road to IOS/Swift dev part 12
## **Стартовые понятия Swift**

<br>

### 21. Автоматический подсчет ссылок(ARC (Automatic Reference Counting))

**ARC** служит для отслеживания и управления памятью приложения. 

- **weak** - слабый захват ссылок, не удерживается замыканием и может быть освобожден и установлен в nil (Всегда optional)

- **unowned** - "бесхозные ссылки", альтернатива weak, разница в том, что НЕ является optional.


```Swift
class Person {
    var dog: Dog?

    deinit { // деинициализатор (деструктор) вызывается когда высвобождается память объекта
        print("Deinit Person")
    }
}

class Dog {
    var person: Person?

    deinit {
        print("Deinit Dog")
    }
}

// области видимости
let firstScope = true 
let secondScope = true

if firstScope{

    if secondScope{
        let person = Person()
        let dog = Dog()

        print("second end")
    }

    print("first end")
}

// Выведет:
// second end
// Deinit Dog
// Deinit Person
// first end
```

Даем перекрестные ссылки

```Swift
// Продолжаем
if firstScope{

    if secondScope{
        let person = Person()
        let dog = Dog()

        person.dog = dog
        dog.person = person

        print("second end")
    }

    print("first end")
}

// Выведет:
// second end
// first end
```

Память не высвободилась, так как ARC видит что на объект кто-то ссылается, а значит он еще нужен. Т.е. объект отпускается только в том случае, если колечество сильных ссылок равно нулю. Чтобы поправить ситуацию есть операторы weak и unowned. Напишем новый класс WeakPerson.

```Swift
// Продолжаем
class WeakPerson {
    weak var dog: Dog?

    deinit { 
        print("Deinit WeakPerson")
    }
}

if firstScope{
    let weakPerson = WeakPerson() // еще в этом примере поиграем 
    let dog = Dog()              // в первой области видимости

    if secondScope{
        weakPerson.dog = dog
        dog.person = weakPerson

        print("second end")
    }

    print("first end")
} // после завершения второй области видимости
 // освободится память, т.е. туть

// Выведет:
// second end
// first end
// Deinit Dog
// Deinit WeakPerson
```

Теперь про unowned. Сразу примерчик и в этот раз мучаем собаку.

```Swift
class UnownedDog {
    unowned var person: Person // обратите внимание что здесь мы ОБЯЗАНЫ
                            // задать значение person
    
    // Поэтому пишем инициализатор
    init(){
        self.person = Person()
    }

    deinit {
        print("Deinit UnownedDog")
    }
}

if firstScope{
    let person = Person() 
    let unownedDog = UnownedDog()              

    if secondScope{
        person.dog = dog
        unownedDog.person = person

        print("second end")
    }

    print("first end")
}

// Выведет:
// Deinit Person
// second end
// first end
// Deinit Person
// Deinit Dog
```

В это примере 2 раза освобождается память (Deinit Person), так как первый раз мы создаем объект Person при инициализации Dog, а второй раз в первой области видимости. И получается что первый раз деинициализатор Person срабатывает внутри второй области видимости.
