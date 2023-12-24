
>copy from https://developer.ibm.com/tutorials/ba-metaprogramming-python/


### [Analytics](https://developer.ibm.com/technologies/analytics/)

Tutorial

# Metaprogramming in Python

Simplifying tasks

By Satwik Kansal  
Published 2018年4月4日

---

Just like metadata is data about data, metaprogramming is writing programs that manipulate programs. It's a common perception that metaprograms are the programs that generate other programs. But the paradigm is even broader. All of the programs designed to read, analyze, transform, or modify themselves are examples of metaprogramming. Some examples include:

- Domain-specific languages (DSLs)
- Parsers
- Interpreters
- Compilers
- Theorem provers
- Term rewriters

This tutorial explores metaprogramming in Python. It refreshes your Python knowledge with a review of Python's features so that you can better understand the concepts in this tutorial. It also explains how type in Python has more significance than just to return the class of an object. Then, it discusses ways of metaprogramming in Python and how metaprogramming can simplify certain tasks.

## A bit of introspection

If you've been programming in Python for a while, you might know that everything is an object, and classes create objects. But if everything is an object (and classes are also objects), who creates those classes? This is exactly the question that I answer.

Let's verify if the previous statements are even correct:

```
>>> class SomeClass:
...     pass
>>> someobject = SomeClass()
>>> type(someobj)
<__main.SomeClass instance at 0x7f8de4432f80>
```

So, the `type` function called on an object to return the class of that object.

```
>>> import inspect
>>>inspect.isclass(SomeClass)
True
>>>inspect.isclass(some_object)
False
>>>inspect.isclass(type(some_object))
True
                
```

