---
title: 'Python Class variable'
date: 2014-11-19T10:19:00+08:00
draft: false
categories: [python]
---
標題下的不好，因為不知道該怎麼說。類別裡宣告變數以後，之後方法裡，會怎麼去認定，為了搞清楚這點，就寫了小程式。

```
class Animal(object):
    NAME = "Animal"

    def __init__(self):
        print(Animal.NAME)

    def bark(self):
        print("Bark(1): " + Animal.NAME)
        print("Bark(2): " + self.NAME)


animal = Animal()
animal.bark()

animal2 = Animal()
animal2.NAME = "Planet"
animal2.bark()
```

結果：
```
Animal
Bark(1): Animal
Bark(2): Animal
Animal
Bark(1): Animal
Bark(2): Planet
```

依據結果的解讀是這樣，當用 Animal 宣告時，每個 instance 都會有一個 NAME，所以在變更 animal2.NAME 以後，bark 裡的 self.NAME 實際上就是變動後的內容 "Planet"；但如果用 Animal.NAME 就還是原來的 "Animal"。
