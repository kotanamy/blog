---
weight: 1
title: "Road to IOS/Swift dev part 9"
date: 2022-05-03
draft: false
author: "Kotanamy"
description: "Стартовые понятия Swift"

tags: ["Swift/IOS", "Tutorial"]
categories: ["Documentation"]

lightgallery: true
---

# Road to IOS/Swift dev part 9 
## **Стартовые понятия Swift**

<br>

### 15. Наблюдатели свойств(property observers)

Наблюдатели свойств - это такие штуковины, которые наблюдают за изменениями значения свойств

willSet - будет изменено

didSet - было изменено

Само значение изменяется где-то между willSet и didSet

```Swift
class SecretLabEmployee {
    var accessLevel = 0 {
        willSet { // неявно willSet(newValue)
            print("willSet")
            print("new access level = \(newValue)")
        }
        didSet { // неявно didSet(oldValue) 
            if accessLevel > 0 {
                accessToDB = true
            } else {
                accessToDB = false
            }
            print("didSet, oldValue \(oldValue)")
        }
    }
    var accessToDB = false
}

let emp = SecretLabEmployee()
emp.accessLevel = 1
// выведет:
// willSet
// new access level = 1
// didSet, oldValue 0
print(emp.accessToDB) // true
```