`inspect.isclass` returns `True` if a class is passed to it and `False` otherwise. Because `some_object` is not a class (it's an instance of class `SomeClass`), it returns `False`. And because `type(some_object)` returns the class of `some_object`, `inspect.isclass(type(some_object))` returns `True`:

```
>>> type(SomeClass)
<type 'classobj'>>>>
inspect.isclass(type(SomeClass))
True
```

`classobj` is the class that every class in Python 3 inherits from by default. Everything makes sense now. But what about `classobj`? Let's spice things up:

```
>>> type(type(SomeClass))
<type 'type'>
>>>inspect.isclass(type(type(SomeClass)))
True
>>>type(type(type(SomeClass)))
<type 'type'>
>>>isclass(type(type(type(SomeClass))))
True
```

Inception, huh? Turns out the very first statement (that everything is an object) wasn't completely true. Here's a better statement:

Everything is an object in Python, and they are all either instances of classes or instances of metaclasses, except for type.

To verify this:

```
>>> some_obj = SomeClass()
>>> isinstance(some_obj,SomeClass)
True
>>> isinstance(SomeClass, type)
True
```

So now we know that an instance is an instantiation class, and class is an instance of a metaclass.

### type is not what we think it is

`type` is itself a class, and it is its own type. It's a metaclass. A metaclass instantiates and defines behavior for a class just like a class instantiates and defines behavior for an instance.

`type` is the built-in metaclass Python uses. To change the behavior of classes in Python (like the behavior of `SomeClass`), we can define a custom metaclass by inheriting the `type` metaclass. Metaclasses are a way to do metaprogramming in Python.

#### Here's what happens when a class is defined

Let's first brush up on what we already know. The basic building blocks of a Python program are:

- Statements
- Functions
- Classes

Statements perform actual work in a program. Statements can execute at global scope (module level) or local scope (enclosed within a function). Functions are fundamental-like units of code that consist of one or more statements ordered in a way to accomplish a specific task. Functions can be defined either at module level or as a method of classes. Classes provide a way for object-oriented programming to function. They define how objects are going to be instantiated and their characteristics (attributes and methods).

The namespaces of classes are layered as dictionaries. For example:

```
 
>>> class SomeClass:
...     classvar = 1
...     def init(self):
...         self.somevar = 'Some value'

>>> SomeClass.dict
{'doc': None,
 'init': <function main.init>,
 'module': 'main',
 'class_var': 1}

>>> s = SomeClass()

>>> s.__dict
{'some_var': 'Some value'}
```

Here's what happens whenever the keyword class is encountered:

- The body (statements and functions) of the class is isolated.
- The namespace dictionary of the class is created (but not populated yet).
- The body of the class executes, then the namespace dictionary is populated with all of the attributes, methods defined, and some additional useful info about the class.
- The metaclass is identified in the base classes or the metaclass hooks (explained later) of the class to be created.
- The metaclass is then called with the name, bases, and attributes of the class to instantiate it.

And because `type` is the default metaclass in Python, you can use `type` to create classes in Python.

#### The other side of type

`type`, when called with one argument, produces the `type` information of an existing class. `type` called with three arguments creates a new class object. The arguments, when invoking `type`, are the name of the class, a list of base classes, and a dictionary giving the namespace for the class (all the fields and methods).

So:  
`>>> class SomeClass: pass`

Is equivalent to:  
`>>> SomeClass = type('SomeClass', (), {})`

And:

```
class ParentClass:
    pass

class SomeClass(ParentClass):
    some_var = 5
    def some_function(self):
        print("Hello!")
```

Is effectively equivalent to:

```
def some_function(self):
    print("Hello")

ParentClass = type('ParentClass', (), {})
SomeClass = type('SomeClass',
                 [ParentClass],
                 {'some_function': some_function,
                  'some_var':5})
```

zo, by using our custom metaclass instead of `type`, we can inject some behavior to the classes that wouldn't have been possible. But before we go into implementing metaclasses for altering behavior, let's see more common ways of metaprogramming in Python.

#### Decorators: A common example of metaprogramming in Python

Decorators are a way to change behaviors of a function or a class. The usage of decorators appears somewhat like this:

```
@some_decorator
def some_func(args, *kwargs):
    pass
```

==`@some_decorator` is just the syntactic sugar to represent that `some_func` is wrapped by another function `some_decorator`==. We know that functions, as well as classes (excluding the type `metaclass`), are objects in Python, which means they can be:

- Assigned to a variable
- Copied
- Passed as parameters to other functions

The previous syntactic is effectively equivalent to:  
`some_func = some_decorator(some_func)`

You might be wondering how `some_decorator` is defined:

```
def some_decorator(f):
    """
    The decorator receives function as a parameter.
    """
    def wrapper(args, **kwargs):
        #doing something before calling the function
        f(args, **kwargs)
        #doing something after the function is called
    return wrapper
```

Let's imagine we have a function fetching scraped data from a URL. The server from where we are fetching the data has a throttling mechanism if it detects a lot of requests coming from an IP address with the same interval. So to make our scraper human-like, we'd like to wait for some random amount of time before submitting the request to trick the server. Can we make a decorator that does that? Let's see:

```
from functools import wraps
import random
import time

def wait_random(min_wait=1, max_wait=30):
    def inner_function(func):
        @wraps(func)
        def wrapper(args, **kwargs):
            time.sleep(random.randint(min_wait, max_wait))
            return func(args, **kwargs)

        return wrapper

    return inner_function

@wait_random(10, 15)
def function_to_scrape():
    #some scraping stuff
```

The `inner_function` and `@wraps` decorator might be new to you. If you look carefully, the `inner_function` is analogous to the `some_decorator` we just previously defined. The other layer of wrapping in `wait_random` enables us to pass parameters to the decorator (`min_wait` and `max_wait`) as well. The `@wraps` is a nice decorator that copies the metadata of the `func` (like name, doc string, and function attributes). If we don't use these, we will not be able to get useful results from function calls like `help(func)` because in that case, it returns the docstring and information of `wrapper` instead of `func`.

But what if we have a `scraper` class with multiple such functions:

```
class Scraper:
    def func_to_scrape_1(self):
        #some scraping stuff
        pass
    def func_to_scrape_2(self):
        #some scraping stuff
        pass
    def func_to_scrape_3(self):
        #some scraping stuff
        pass
```

One option is to wrap all the functions with `@wait_random` individually. But we can do better: We can create a class decorator. The idea is to walk through the class namespace, identify the functions, and wrap them with our decorator.

```
def classwrapper(cls):
    for name, val in vars(cls).items():
        #callable return True if the argument is callable
        #i.e. implements the __call
        if callable(val):
            #instead of val, wrap it with our decorator.
            setattr(cls, name, wait_random()(val))
    return cls
```

Now you can wrap the entire class with `@classwrapper`. But what if there are multiple scraper classes or multiple subclasses of the `scraper`? You can either use `@classwrapper` on them individually or in such a scenario, you can create a metaclass.

#### Metaclasses

Writing a custom metaclass involves two steps:

1. Write a subclass of the metaclass type.
2. Insert the new metaclass into the class creation process using the metaclass hook.

We subclass the type class and modify the magic methods like [`__init__`](https://docs.python.org/2/reference/datamodel.html#object.__init__), [`__new__`](https://docs.python.org/2/reference/datamodel.html#object.__new__), [`__prepare__`](https://docs.python.org/3/reference/datamodel.html#preparing-the-class-namespace), and [`__call__`](https://docs.python.org/2/reference/datamodel.html#object.__call__) to modify the behavior of the classes while creating them. These methods have information like base class, name of the class, attributes, and their values. In Python 2, the metaclass hook is a static field in the class called `__metaclass__`. In Python 3, you can specify the metaclass as a `metaclass` argument in the base-class list of a class.

```
>>> class CustomMetaClass(type):
...     def __init__(cls, name, bases, attrs):  
...         for name, value in attrs.items():
                # do some stuff
...             print('{} :{}'.format(name, value))
>>> class SomeClass:
...          # the Python 2.x way
...         __metaclass__ = CustomMetaClass
...         class_attribute = "Some string"
__module__ :__main__
__metaclass__ :<class '__main__.CustomMetaClass'>
class_attribute :Some string
```

	The attributes get printed automatically due to the print statement in the `__init__` method of our `CustomMetaClass`. Let's imagine you have an annoying collaborator on your Python project who prefers using `camelCase` for naming class attributes and methods. You know it's bad, and the collaborator should be using `snake_case` (after all, it's Python!). Can we write a metaclass to change all those camelCase attributes to snake_case?

```
def camel_to_snake(name):
    """
    A function that converts camelCase to snake_case.
    Referred from: https://stackoverflow.com/questions/1175208/elegant-python-function-to-convert-camelcase-to-snake-case
    """
    import re
    s1 = re.sub('(.)([A-Z][a-z]+)', r'\1_\2', name)
    return re.sub('([a-z0-9])([A-Z])', r'\1_\2', s1).lower()

class SnakeCaseMetaclass(type):
    def __new__(snakecase_metaclass, future_class_name,
                future_class_parents, future_class_attr):
        snakecase_attrs = {}
        for name, val in future_class_attr.items():
            snakecase_attrs[camel_to_snake(name)] = val
        return type(future_class_name, future_class_parents,
                    snakecase_attrs)
```

You might have wondered why we use `__new__` instead of `__init__` here. `__new__` is actually the first step in creating an instance. It is responsible for returning a new instance of your class. `__init__`, on the other hand, doesn't return anything. It's only responsible for initializing the instance after it's been created. A simple rule of thumb to remember: Use `new` when you need to control the creation of a new instance; use `init` when you need to control the initialization of a new instance.

You won't often see `__init__` being implemented in a metaclass because it's not that powerful — The class is already constructed before `__init__` is actually called. You can see it as having a class decorator with the difference that `__init__` would be run when making subclasses, while class decorators are not called for subclasses.

Because our task involved creating a new instance (preventing those camelCase attributes from creeping into the class), override the `__new__` method in our custom `SnakeCaseMetaClass`. Let's confirm that it works:

```

>>> class SomeClass(metaclass=SnakeCaseMetaclass):
...     camelCaseVar = 5
>>> SomeClass.camelCaseVar
AttributeError: type object 'SomeClass' has no attribute 'camelCaseVar'
>>> SomeClass.camel_case_var
5
```

It works! Now you know how to write and use a metaclass in Python. Let's explore what you can do with this.

### Using metaclasses in Python

You can use metaclasses to enforce different guidelines on the attributes, methods, and their values. Similar examples along the lines of the previous example (using snake_case) include:

- Domain restriction of values
- Implicit conversion of values to custom classes (you might want to hide all of these complexities from users writing the class)
- Enforcing different naming conventions and style guidelines (like "every method should have a docstring")
- Adding new attributes to a class

The main reason to use metaclasses over having all of this logic defined in the class definitions itself is to avoid the code repetition throughout the codebase.

### Real-world uses of metaclasses

Because metaclasses are inherited among subclasses, they solve a practical problem of code redundancy (Don’t Repeat Yourself — DRY). Metaclasses also help in abstracting complex logic of class creation, typically by performing extra actions or adding extra code while class objects are produced. A few real world use cases of metaclasses are:

- Abstract base classes
- Registration of classes
- Creating APIs in libraries and frameworks

Let’s see examples of each one of them.

### Abstract base classes

Abstract base classes are the classes that are only meant to be inherited from and not to be instantiated. Python has the following:

```
from abc import ABCMeta, abstractmethod

class Vehicle(metaclass=ABCMeta):

    @abstractmethod
    def refill_tank(self, litres):
        pass

    @abstractmethod
    def move_ahead(self):
        pass
```

Let's create a `Truck` class inheriting from `Vehicle` class:

```
class Truck(Vehicle):
    def init(self, company, color, wheels):
        self.company = company
        self.color = color
        self.wheels = wheels

    def refill_tank(self, litres):
        pass

    def move_ahead(self):
        pass
```

Note that we haven't implemented the abstract methods. Let's see what happens if we try to instantiate an object of our `Truck` class:

```
>>> mini_truck = Truck("Tesla Roadster", "Black", 4)

TypeError: Can't instantiate abstract class Truck with abstract methods move_ahead, refill_tank
```

We can fix this by defining both the abstract methods in our `Truck` class:

```
class Truck(Vehicle):
    def init(self, company, color, wheels):
        self.company = company
        self.color = color
        self.wheels = wheels

    def refilltank(self, litres):
        pass

    def moveahead(self):
        pass
>>> mini_truck = Truck("Tesla Roadster", "Black", 4)
>>> mini_truck
<__main.Truck at 0x7f881ca1d828>
```

#### Registration of classes

To understand this, let's take an example of multiple file handlers at some server. The idea is to be able to find the right handler class quickly on the basis of file format. We'll create a handlers dictionary and let our `CustomMetaclass` register different handlers encountered in the code:

```
handlers = {}

class CustomMetaclass(type):
    def new(meta, name, bases, attrs):
        cls = type.new(meta, name, bases, attrs)
        for ext in attrs["files"]:
            handlers[ext] = cls
        return cls

class Handler(metaclass=CustomMetaclass):
    formats =     #common stuff for all kinds of file format handlers


class ImageHandler(Handler):
    formats = "jpeg", "png"
class AudioHandler(Handler):
    formats = "mp3", "wav">>> handlers
{'mp3': main.AudioHandler,
 'jpeg': main.ImageHandler,
 'png': main.ImageHandler,
 'wav': main.AudioHandler}
```

Now we can easily know which handler class to use based on the format of the file. Speaking generically, whenever you have to maintain some sort of data structure storing characteristics of classes, you can use metaclasses.

#### Creating APIs

Due to their ability to prevent redundancy of logic among subclasses and the ability to hide custom class creation logic that users need not to know, metaclasses are extensively used in frameworks and libraries. This presents interesting opportunities for reducing boilerplate and having a nicer API. For example, consider this snippet usage of Django’s ORM:

```

>>> from from django.db import models
>>> class Vehicle(models.Model):
...    color = models.CharField(max_length=10)
...    wheels = models.IntegerField()
```

Here, we create a `Vehicle` class inheriting from the models.Model class in a Django package. Inside the class, we define a couple of fields (color and wheels) to represent characteristics of a vehicle. Now, let's try to instantiate an object of the class we just created.

```

>>> four_wheeler = Vehicle(color="Blue", wheels="Four")
#Raises an error
>>> four_wheeler = Vehicle(color="Blue", wheels=4)
>>> four_wheeler.wheels
4
```

As a user creating a model for a Vehicle, we just had to inherit from the models.Model class and write some high-level statements. The rest of the work (like creating a hook to the database, raising an error for invalid values, returning an `int` type instead of models.IntegerField) was done behind the scenes by the model.Models class and the metaclass used by it.

### Summary

In this tutorial, you saw the relationship among instances, classes, and metaclasses in Python. You learned about metaprogramming, which is a way to manipulate code. We discussed function decorators and class decorators as a way to inject custom behavior into classes and methods. Then we went on to implement our custom metaclasses by subclassing the default type metaclass of Python. Finally, we saw some real-world use cases of metaclasses. The question of whether to use metaclasses is highly debated online, but it should now be much easier for you to analyze and answer if some problem is better solved using metaprogramming.

- [](http://www.facebook.com/sharer.php?u=https%3A%2F%2Fdeveloper.ibm.com%2Ftutorials%2Fba-metaprogramming-python%2F&t=Metaprogramming%20in%20Python "Share this on Facebook")

- [](https://twitter.com/intent/tweet?url=https%3A%2F%2Fdeveloper.ibm.com%2Ftutorials%2Fba-metaprogramming-python%2F&text=Metaprogramming%20in%20Python "Share this on Twitter")
- [](http://www.linkedin.com/shareArticle?mini=true&url=https%3A%2F%2Fdeveloper.ibm.com%2Ftutorials%2Fba-metaprogramming-python%2F&title=Metaprogramming%20in%20Python "Share this on LinkedIn")
Legend

- [
    
    Analytics
    
    ](https://developer.ibm.com/technologies/analytics)[
    
    Python
    
    ](https://developer.ibm.com/languages/python)
    

Interested in generative AI?

[

Learn generative AI skills

](https://developer.ibm.com/generative-ai-for-developers?cm_sp=ibmdev--developer--GenAI/)

[BuildSmart](https://developer.ibm.com/)[BuildSecure](https://developer.ibm.com/)

- IBM Developer
- [About](https://developer.ibm.com/about/?lang=en)
- [FAQ](https://developer.ibm.com/feedback/)
- [Third-party notice](https://developer.ibm.com/terms/third-party-notice/)

- Follow Us
- [Twitter](https://twitter.com/IBMDeveloper/)

- [LinkedIn](https://www.linkedin.com/showcase/developerworks/)

- [YouTube](https://www.youtube.com/channel/UCUm6InQvGI9-6vo1teGWINA/)

- Explore
- [Newsletters](https://developer.ibm.com/newsletters/)
- [Patterns](https://developer.ibm.com/patterns/)
- [APIs](https://developer.ibm.com/apis/)
- [Articles](https://developer.ibm.com/articles/)
- [Tutorials](https://developer.ibm.com/tutorials/)
- [Open source projects](https://www.ibm.com/opensource/open-projects/)
- [Videos](https://developer.ibm.com/videos/)
- [Events](https://developer.ibm.com/events/)

- [Community](https://developer.ibm.com/community)
- [Career Opportunities](https://ibm.com/employment)
- [Privacy](https://www.ibm.com/privacy/us/en)
- [Terms of use](https://developer.ibm.com/terms/ibm-developer-terms-of-use)
- [Accessibility](https://www.ibm.com/accessibility/us/en)
- [Cookie preferences](https://developer.ibm.com/tutorials/ba-metaprogramming-python/)
- [Sitemap](https://developer.ibm.com/sitemap)

