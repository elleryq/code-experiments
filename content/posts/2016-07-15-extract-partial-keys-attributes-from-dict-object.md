---
title: 'Extract partial keys/attributes from dict/object'
date: 2016-07-15T09:25:00+08:00
draft: false
categories: [python]
---
利用 dictionary comprehension 很輕易的就可以辦到這件事情。
參考自：http://stackoverflow.com/questions/5352546/best-way-to-extract-subset-of-key-value-pairs-from-python-dictionary-object

```
from datetime import datetime


def getattrs(instance, attrs=[]):
    if isinstance(instance, dict):
        return { attr: instance[attr] for attr in attrs if attr in instance}
    elif isinstance(instance, object):
        return { attr: getattr(instance, attr) for attr in attrs }
    raise Exception("Not object or dict")

    
class Book(object):
    def __init__(self, title, author, created=datetime.now()):
        self.title = title
        self.author = author
        self.created = created
        
    def __str__(self):
        return "{title} - {author}".format(title=self.title, author=self.author)

    def __repr__(self):
        return self.__str__()

    
books = [Book("snow white", "unknown"), Book("beauty and the beast", "unknown"), Book("The lord of the rings", "Torking")]
print(books)

simplified = [getattrs(book, ['title', 'author']) for book in books]
print(simplified)

more_simplified = [getattrs(x, ['title']) for x in simplified]
print(more_simplified)
```