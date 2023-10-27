# Python

Created by: Daniel Dukeshire
Created time: April 7, 2023 7:27 PM
Tags: Product

---

# Table of Contents

# Chapter 4 Project Structure and Imports

### Entry Points

- When you import a Python module or package, it is given a special variable called __name__
- This contains the f**ull qualified name** of the module or package, with is the name as the import system sees it
    - This __name__ is __main__ when that file is executed DIRECRLY
- Otherwise, the name is <packagename>.<filename>
- files in a package called __main__.py are executed directly when the PACKAGE is executed

### Package Entry Points

- When we want to define a folder of python modules as a package, we define the __init__.py file
    - if  you ommit __main__.py from a package, it cannot be executed directly
- A package’s __init__.py file can come in handy when you want to change what is available for import and how it can be used
    - Most common use cases are to simplify imports and control the behavior of import *
    - controlled with the dunder __all__ = [package names]
    - when importing all, python only unpacks the names in this list

# Chapter 5 Variables and Types

---

- I have some misunderstandings in this area.
    
    [https://www.youtube.com/watch?v=_AEJHKGk9ns](https://www.youtube.com/watch?v=_AEJHKGk9ns) 
    

### Names and Values

```python
answer = 42
insight = answer
print(insight = answer) #true
print(insight is answer) #true
answer +=1
print(answer) #43 
print(insight) #42
print(insight is answer) #false
```

- the NAME answer is bound to the VALUE 42 “assignment”
    - Names have scope, but not type
    - values have type, but no scope
- insight does not refer to a copy of answer, but rather the original value
- This brings us to the *is* operator (identity operator)
- BEWARE this is tricky - *is* checks IDENTITY i.e. same value in memory
    - this is different than comparison
    - avoid using this unless for *None*
- Python is a dynamically, strongly typed language
    - [https://www.educative.io/answers/statically-v-dynamically-v-strongly-v-weakly-typed-languages](https://www.educative.io/answers/statically-v-dynamically-v-strongly-v-weakly-typed-languages)
    - “Duck typing”

### Scope and Garbage Collection

- Functions and comprehensions define their own scope, and are the only structures in the language to do so
    - i.e. list comprehensions
- For garbage collection, python uses reference counting
    - For each value, python keeps a count of the number of references
    - once that value has no more references
- Global and nonlocal keywords
    
    ```python
    def outer_function():
        x = 0
    
        def inner_function():
            nonlocal x # see here
            x += 1
            print(x)
    
        inner_function()
        inner_function()
    
    outer_function()
    ```
    
- Scope Resolution order
    1. Local
    2. Enclosing function locals (nonlocal finds)
    3. Global
    4. Built-in
- Nonlocal and global adjust the behavior of scope resolution order
    - An example of global keyword is when a function wants to modify a global variable. The global keyword is used to indicate that the variable is a global variable, not a local variable. This allows the function to modify the value of the global variable. However, using global variables is generally discouraged because it can make code harder to read and debug.
        - An example of nonlocal is when a variable is defined in the outer function and is referenced in the inner function. If the inner function wants to modify the value of the variable, the nonlocal keyword is used to indicate that the variable should be looked up in the enclosing function's namespace, not the global namespace.
- **weakref**
    - This is handy for performance costs during garbage collection
    - The *weakref* module is useful for creating references to values without increasing their reference count, which can help with performance during garbage collection.
    
    The *weakref* module in Python can be used to create references to values without increasing their reference count, which can help with performance during garbage collection. Here is an example of using *weakref*:
    
    ```python
    import weakref
    
    class MyClass:
        def __init__(self, name):
            self.name = name
    
    obj = MyClass("example object")
    ref = weakref.ref(obj)
    
    print(ref())  # Output: <__main__.MyClass object at 0x7f8abdd6f340>
    
    del obj
    
    print(ref())  # Output: None
    
    ```
    
    In this example, we create an instance of the `MyClass` class called `obj` with the name `"example object"`. We then create a weak reference to `obj` using the `weakref.ref()` function and assign it to the variable `ref`. We can retrieve the original object by calling the weak reference using parentheses, as in `ref()`. After `obj` is deleted, we can see that calling `ref()` returns `None`, indicating that the object is no longer available in memory.
    
    Note that because `ref` is a weak reference, it does not increase the reference count of `obj`, so the object is still eligible for garbage collection even though `ref` still exists.
    

### Immutable vs Mutable

- A variable is immutable when it cannot be changed after it is created, whereas a mutable variable can be altered. i.e. modified in place
- **Mutable** types:
    - lists, dictionaries, sets, byte arrays, and user-defined classes (objects)
    - When a mutable object is changed, all references to it are affected because they all reference the same object in memory
        - This is *aliasing*
- **Immutable** types:
    - numbers (int, float, complex), strings, tuples, and frozensets
    - When an immutable object is modified, a new object is created in memory and the reference is updated to point to the new object
    - Don’t really need to worry about these when assigning

- Example for mutable types:

```python
temps = [1, 2, 3]
highs = temps
print(highs is temps) #prints true
temps += [81]
print(temps is highs) #prints true
print(temps) #[1, 2, 3, 81]
print(highs) #[1, 2, 3, 81]
```

- Because the list is aliased to both temps and highs, any modifications made to the list value are visible through either name

### Passing by Assignment

- Python does not pass by reference or by value - passes by assignment
    - This behaves as expected for immutable objects, but gets tricky with mutable ones
- Example:

```python
def find_lowest(temperatures):
	temperatures.sort() # sort is a modifier - does not create a new list but adjusts the current
	print(temperatures[0]

temps = [85, 100, 105, 24, 45]
print(find_lowest(temps)
print(temps) # this will be the sorted temps
```

- As python is pass by assignment, we essentially set temperatures = temp when find_lowest() is called
    - Now, temperatures is referring to the same object as temps. They are ALIASED
    - So we are modifying the variable directly
    - We can modify this function to prevent this:

```python
def find_lowestS(temperatures):
	new_temps = sorted(temperatures) # sorted returns a new list, so we do not modify the original
	print(new_temps[0])
```

### Collections and References

- All collections, including lists, employ this detail:
    - *Individual items are references*
    - Just as a name is bound to a value, so are items in collections bound to values in the same manner.
    - This binding is called a *reference*
- Example:
    
    ```python
    board = [["-"] * 3] * 3 # create a 3x3 board for tic tac toe
    
    board[1][0] = "X"
    
    for row in board:
    	print(f'{row[0} {row[1} {row[2}')
    
    # Output:     # Expected Output
    X - -         - - -
    X - -         X - -
    X - -         - - -
    ```
    

- This is because lists are MUTABLE. The outer list has three aliases to the same list.
    - Therefore, changing one of the immutable strings inside one of the inner lists changes all of the lists
    - This can be fixed by changing the way the board is declared
        - Maybe list comprehension?
        
        ```python
        board = [["-"] * 3 for _ in range(3)]
        ```
        

- Lets think about this in a nested fashion. A tuple (immutable) of lists (mutable)
    - You cannot change the tuple itself, but you can indirectly modify the values its items refer to:
    
    ```python
    scores_team_one = [100, 200, 400]
    scores_team_two = [300, 400, 200]
    scores_team_three = [900, 300, 400]
    
    scores = (scores_team_one, scores_team_two, scores_team_three)
    
    scores_team_one[0] = 300 
    print(scores[0]) # [300, 200, 400]
    scores[0][2] = 80
    print(scores[0]) # [300, 200, 80]
    ```
    
- Therefore, mutable values are ALWAYS going to be mutable, no matter where they live.

### Copies

- There are many ways to ensure you are binding a name to a copy of a mutable value - instead of aliasing the original
- Shallow Copy vs Deep Copy
    - In Python, a shallow copy is a copy of an object that is created such that the new object points to the same memory addresses as the original object. This means that any changes made to the new object will also affect the original object.
    
    ```python
    import copy
    
    class Taco:
        def __init__(self, toppings):
            self.toppings = toppings
    
        def __repr__(self):
            return f"Taco({self.toppings})"
    
    taco1 = Taco(["beef", "cheese", "lettuce"])
    taco2 = copy.copy(taco1)
    taco2.toppings.append("sour cream")
    
    print(taco1)  # Output: Taco(['beef', 'cheese', 'lettuce', 'sour cream'])
    print(taco2)  # Output: Taco(['beef', 'cheese', 'lettuce', 'sour cream'])
    ```
    
- In this example, we have a `Taco`class that takes a list of toppings as an argument to its constructor. We create an instance of `Taco`called `taco1`with toppings `["beef", "cheese", "lettuce"]`
    - We then make a shallow copy of `taco1`lcalled `taco2`using the `copy.copy()`method.
- We then add the topping `"sour cream"`to the `toppings` list of `taco2`. However, since `copy.copy()`creates a shallow copy, the `toppings`list of `taco2`is still referencing the same list object as `taco1`, instead of creating a new list object. Therefore, when we append `"sour cream"`to `taco2.toppings`, we are also modifying the `toppings`list of `taco1`.
- To avoid this bug, we could use `copy.deepcopy()`instead of `copy.copy()` to create a deep copy of the `Taco` object, which would create a new list object for the `toppings`list of `taco2`
    - It's important to note that the `copy.copy()`method only creates a shallow copy of an object, so any nested objects, such as lists within a list, will still be shared between the original object  and the shallow copy. If you want to create a copy of an object with all nested objects also copied, you can use `copy.deepcopy()` instead.

Coercion and Conversion

- Python has no need of type casting as names do not have types.
- When we allow python to make conversions by itself, we call it *coercion*
    - i.e., the act of implicitly casting a value from one type to another
    - conversion - the act of explicitly casting a value from one type to another

# Chapter 6 Functions and Lambdas

---

- Functions are first-class objects - they are treated no differently than any other object
    - This is similar to FUNCTIONAL PROGRAMMING (remember haskell?)
- Python can be treated as a functional language. Whether I use it as such, there are general rules of thumb that are applicable nonetheless:
    1. Every function should do ONE specific thing
    2. A function’s implementation should never affect the behavior of the rest of the program
    3. Avoid side affects
    4. Functions should not be affected or affect state. They should be stateless
        1. providing the same input should always yield the same output
        2. This has exceptions in terms of classes and objects, of course

## Keyword Arguments

- Positional arguments is what you generally think of
- keyword arguments allow you to pass a dict of values in
    - i.e. **kwargs

Overloaded Functions 

- Python does not need overloaded functions (dont confuse with overriden functions)
    - Using dynamic typing and OPTIONAL PARAMETERS INTRODUCED FROM KEYWORD ARGUMENTS, it rids this necessity
- However, they can be forced with single dispatch functions
- If there are not a known number of arguments, we can use the single asterisk
    - *args - allows for any number of arguments
        
        ```python
        def sum_numbers(*args):
            total = 0
            for num in args:
                total += num
            return total
        
        print(sum_numbers(1, 2, 3, 4, 5)) # Output: 15
        ```
        
- We can also use keyword arguments as such
    - **kwargs
    - This is a *keyword variadic parameter*
    
    ```python
    def describe_pet(**kwargs):
        for key, value in kwargs.items():
            print(f"{key}: {value}")
    
    describe_pet(animal_type='dog', pet_name='Fido')
    ```
    
- We can also take callable objects, like functions:
    
    ```python
    class MyCallable:
        def __call__(self, *args, **kwargs):
            print("I am callable!")
    
    # Create an instance of the class
    mc = MyCallable()
    
    # Call the instance as if it were a function
    mc()
    
    # I am callable!
    ```
    
- You can check to see if an object is callable by passing it to the callable() function
- This comes back into play with *Decorators*

Keyword only Parameters

- We can force functions to only pass in arguments as keywords, as such
    - Everything after the * MUST be keyword only
    
    ```python
    def greet(name, *, greeting='Hello'):
        print(f"{greeting}, {name}!")
    
    greet('John', greeting='Hi') # Output: Hi, John!
    greet('Jane') # Output: Hello, Jane!
    
    def roll_dice(*, side=6, dice=1)
    ```
    
- Positional Only Parameters
- Same thing applies with a backslash - all elements preceding must be passed in by order:
    
    ```python
    def add(x, y, /):
        return x + y
    
    print(add(3, 4)) # Output: 7
    
    def roll_dice(dice=1, /, sides=6):
    	pass
    
    roll_dice()         # Valid, equivalent to roll_dice(1, 6)
    roll_dice(2)        # Valid, equivalent to roll_dice(2, 6)
    roll_dice(2, 8)     # Valid, specifies 2 dice with 8 sides each
    roll_dice(sides=8)  # Valid, equivalent to roll_dice(1, 8)
    roll_dice(1, sides=8) # Valid, equivalent to roll_dice(dice=1, sides=8)
    roll_dice(dice=2, sides=8) # Invalid, since `dice` is a positional-only parameter
    roll_dice(sides=8, dice=2) # Invalid, since `dice` is a positional-only parameter
    ```
    
- The parameter dice has a default value of one, but it is now positional only
- sides can be used as a positional or keyword

### Closures

- A closure is a function object that has access to variables in its enclosing lexical scope, even when the function is called outside that scope.
- Closures are a way to implement data hiding and encapsulation in Python. A closure is created when a nested function references a value in the enclosing function's scope. The nested function can then access that value even after the enclosing function has returned.

Closures can be used to create functions with a specific state that can be preserved across multiple function calls.

```python
def make_multiplier(factor):
    def multiplier(x):
        return x * factor
    return multiplier

double = make_multiplier(2)
triple = make_multiplier(3)

print(double(5)) # Output: 10
print(triple(5)) # Output: 15
```

Closures have state…. so be careful 

```python
def counter():
    count = 0
    
    def increment():
        nonlocal count
        count += 1
        return count
    
    return increment

c = counter()
print(c()) # Output: 1
print(c()) # Output: 2
print(c()) # Output: 3
```

- Whenever you see a nonlocal in a closure, BE CAREFUL
- It suggests the presence of state
- Nested functions or closures can be used to create functions with a specific state that can be preserved across multiple function calls. This is useful for creating counters and other similar constructs.
- We can return the function one of two ways:
    - Executed, i.e. return: increment()
    - Or as a first-class variable, return: increment.
        - This can be executed down the line as it is stored as an output var
        - Therefore, we can call it as many times as we want in the future

### Lambdas

- Lambda functions, also known as anonymous functions, are functions that are defined without a name.
- They are typically used for small, one-time use cases and are defined using the `lambda` keyword.

Here is an example of a simple lambda function that takes two arguments and returns their sum:

```python
sum = lambda x, y: x + y
print(sum(3, 4)) # Output: 7
```

Lambda functions can be useful for functional programming techniques, such as passing a function as an argument to another function. They are also commonly used in Python's built-in functions, such as `map()`, `filter()`, and `reduce()`

- Lambdas as sorting keys i.e., specifiying a key function
    
    ```python
    my_dict = {'c': 3, 'a': 1, 'b': 2}
    sorted_dict = dict(sorted(my_dict.items(), key=lambda x: x[0]))
    print(sorted_dict)
    
    my_list = [(3, 'c'), (1, 'a'), (2, 'b')]
    sorted_list = sorted(my_list, key=lambda x: x[1])
    print(sorted_list)
    ```
    

The sorted function uses the key argument, which is always a function or other callable, by passing each item to it and then using the value returned from the callable to determine the sorting order

- Because we want to sort by one of the values in the container, we can specify that value with the lambda

### Decorators

- Decorators in Python are a way to modify the behavior of a function by wrapping an additional layer of logic around it, without having to change the function itself.
- Decorators are essentially higher-order functions that take a **function as an argument** and return a new function that modifies the behavior of the original function in some way.

A decorator is defined using the `@` symbol followed by the decorator function name, which is placed on the line immediately before the function definition. The decorator function must take a function as its argument and return a new function that wraps the original function.

Here's an example of a decorator:

```python
def my_decorator(func):
    def wrapper():
        print("Before the function is called.")
        func()
        print("After the function is called.")
    return wrapper

@my_decorator
def say_hello():
    print("Hello!")

say_hello()
# or
my_decorator(say_hello)
```

In this example, we defined a decorator called `my_decorator`. This decorator takes a function as its argument, and returns a new function called `wrapper` that first prints a message before the original function is called, then calls the original function, and then prints another message after the original function is called.

We then applied the `my_decorator` decorator to the `say_hello` function using the `@` symbol, which modified the behavior of the `say_hello` function by running the additional layer of logic defined in the `my_decorator` function.

When we run the `say_hello` function, we can see that it now runs the additional logic defined in the `my_decorator` function:

```python
Before the function is called.
Hello!
After the function is called.
```

Decorators are very useful for adding functionality to a function without having to modify its code directly. They can be used to implement features such as logging, timing, authentication, and caching, among others.

- Because we do not know the number of arguments will be passed into the function we apply the decorator to, we need to use closures
    
    ```python
    def character_action(func):
        @functools.wraps(func)
        def wrapper(*args, **kwargs):
            if health <= 0:
                print(f"{character} is too weak.")
                return
    
            result = func(*args, **kwargs)
            print(f"    Health: {health} | XP: {xp}")
            return result
    
        return wrapper
    
    @character_action
    def eat_food(food):
        global health
        print(f"{character} ate {food}")
        health += 1
    
    @character_action
    def fight_monster(monster, strength):
        global health, xp
        if random.randint(1, 20) >= strength:
            print(f"{character} defeated {monster}.")
            xp += 10
        else:
            print(f"{character} flees from {monster}.")
            health -= 10
            xp += 5
    
    eat_food("bread")
    fight_monster("Imp", 15)
    fight_monster("Direwolf", 15)
    fight_monster("Minotaur", 19)
    ```
    

### Type Hints and Function Annotations

- Type hints are hints about what type should be passed in or returned - they are not strictly necessary
- Type hints are specified with annotations
- Python uses Duck Typing, implying that variables are not bound to a specific type
    - Rather, we are able to infer the type of a variable based on how it is used throughout the program
- Type hints are not enforced, as Python is dynamically typed, but they can help improve code readability and reduce errors
- Python 3.5 introduced support for function annotations, which allow you to add metadata to function arguments and return values
- Function annotations are specified using the `>` syntax
- The syntax for function annotations is as follows: `def function_name(arg1: type, arg2: type) -> return_type:`

Here is an example of a function with annotations:

```python
def add_numbers(x: int, y: int) -> int:
    return x + y

print(add_numbers(3, 4)) # Output: 7
```

- In this example, we are using type hints to indicate that the `x` and `y` arguments should be integers, and that the function will return an integer
- Type hints can be used with any Python object, including custom classes and modules
- Type hints can also be used with variables and class attributes, as well as with function arguments and return values
- However, it's important to note that type hints are not enforced by the Python interpreter, so it's still possible to pass in arguments of the wrong type or return values of the wrong type

### Type Aliasing

- Type aliases are a way to create a new name for an existing type in order to make the code more readable and modular
- Type aliases are created using the `typing` module, which provides a number of useful classes and functions for working with types in Python
- Type aliases are defined using the `TypeVar` class, which takes a name and a bound type as its arguments
- Here is an example of a type alias:

```python
from typing import List, Dict, Tuple, TypeVar

Vector = List[float]
Employee = Dict[str, Union[str, int]]
Coordinates = Tuple[float, float]

T = TypeVar('T')
```

- In this example, we are creating type aliases for a list of floats, a dictionary of employee information, and a tuple of coordinates
- We are also creating a type variable `T` that can be used to represent any type
- Type aliases can be used to improve the readability of code, especially when working with complex data structures or function signatures
- They can also make it easier to refactor code by allowing you to change the underlying implementation of a type without having to modify all the code that uses it
- Type aliases are not enforced by the Python interpreter, but they can be used by static type checkers like `mypy` to catch type errors at compile time
- Type aliases can also be used as generic types, allowing you to create functions and classes that work with any type that satisfies a certain requirement

# Chapter 7 Objects And Classes

---

- In python, functional programming is not mutually exclusive with object oriented programming.
- REMEMBER: Functional = haskell, oop = java, procedural = c
- A class is a sort of blueprint for creating one or more objects, known as an instance in python
- The purpose of a class is encapsulation
    1. The data and the functions that manipulate said data are bound together into one cohesive unit
    2. The implementation of a class’s behavior is kept out of the way of the rest of the program (known as a black box)
- There are important relationships in object oriented programming
    1. **Inheritance**: Inheritance is a mechanism where a class (subclass) is derived from another class (superclass) and inherits all the properties and methods of the superclass. This allows for the reuse of code and can help to organize classes into a hierarchy.
    2. **Encapsulation**: Encapsulation is a mechanism for hiding the implementation details of a class from the outside world, and exposing a public interface for interacting with the class. This helps to ensure that the class is used correctly and can make it easier to change the implementation details without affecting the rest of the code.
    3. **Polymorphism**: Polymorphism is the ability of objects of different classes to be treated as if they were objects of the same class. This is achieved through the use of inheritance and interfaces, and can make it easier to write flexible and reusable code.
    4. **Composition/Abstraction**: Composition is a mechanism where a class contains one or more instances of other classes as member variables. This allows for the creation of complex objects by combining simpler objects, and can help to promote code reuse and modularity.
    
    Understanding these relationships is essential to developing effective object-oriented software, and can help to ensure that your code is flexible, maintainable, and scalable.
    

```python
class MyBaseClass:
    def __init__(self, x):
        self.x = x

    def foo(self):
        print(self.x)

class MyDerivedClass(MyBaseClass):
    def bar(self):
        print("Hello, world!")
```

### The Constructor

- In Python, both `__init__()` and `__new__()` are special methods that are called when an object is created, but they serve different purposes.
- `__new__()` is responsible for creating the object and returning it. It is called before `__init__()`, and is used to create and return a new instance of the class. The method is called with the class itself as the first argument, followed by any arguments passed to the constructor. The return value of `__new__()` is an instance of the class, which is then passed as the first argument to `__init__()`.
- `__init__()` is responsible for initializing the object's attributes after it has been created by `__new__()`. It is called with the instance of the class as the first argument (i.e., `self`), followed by any other arguments passed to the constructor. It is used to set the initial values of the object's attributes, and to perform any other necessary setup.
- In other words, `__new__()` is responsible for creating the object, and `__init__()` is responsible for initializing it. You can override both methods in your own classes to customize the object creation and initialization process, but in most cases, you'll only need to define `__init__()`.

Here's an example of a class that overrides both `__new__()` and `__init__()`:

```python
class MyObject:
    def __new__(cls, *args, **kwargs):
        print("Creating object...")
        instance = super().__new__(cls)
        return instance

    def __init__(self, x):
        print("Initializing object...")
        self.x = x
```

In this example, `__new__()` is used to print a message indicating that the object is being created, and to return the instance of the class created by the `super()` call. `__init__()` is used to print a message indicating that the object is being initialized, and to set the `x` attribute of the object.

- Note that `__new__()` is not commonly used in Python, and in most cases, you'll only need to define `__init__()` to customize object initialization.

### The Finalizer

- In Python, the finalizer (also known as the destructor) is a special method called `__del__()` that is called when an object is garbage collected. The finalizer is used to perform any necessary cleanup tasks before the object is destroyed.
- The finalizer method is defined like any other method in a class, but with the name `__del__`. It is automatically called when the object is no longer needed and is about to be deleted from memory.

Here is an example of a class that defines a finalizer:

```python
class MyClass:
    def __init__(self, name):
        self.name = name

    def __del__(self):
        print(f"{self.name} is being deleted...")

me = myClass("Dan")
del me
```

In this example, the `__del__()` method is defined to print a message indicating that the object is being deleted. This method will be called automatically by the Python interpreter when the object is no longer referenced and is about to be garbage collected.

- It's worth noting that the finalizer is not always a reliable way to perform cleanup tasks, as it is not guaranteed to be called in all cases. It's generally better to use the `with` statement or context managers to ensure that resources are properly cleaned up when they are no longer needed.

### Attributes

- In Python, attributes are variables that are associated with an object. There are two types of attributes in Python: class attributes and instance attributes.
    - Class attribute = static attribute, instance = ins

Class attributes are variables that are defined at the class level and are shared by all instances of the class. They are defined outside of any methods and are accessed using the class name or an instance of the class. For example:

```python
class MyClass:
    class_attribute = 10

    def __init__(self, instance_attribute):
        self.instance_attribute = instance_attribute
```

In this example, `class_attribute` is a class attribute because it is defined at the class level, outside of any methods. 

- All instances of the `MyClass` class share the same value of `class_attribute`, which is 10.

Instance attributes, on the other hand, are variables that are specific to each instance of the class. They are defined inside the `__init__` method and are accessed using an instance of the class. For example:

```python
class MyClass:
    class_attribute = 10

    def __init__(self, instance_attribute):
        self.instance_attribute = instance_attribute
```

In this example, `instance_attribute` is an instance attribute because it is defined inside the `__init__` method. 

- Each instance of the `MyClass` class has its own value of `instance_attribute`.

Here's an example that illustrates the difference between class attributes and instance attributes:

```python
class MyClass:
    class_attribute = 10

    def __init__(self, instance_attribute):
        self.instance_attribute = instance_attribute

my_object1 = MyClass(20)
my_object2 = MyClass(30)

print(MyClass.class_attribute)    # Output: 10
print(my_object1.class_attribute) # Output: 10
print(my_object2.class_attribute) # Output: 10

my_object1.class_attribute = 40

print(MyClass.class_attribute)    # Output: 10
print(my_object1.class_attribute) # Output: 40
print(my_object2.class_attribute) # Output: 10

print(my_object1.instance_attribute) # Output: 20
print(my_object2.instance_attribute) # Output: 30

```

In this example, we create two instances of the `MyClass` class (`my_object1` and `my_object2`). We then modify the `class_attribute` of `my_object1` to be 40. When we print the values of `class_attribute` for `MyClass`, `my_object1`, and `my_object2`, we see that `MyClass` and `my_object2` still have a value of 10, but `my_object1` has a value of 40. This is because `class_attribute` is a class attribute and is shared by all instances of the class, but when we modify it using an instance of the class, it creates a new instance attribute that is specific to that instance. On the other hand, `instance_attribute` is an instance attribute and is specific to each instance of the class.

### Scope Naming Conventions

- There are three different levels of visibility for attributes and methods in a class: public, non-public, and name-mangled.
1. **Public**: Public attributes and methods are accessible from outside the class. They do not have any prefix or special naming convention. By convention, public names are written in lowercase letters and separated by underscores, e.g., "my_attribute" or "my_method()".
2. **Non-public**: Non-public attributes and methods are intended for use within the class or by subclasses. These attributes and methods have a single underscore prefix, e.g., "_my_attribute" or "_my_method()". This naming convention is a convention that tells other programmers that these attributes or methods are not part of the public API and are subject to change.
3. **Name-mangled**: Name-mangled attributes and methods are intended to be truly private, to be accessed only within the class. They have a double underscore prefix followed by the name of the attribute or method, e.g., "__my_attribute" or "__my_method()". Name-mangling is a technique to make the attribute or method name more difficult to access accidentally from outside the class.
    1. Use this when:
        1. Need to avoid a naming conflict in the context of inheritance
        2. External access of the attribute will have exceptionally horrific effects on the behavior of the class

It's important to note that these visibility levels are not intended to provide complete security, but rather to provide organization and to communicate intent to other programmers. Developers should use these naming conventions judiciously and not rely on them as a security mechanism.

- In summary, public attributes and methods are accessible from outside the class, non-public attributes and methods are intended for use within the class or by subclasses, and name-mangled attributes and methods are intended to be truly private, to be accessed only within the class.

### Methods

- **Instance Methods**
    - Methods that exist on the instance itself
    - The first parameter, named self, provides access to the instance attributes of the instance
- **Class Methods**
    - Belong to the class opposed to the instance of the class → similar to a traditional static method
    - Useful for working with class attributes.
    - The class method is preceded with the built-in @classmethod decorator
        - A class method takes the classname as the first parameter
    
    ```python
    class Car():
    	_codeword = ""
    
    	@classmethod
    	def inform(cls, codeword):
    		cls._codeword = codeword
    
    print(Car.inform("my_codeword"))
    ```
    
    - One of the benefits of this approach is that I don’t have to worry about whether I am calling inform() on the class or on the instance. Because the method is a class instance, it will always access the class attribute on the class (cls) instead of the instance (self), and avoid accidentally shadowing _codeword on a single instance.
    - You can also call these class methods on any instance as well, not just the class keyword:
        
        ```python
        Car.inform("my_codeword")
        print(Car._codeword) # my_codeword
        
        buick = Car()
        buick.inform("buick_codeword")
        print(buick._codeword) #buick_codeword
        ```
        
    - When calling the class method with the dot operator, the class is implicitly passed to the cls parameter.
- **Static Methods**
    - A regular function inside a class that accesses NEITHER the class or instance attributes
    - The only difference between a static method and a function is that the static method belongs to the class namespace
    - The main reason to write a static method is when your class offer some functionality that doesn’t need to access any of the class or instance attributes or methods.
    
    ```python
    @staticmethod
    def inquire(question):
    	print("I know nothing")
    ```
    
- Again, the method is preceded with the @staticmethod decorator

### Properties

- Properties constitute a special variety of instance method that allows you to write getters and setters that behave so it appears that you were directly accessing an instance attribute.
- It is preferable to use properties, instead of making the user remember whether to call a method or use an attribute.
    - It is also much more pythonic than cluttering your class with bare getters and setters that don’t augment attribute access or modification
- A property behaves lie an attribute, but it is made up of three instance methods:
    - A **getter**
    - A **setter**
    - A **deleter**
- Remember! A property appears to be an ORDINARY attribute to the user of the class
    - Accessing the attribute calls the getter, changing the attribute calls the setter, and deleting the attribute calls the deleter  (with the del keyword)
    - Thus, the actual property functions we want as PRIVATE
- There are many different ways to declare properties:

Here are some properties in action

```python
class SecretAgent:
	_codeword = None
	
	def __init__(self, codename):
		self.codename = codename
		self._secrets = []

	def __del__(self):
		print(f"Agent {self.codename} has been disavowed")

	def remember(self, secret):
		self._secrets.append(secret)

	@classmethod
	def inform(cls, codeword):
		cls._codeword = codeword

	@staticmethod
	def inquire(question):
		print("I know nothing")

	@classmethod
	def _encrypt(cls, message, *, decrypt=False):
		code = sum(ord(c) for c in cls._codeword)
		if decrypt:
			code = -code
		return ''.join(chr(ord(m) + code) for m in message)

############
# OPTION ONE
# Convention: getx, setx, delx, where x is the name of the property
def _getsecret(self):
	return self._secrets[-1] if self._secrets else None

def _setsecret(self, value):
	self._secrets.append(self._encrypt(value)

def _delsecret(self):
	self._secrets = []

# now, we can treat this public class attriubte as a "normal" variable
# this is normally defined on the class itself, and after all the functions
# that make it up
secret = property(fget = _getsecret, fset=_setsecret, fdel =_delsecret)

############
# OPTION TWO
# Property with decorators
secret = property()

@secret.getter
def secret(self):
	return self._secrets[-1] if self._secrets else None

@secret.setter
def secret(self, value)
	self._secrets.append(self.encrypt(value))

@secret.deleter
def secret(self)
	self._secrets = []

##############
# OPTION THREE 
# Property with decorators and no property()
# this is the most popular -> as most properties
# are just getters

@property
def secret(self)
	return self._secrets[-1] if self._secrets else None

@secret.setter
def secret(self, value)
	self._secrets.append(self.encrypt(value))

@secret.deleter
def secret(self)
	self._secrets = []

```

- Bear in mind, for option three, that shortcut only works with the getter method
- You can omit any of the three if their behavior is not necessary

One of the chief drawbacks of properties is that they conceal that some calculation or processing is being performed upon assignment, which the client might not expect.

- This especially becomes a problem if the processing is long or calculated, and the property needs to be run concurrently with async or threads.

### Special Methods

- **Special**/**Magic**/**Dunder** methods allow you to add support to your classes for virtually any Python operator or built-in command
    - dunder == double underscore
    - __init__(), __new__(), __del__() are some examples
    - See [https://docs.python.org/3/reference/datamodel.html](https://docs.python.org/3/reference/datamodel.html) for a comprehensive list of special methods

**Conversion Methods** 

- Canonical String Representation
    - __repr__()
        - print(repr(classObject))
    - It is considered good practice to define at a minimum, this instance method. This returns a string representation of all the data necessary to create another class instance with the same contents.
    - If you do not find one, python falls back on the default definition
- Human-Readable String Representation
    - __str__()
    - Similar purpose to canonical string rep, but it is meant to be human readable
    - print(str(classObject))
- Unique Identifier
    - __hash__()
    - Returns a hash value, which is an integer that is unique to the data within the class instance
    - Allows you to use the object in certain collections, such as keys in dictionaries or values in a set
    - Should only depend on values that will not change through the life of the instance.
- Here are a list of some of the most common conversion special methods.
1. `__str__(self)`: This method is called by the built-in `str()` function and is used to return a string representation of the object.
2. `__repr__(self)`: This method is called by the built-in `repr()` function and is used to return a string that can be used to recreate the object.
3. `__bytes__(self)`: This method is used to convert the object into a bytes object. It is called by the built-in `bytes()` function.
4. `__format__(self, format_spec)`: This method is called by the built-in `format()` function and is used to return a formatted string representation of the object.
5. `__int__(self)`: This method is used to convert the object into an integer. It is called by the built-in `int()` function.
6. `__float__(self)`: This method is used to convert the object into a float. It is called by the built-in `float()` function.
7. `__complex__(self)`: This method is used to convert the object into a complex number. It is called by the built-in `complex()` function.
8. `__bool__(self)`: This method is used to convert the object into a boolean value. It is called by the built-in `bool()` function.
9. `__hash__(self)`: This method is used to return a hash value for the object, which is required for the object to be used in dictionaries and sets.
10. `__index__(self)`: This method is used to return an integer that represents the object in a sequence, such as a list or tuple. It is called by the built-in `index()` function.
11. `__len__(self)`: This method is used to return the length of the object. It is called by the built-in `len()` function.
12. `__iter__(self)`: This method is used to return an iterator object for the object, which allows it to be used in loops and comprehensions.
13. `__reversed__(self)`: This method is used to return a reversed iterator object for the object, which allows it to be used in reverse loops and comprehensions.
14. `__next__(self)`: This method is used to return the next item in an iterator. It is called by the built-in `next()` function.
15. `__contains__(self, item)`: This method is used to determine whether the object contains a given item. It is called by the `in` operator.

**Comparison Methods**

- Equals + Non
    - __eq__(self, other) → other being another object of that class
    - called by the == operator → a == b is a.__eq__(b)
    - __ne__() is not equals, same thing. If not defined, this delegates to the opposite of what equals return
- Less + Greater than
    - __lt__() and __gt__(), __le__() and __ge__()
    - called by < and >, ≤ and ≥
    - These are reflections of one another, meaning one operator can be substituted for the other
- Bool
    - __bool__()
    - if the object is true or false

**Binary Operator Support**

- Operators with two operands: add, subtract, etc.
1. `__add__(self, other)` : used to implement addition operator "+"
2. `__sub__(self, other)` : used to implement subtraction operator "-"
3. `__mul__(self, other)` : used to implement multiplication operator "*"
4. `__truediv__(self, other)` : used to implement true division operator "/"
5. `__floordiv__(self, other)` : used to implement floor division operator "//"
6. `__mod__(self, other)` : used to implement modulo operator "%"
7. `__divmod__(self, other)` : used to implement divmod() function
8. `__pow__(self, other[, modulo])` : used to implement power operator "**"
9. `__lshift__(self, other)` : used to implement left shift operator "<<"
10. `__rshift__(self, other)` : used to implement right shift operator ">>"
11. `__and__(self, other)` : used to implement bitwise and operator "&"
12. `__or__(self, other)` : used to implement bitwise or operator "|"
13. `__xor__(self, other)` : used to implement bitwise xor operator "^"

**Unary Operator Support**

1. `__pos__(self)` : used to implement unary plus operator "+"
2. `__neg__(self)` : used to implement unary negation operator "-"
3. `__abs__(self)` : used to implement absolute value function abs()
4. `__invert__(self)` : used to implement bitwise inversion operator "~"
5. `__round__(self[, ndigits])` : used to implement round() function
6. `__floor__(self)` : used to implement math.floor() function
7. `__ceil__(self)` : used to implement math.ceil() function
8. `__trunc__(self)` : used to implement math.trunc() function

**Making Callable**

- The last of the special methods is making an instance callable - just like a function
- __call(self, other)__

```python
class MyCallable:
    def __init__(self, name):
        self.name = name
    
    def __call__(self):
        print(f"Hello, {self.name}!")

mc = MyCallable("Alice")
mc() # Output: "Hello, Alice!"
```

### Method Chaining

- i.e. returning self for instance methods, etc. in OOP
- returning an instance of itself

### Class Decorators

- Classes support decorators much like functions do.

Class decorators wrap the instantiation of the class, allowing you to intervene in any number of ways: adding attributes, initializing another class containing an instance of the one being decorated, or performing some behavior immediately on the new object. 

```python
class CoffeeOrder:

    def __init__(self, recipe, to_go=False):
        self.recipe = recipe
        self.to_go = to_go

    def brew(self):
        vessel = "in a paper cup" if self.to_go else "in a mug"
        print("Brewing", *self.recipe.parts, vessel)

class CoffeeRecipe:

    def __init__(self, parts):
        self.parts = parts

special = CoffeeRecipe(["double-shot", "grande", "no-whip", "mocha"])
order = CoffeeOrder(special, to_go=False)
order.brew()  # prints "Brewing double-shot grande no-whip mocha in a mug"

import functools
def auto_order(to_go):
    def decorator(cls):
        @functools.wraps(cls)
        def wrapper(*args, **kwargs):
            recipe = cls(*args, **kwargs)
            return (CoffeeOrder(recipe, to_go), recipe)
        return wrapper
    return decorator

@auto_order(to_go=True)
class CoffeeShackRecipe(CoffeeRecipe):
    pass

order, recipe = CoffeeShackRecipe(["tall", "decaf", "cappuccino"])
order.brew()  # prints "Brewing tall decaf cappuccino in a paper cup"
```

### Functional Meets Object Oriented

- Attributes should only be modified by their instance’s or class’s methods, called via the dot operator:
    - thing.action() #modifies
- When an  object is passed to a function, it should not be mutated (no side effects)
    - action(thing) #should not modify thing → returns a new value or object

### When to use classes

- It is not always necessary to write classes in python
- Classes arent modules
    - You should not write a class where a module will be sufficient
    - Modules already allow you to organize variables and functions by purpose or category, so the use of classes here does not matter
    - Make sure the class is a cohesive unit - are the class attributes + variables + functions related?
- Single Responsibility
    - A class should have a single, well-defined responsibility.
    - Avoid making god classes
- Sharing State
    - Static classes are a preferred way for maintaining state

# Chapter 8 Errors and Exceptions

### Exceptions in Python

- Exception - an interruption in normal processing, typically caused by an error condition that can be handled by another part of the program

**Catching Exceptions**

- Error handling - we surround the potential exception in a try except block
    - This catches the exception for us to handle
    
    ```python
    while True:
        try:
            user_input = input("Enter a number: ")
            number = int(user_input)
            break
        except ValueError:
            print("Error: Invalid input. Please enter a valid integer.")
    ```
    
- Easier to ask forgiveness than permission (EAFP) vs Look before you leap (LBYL)
    - Error handling is expensive, just only on the exceptions

**Multiple Exceptions**

- The try statement is not limited to a single error type
    
    ```python
    try:
        x = int(input("Enter a number: "))
        y = int(input("Enter another number: "))
        result = x / y
        print("Result:", result)
    except ZeroDivisionError:
        print("Error: division by zero")
    except ValueError:
        print("Error: invalid input")
    except Exception as e:
        print("Error:", e)
    ```
    

Beware the Diaper Antipattern

- A normal try except block without specifying the exception type will catch ALL exceptions
- ALWAYS EXPLICITLY CATCH A PARTICULAR EXCEPTION TYPE
    - Any error you cant forsee is a bug that needs to be resolved
    - Also, this can cause some weird issues

### Raising Exceptions

- You can also raise exceptions to indicate the occurance of a provlem that your code cannot recover from automatically
    
    ```python
    def my_function(value):
        if value < 0 or value > 100:
            raise ValueError("Value must be between 0 and 100")
        # function logic here
    
    ****************if condition:
    	    raise TypeError #also, do not need to specify a custom error message****************
    ```
    

### **Using Exceptions**

- Just like everything else in python, exceptions are objects that you can both use and extract information from
- You can use this to see if a key exists in a dictionary
    
    ```python
    my_dict = {"key1": "value1", "key2": "value2", "key3": "value3"}
    
    try:
        value = my_dict["key4"]
    except KeyError:
        print("KeyError: Key does not exist in dictionary")
    else:
        print(f"The value for key4 is {value}")
    ```
    

### Exceptions and Logging

- An introduction to the python Logging Module
- There are five different levels of severity
    - `DEBUG`: Detailed information, typically of interest only when diagnosing problems.
    - `INFO`: Confirmation that things are working as expected.
    - `WARNING`: An indication that something unexpected happened or indicative of some problem in the near future (e.g. ‘disk space low’).
    - `ERROR`: Due to a more serious problem, the software has not been able to perform some function.
    - `CRITICAL`: A very serious error, indicating that the program itself may be unable to continue running.
- The logging.basicConfig() function allows to configure the logging level, and specify things like which file to write the log to
    
    ```python
    logging.basicConfig(filename='log.txt', level=logging.INFO)
    ```
    
- By passing lnfo, we are telling the logging module to log all messages of that level, as well as the three messages above it (warning error critical)
    - If we leave the filename blank, it is just printed to the console
- With user-input, be careful for catching KeyboardInterrupt and EOFErrors!

Bubbling Up

- The issue with this form of logging is that any exceptions not accounted for wont be logged
- Any error  you catch can be re-raised, or bubbled up
    - Since the error isnt handled after being re-raised, the program terminates
    
    ```python
    def divide(a, b):
        try:
            result = a / b
        except ZeroDivisionError:
            print("Error: Cannot divide by zero")
            raise
        return result
    
    def calculate():
        try:
            result = divide(10, 0)
        except ZeroDivisionError:
            print("Error: Cannot perform calculation")
        else:
            print(f"The result is {result}")
    ```
    

### Exception Chaining

- If you catch one exception and raise another, you are at risk of losing the context of the original error
- Python offers exception chaining:
    
    ```python
    def foo():
        try:
            bar()
        except Exception as e:
            raise ValueError("An error occurred in foo") from e
    
    def bar():
        try:
            baz()
        except Exception as e:
            raise TypeError("An error occurred in bar") from e
    
    def baz():
        raise RuntimeError("An error occurred in baz")
    
    try:
        foo()
    except Exception as e:
        print(type(e).__name__ + ": " + str(e))
        print(type(e.__cause__).__name__ + ": " + str(e.__cause__))
        print(type(e.__cause__.__cause__).__name__ + ": " + str(e.__cause__.__cause__))
    ```
    

### Else and Finally

- There are two more exception handling clauses
    - **Else** - used for any section of code that should only run if NONE of the except clauses caught anything
    
    ```python
    try:
        x = int(input("Enter a number: "))
        y = int(input("Enter another number: "))
        result = x / y
    except ValueError:
        print("Invalid input.")
    except ZeroDivisionError:
        print("Cannot divide by zero.")
    else:
        print("The result is:", result)
    ```
    
    - **Finally** - executed regardless of whether  an exception is raised or not. It is often used to perform some cleanup or finalization tasks that need to be executed no matter what.
        - “after everything, no matter what”
    
    ```python
    try:
        file = open("data.txt", "r")
        data = file.read()
        print(data)
    except FileNotFoundError:
        print("File not found.")
    finally:
        file.close()
    ```
    

### Creating Exceptions

- You can inherit from one of the many provided exception classes
- All error-type exception classes inherit from the Exception class, which inherits from the BaseException class.
    - When making a custom, avoid inheriting from BaseException  as that class is not designed to be inherited from by custom classes
- Examples of exception types - [https://docs.python.org/3/library/exceptions.html](https://docs.python.org/3/library/exceptions.html)

# Chapter 9 Collections and Iteration

- While Loops
    - Executes as long as the condition is true
    - Can use an **else** clause after the while loop
        - Will enter as long as the loop is not aborted with a break, return, or raised exception
- For loops
    - The primary focus for for loops is to ITERATE over a collection of values
    - Just like while loops, the for loop also has an else statement

### Collections

- A collection is a container object containing one or more items organized in some fashion
    - 5 - Tuple, list, set, dictionary, deque

**Tuples**

- An array like collection that is **immutable** - once created items cannot be added, modified, or destroyed
- Tuples are used for heterogeneously typed, sequentially ordered data.
    - Such as when you need to keep different but asocited values together.
    - Comma separated values enclosed in parens
    
    ```python
    order = ('Jason', 'Hot Chocolate', 12)
    print(order[1])
    ```
    
- Because the contents are ordered, you can index the individual items by index, or subscript

**Named Tuples**

- Allows you to define a tuple-like collection with named fields.
- Also immutable
- Functionality is adding keys to the values while maintaining subscriptable behavior
    
    ```python
    from collections import namedtuple
    
    # Define a named tuple
    Person = namedtuple('Person', ['name', 'age', 'gender'])
    
    # Create an instance of the named tuple
    person1 = Person(name='Alice', age=25, gender='female')
    
    # Access elements by name
    print(person1.name)    # Output: Alice
    print(person1.age)     # Output: 25
    print(person1.gender)  # Output: female
    ```
    
- Can access the values within person1 by field name or by subscript

**Lists**

- Lists are  sequence collections that are mutable, meaning items can be added, removed, and reordered
- Homogeneously typed, sequentially ordered data
- Comma-separated sequence, enclosed in square brackets
    - Used to implement **Stacks** (LIFO) and **queues** (FIFO)
    - pop, append, insert are common functions
    - .remove removes the first item in the list that meets that valaue
    - del mylist[i] removes that index

**Deques**

- Optimized for accessing the first and last items of a collection
    - This makes it a good option for implementing stacks and queues
    - introduces the .appendleft () and .popleft() functions on top of the current ones

**Sets**

- Mutable unordered collection where all items are guaranteed to be unique
- Every item stored in a set must be hashable - having a  hash value that never changes during its lifetime
    
    ```python
    raffle = {'James', 'Daniel', 'Arthur'}
    ```
    
- Can add to the set with the .add() function
- Can remove from the set with the .remove() function
- .pop() pops the first item in the set, which is RANDOM. Not guaranteed for same output

**Frozenset**

- The immutable twin to the set. Same thing but cannot change
- Both support SET MATHMATICS
    - Union (|)
    - Intersection (&)
    - Difference (-)
    - Symmetric difference (^)
    - Check if one set is subset (< or ≤)
    - Check if one set is superset (> or ≥)

**Dictionaries**

- Mutable collection that stores data in key-value pairs, instead of linear fashion
- Associative manner of storage is MAPPING
- Hashable types must be of the key
    - Remember, hashable types are almost always immutable
- The value in the pair can be anything
- Access and insert with mydict[key] = value
- .items() returns a dict to a list of key value tuples
- Check or Except?
    - Do we check for a key in the dictionary directly with the in operator or use a tryerror with a keyerror instead?
    - LBYL (check if it is in) or EAFP (catch the thrown exception)
- There are a variety of dictionary varients - defaultdict, **ordereddict** and counter
    - Starting with 3.7, all dicts maintain order by insertion

### Unpacking Collections

- All collections can be unpacked into multiple variables, meaning each item is assigned to its own name
    
    ```python
    menu = {'drip': 1.95, 'cappuccino': 2.95, 'americano': 2.49}
    
    a, b, c = menu.items()
    print(a)  # prints ('drip', 1.95)
    print(b)  # prints ('cappuccino', 2.95)
    print(c)  # prints ('americano', 2.49)
    
    (a_name, a_price), (b_name, b_price), *_ = menu.items()
    print(a_name)   # prints 'drip'
    print(a_price)  # prints 1.95
    print(b_name)   # prints 'cappuccino'
    print(b_price)  # prints 2.95
    
    # or, with a deque
    
    customers = deque(['Kyle', 'Simon', 'James'])
    customers.append('Daniel')
    
    # first, second, third, _ = customers
    # first, second, _, _ = customers
    first, second, *rest = customers
    print(first)   # prints 'Kyle'
    print(second)  # prints 'Simon'
    print(rest)    # prints ['James', 'Daniel']
    
    first, *middle, last = customers
    print(first)   # prints 'Kyle'
    print(middle)  # prints '['Simon', 'James']
    print(last)    # prints 'Daniel'
    
    *_, second_to_last, last = customers
    print(second_to_last)  # prints 'James'
    print(last)            # prints 'Daniel'
    
    ```
    
- There is one major limitation - you have to know how many values you are unpacking
    - If we specify too little or too many arguments, we will get a valueerror
- The “_” ignores that item
- We can unpack the rest using a **Starred Expression**
    - first, middle, *last → The first two values are unpacked, and the remaining are sent into last (in a list)
    - first, *middle, last → unpacks the first and last value, and then creates a list of everything in between
    - *_, second_to_last, last → unpacks the last two, and we ignore everything else.

**Unpacking Dictionaries**

- Unpacks the keys
- If you want the values instead,  must unpack using *dictionary view*
- Can also unpack the key value pairs
    
    ```python
    menu = {'drip': 1.95, 'cappuccino': 2.95, 'americano': 2.49}
    
    a, b, c = menu.values()
    print(a) # prints 1.95
    print(b) # prints 2.95
    print(c) # prints 2.49
    
    a, b, c = menu.items()
    print(a)  # prints ('drip', 1.95)
    print(b)  # prints ('cappuccino', 2.95)
    print(c)  # prints ('americano', 2.49)
    
    (a_name, a_price), (b_name, b_price), *_ = menu.items()
    print(a_name)   # prints 'drip'
    print(a_price)  # prints 1.95
    print(b_name)   # prints 'cappuccino'
    print(b_price)  # prints 2.95
    ```
    

### Structural Pattern Matching on Collections

- In patterns, tuples and lists are interchangeable. They are both matched against  sequence patterns.
    
    ```python
    order = ['venti', 'no whip', 'mocha latte', 'for here']
    
    match order:
        case ('tall', *drink, 'for here'):
            drink = ' '.join(drink)
            print(f"Filling ceramic mug with {drink}.")
        case ['grande', *drink, 'to go']:
            drink = ' '.join(drink)
            print(f"Filling large paper cup with {drink}.")
        case ('venti', *drink, 'for here'):
            drink = ' '.join(drink)
            print(f"Filling extra large tumbler with {drink}.")
    ```
    
- Sequence patterns are the same whether they are tuples or lists
- We can also do the same thing with a **mapping pattern**
    
    ```python
    order = {
        'size': 'venti',
        'notes': 'no whip',
        'drink': 'mocha latte',
        'serve': 'for here'
    }
    
    match order:
        case {'size': 'tall', 'serve': 'for here', **rest}:
            drink = f"{rest['notes']} {rest['drink']}"
            print(f"Filling ceramic mug with {drink}.")
        case {'size': 'grande', 'serve': 'to go', **rest}:
            drink = f"{rest['notes']} {rest['drink']}"
            print(f"Filling large paper cup with {drink}.")
        case {'size': 'venti', 'serve': 'for here', **rest}:
            drink = f"{rest['notes']} {rest['drink']}"
            print(f"Filling extra large tumbler with {drink}.")
    ```
    
- Mapping patterns must be wrapped in curley braces
    - Order does not matter, just keys and value associations

Accessing by Index or Key

- Collections are subscriptable, meaning individual items can be accessed via an specified index with square brackets.
- Implement special methods
    - __getitem__()
    - __setitem__()
    - __delitem__()
        - All accept a single integer argument
        - Implemented by the dict class with key instead of an integer arg

### Slice Notation

- Allows you to access specific items or ranges
    - Only tuple and list can be sliced
- Use square brackets around the slice notation
    
    ```python
    [start:stop:step]
    # start is inclusive, stop is exclusive
    # meaning, it goes up to not including stop
    
    my_list = [1, 2, 3, 4]
    print(my_list[1:3]) # [2, 3] -> everyhting  UP TO INDEX 3
    
    my_list = [1, 1, 2, 3, 5, 8, 13, 21]
    print(my_list[1:]) # prints everything other than the element at index 0
    print(my_list[:4] # prints the first four elements
    
    # Lets say we want to print every other element
    my_list = [0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89, 144]
    print(my_list[::2])  # the third number is the step
    
    # you can skip the inputs
    [start::step]
    [:stop]
    [start:]
    [::step]
    [:stop:step]
    ```
    
- START IS INCLUSIVE, STOP IS EXCLUSIVE

### Negative Indices

- Use negative numbers as indices to count backwards from the end of the list
- Also works in slicing, and steps
- A negative step argument reverses the direction of the list

```python
my_list = [1, 1, 2, 3, 5, 8, 13, 21]

print(my_list[::-1]) # prints the list in reverse
print(my_list[-3:] # prints the last 3 items (-3 is at value 8)
```

- HOWEVER, the reverse behavior affects the values we use for start and stop
    - So,  if you have the step as a negative, you flip the indicies

```python
print(my_list[3:6:-1] # returns an empty list, as it starts at 3 and counts down from there
print(my_list[5:2:-1] # now it gives me what I want

my_new = my_list[:] # returns a new list 
	# However, this is a shallow copy
```

- Slicing always returns a new list, or a tuple with the selected items.
    - Therefore, we can copy with slicing

### Using islice

- You can slice non iterable objects using itertools.islice()
    - Behaves the same, except without negative numbers
    
    ```python
    islice(collection, start, stop, step)
    
    menu = {'drip': 1.95, 'cappuccino': 2.95, 'americano': 2.49}
    menu = dict(islice(menu.items(), 0, 3, 2))
    print(menu)
    ```
    
- Here, we can take a slice from a dictionary
- islice returns an itertools slice object → must do it on the items and not the values

### The In Operator

- You can use this operator to check if a value is in a collection.
- Can also check if a list omits a particular item: not in

**Checking Collection length**

- len produces the length
- However, you do not need to use len if you are simply checking to see if there are any values in the collection:

```python
customers = []
if customers: # the collection dunder defaults to FALSE if the item is empty
	print("There are people here")
else:
	print("The store is empty")

# same thing as...
print(bool(customers)) # False
```

- Because customers is empty, it is “falsely”
- If we directly cast customers to a boolean value, it is false

### Iteration

- Collections are designed to work with iteration
    - The ability to access each element one by one

**Iterables and Iterators**

- An *iterable* is any objet whose items or values can be accessed one at a time, on demand
    - For an object to be iterable, it must have an associated iterator, which is returned from the object’s instance method __iter__()
- An *iterator* is the object that performs the actual iteration, providing ready access to the next item in the iterable it is traversing
    - To be an iterator, the object must implement the special function __next__(), which accepts no parameters and returns a value
    - These, like iterables, implement the __iter__()

**Manual Iteraion**

```python
specials = ["latte", "espresso", "cold brew"]
first_iterator = specials.__iter __()
sceond_iterator = specials.__iter__()

item = first_iterator.__next__()
print(item) # latte
```

- The first call to next will print the first item in the list
- Any subsequent calls moves the iterator forward
- When the iterator reaches the end of the list, it raises a stopIteration

**Automatic Iteration**

- We can use the python built-in functions iter() and next()
    
    ```python
    first_iterator = iter(specials)
    second_iterator = iter(specials)
    
    print(type(first_iterator)) # prints <class 'list iterator'>
    item = next(first_iterator)
    print(item) # prints latte
    item = next(first_iterator)
    print item # prints espresso
    
    # We can simplify this expression
    
    iterator = iter(specials)
    while True:
    	try:
    		item = next(iterator)
    	except StopIteration:
    		break
    	else:
    		print(item)
    ```
    
- Although it is nice to know how to manually handle iterators …. we dont need to use them
- The for loop does this for us

### Iterating with For Loops

- A nice thing in python is that you never need a counter variable for loop control
    - Mainly because iterables directly control for loops
    
    ```python
    customers = ["dan",  "mark", "brenda", "jake"]
    for customer in customers
    	print(f"welcome to the store {customer}!!")
    ```
    
- We loop through the customers list, which is an iterable. On each iteration, the loop binds the next to the specified delimiter customer.

### Sorting collections in loops

- The sorted() function returns a new sorted list
    
    ```python
    	for _, drink in sorted(customers, key=lambda x: x[1]):# the key takes in what to sort by, as customers is a list of tuples
    		print(f'{drink}')
    ```
    
- We can pass in a key function → in this case it is a lambda function. It must accept an item as an argument and return the value I want to sort that item by
    - customers remains unchaged! Remember, it is returning a new list

### Enumerating Loops

- In  python, remember, there is a way to get around indexing. You NEVER need a counter variable for loop control
- We can use enumeration
    
    ```python
    for number, (customer, drink) in enumerate(customers, start=1):
    	print(f"{number}. {customer}: {drink})
    ```
    
- enumerate() returns a tuple with the count (which is sometimes, coincidentally, the index) as an integer in the first position and the item from the collection in the second
    - It will normally start at 0 → but here we started at 1

### Mutation in Loops

- You can not mutate a collection while ITERATING over it.
- Therefore, we cannot use a for loop and modify a collection

```python
for customer, drink in customers:
			customers.popleft() #runtime error
```

- You can:
    1. Make a copy of the collection before iterating over it (iterate over the copy, mutate the original)
        1. This is generally better → especially if we are going to be adding to the collection
    2. Use a while loop instead of a for loop!

### Loop Nesting and Alternatives

- Avoid doing this. Flat is better than nested
- It is impossible to break out of nested loops. The continue and break keywords only apply to the direct lauer of the loop

### Iteration Tools

- There are a number of iteration tools that are built into the language itself.
    - **all**(), **any**(), enumerate(), max(), min(), range(), reversed(), sorted(), sum()
- range() → an iterable that returns a sequence of integers from an optional starting value

### Filter

- Allows you to search for values in an iterable that fit a particular criterion.
    
    ```python
    orders = ['cold brew', 'frappe', 'green tea', 'matcha', 'hot chocolate', 'americano', 'latte', 'chai', 'water', 'tea', 'drip coffee', 'drip tea']
    
    drip_orders = list(filter(lambda x: 'drip' in x, orders))
    
    print(f'There are {len(drip_orders} that are drip orders')
    ```
    
- The callable can be a function or a lambda - this is similar to the sorted operator as well!

### Map

- The map iterable will pas every item in an iterable to a callable as an argument.
    - map(function, positonal arguments based on the function)
- Then, it will pass the returned value back as its own current iterative value
    
    ```python
    def brew(order)
    	print(f"Making {order}")
    	return order
    
    for order in map(brew, orders):
    	print(f"One order is ready!")
    
    costs = [1, 2, 3, 6.50, 4, 60]
    tip = [2, 3, 2, 6, 7, 4, 20]
    
    totals = list(map(add, costs, tip))
    ```
    

### Zip

- Combines multiple iterables together. On each iteration, it takes the next value for each iterable in turn and packs them all together into a tuple.
- This is particularly useful if you want to create a dictionary from multiple lists, although you could populate any collection using a zip

```python
regulars = ['Dan', 'Will', 'Josh', 'Devin', 'Jake', 'Jesi', 'Mark', 'Brenda']
usuals = ['chicken', 'burger' ,'sandwich' ,'salad' ,'shake' ,'fries' ,'chips' ,'bread']

usual_orders = dict(zip(regulars, usuals))
{ 'Dan': 'chicken'
	'Will': 'burger' ...}
```

- Excess values in these (including map) are ignored
- Take a look at **itertools**
    - accumulate chain, combinations, dropwhile, filterfalse, islice, permutations, product, starmap, takewhile

### Custom Iterable Classes

- This is a good project to do.

# Chapter 10 Generators and Comprehensions

- The solution to nested loops is to employ generator expressions
    - Allows you to write the entire logic of a loop into a single statment
- Lazy evaluation - A process in which an iterator does not provide the next value until it is requested
- Collection iterables evaluate all of their items upon creation
    - i.e., if you have function executables in the collection, they are executed
    - This behavior means that collections have the potential to become performance bottlenecks when working with large amounts of data
    - One of the best ways to handle a lot of data is with generators

### Infinite Iterators

- Provides values on demand without ever being exhaused
- itertools offers three
    - count() - counts between values forever
    - cycle() - cycles over the values in a list, forever
    - repeat() -  repeats a single value, forever

### Generators

- A powerful alternative to the iterator class - a generator function
    - BASICALLY A FUNCTION THAT ACTS AS AN ITERATOR TO A COLLECTION
    - INSTEAD OF RETURNING A COLLECTION, IT RETURNS AN ITERATOR
- Looks like a normal function, but with the yield keyword
- When a function is called directly, it returns a generator iterator, or a generator object, that encapsulates the logic from the suite of the generator function
- Therefore, the result of calling the function is an iterator, which you have to call next() on to get the next values
    
    ```python
    from itertools import product
    from string import ascii_uppercase as alphabet
    
    def gen_license_plates(): #this is the generator function
        for letters in product(alphabet, repeat=3):
            letters = "".join(letters)
            if letters == 'GOV':
                continue
    
            for numbers in range(1000):
                yield f'{letters} {numbers:03}'
    
    license_plates = gen_license_plates()
    
    # for plate in license_plates:
    #     print(plate)
    
    registrations = {}
    
    def new_registration(owner):
        if owner not in registrations:
            plate = next(license_plates) #have to get the next plate
            registrations[owner] = plate
            return plate
        return None
    
    # Fast-forward through several results for testing purposes.
    for _ in range(4441888):
        next(license_plates)
    
    name = "Jason C. McDonald"
    my_plate = new_registration(name)
    print(my_plate)
    print(registrations[name])
    ```
    

### Closing Generators

- When you are done with an interator you should close it.

```python
from random import choice

colors = ['red', 'green', 'blue', 'silver', 'white', 'black']
vehicles = ['car', 'truck', 'semi', 'motorcycle', None]

def traffic():
    while True:
        vehicle = choice(vehicles)
        color = choice(colors)
        try:
            yield f"{color} {vehicle}"
        except GeneratorExit:
            print("No more vehicles.")
            raise

def car_wash(traffic, limit):
    count = 0
    for vehicle in traffic:
        print(f"Washing {vehicle}.")
        count += 1
        if count >= limit:
            traffic.close()

queue = traffic()
car_wash(queue, 10)
```

- As seen here, we can treat the generator object kind of like a colletion, as we are just calling next() on the iterator

### Yield From

- Basically, returning a function call
- yield from <call to another generator> → Will yield the yield of that generator
    - Allows for return nesting
    
    ```python
    def numbers_up_to(n):
        i = 0
        while i < n:
            yield i
            i += 1
    
    def square_numbers_up_to(n):
        for num in numbers_up_to(n):
            yield num*num
    
    def print_square_numbers_up_to(n):
        yield from square_numbers_up_to(n)
        print("Done printing square numbers up to", n)
    
    for num in print_square_numbers_up_to(5):
        print(num)
    ```
    

### Generator Expressions

- An iterator that wraps the entire logic of a generator into a single expression

```python
squares = (x*x for x in range(10))

for num in squares:
    print(num)

#OR

license_plates = (
	f'ABC {number:03}'
	for number in range(1000)
)

for plate in license_plates:
	print plate

# OR
from itertools import product
from string import ascii_uppercase as alphabet

license_plates = (
	f'{"".join(letters)} {number:03}'
	for letters in product(alphabet, repeat=3)
	for number in range(1000)
) # This is nested
```

- Remember, all generator objects are lazy!
    - Therefore, the collection is not checked on initialization
    - So, if we have executables, they will not be executed until they are ‘hit’
    
    ```python
    sleepy = (time.sleep(t) for t in [1, 2, 3, 4, 5])
    
    for s in sleepy:
        print("sleeping")
    
    # Output
    # sleep 1 sec
    # Sleeping
    # sleep 2 sec
    # Sleeping 
    # ....
    ```
    
    - Unlike a list, whose very definition caused the program to sleep for three seconds before continuing, because every item is being evaluated at definition, this code runs instantly
        - As it defers evaluation of its values until they are needed
        - Defining the generator itself does not execute time.sleep()

### Conditionals in Generator Expressions

```python
from itertools import product
from string import ascii_uppercase as alphabet

license_plates = (
	f'{"".join(letters} {numbers:03}'
	for letters in product(alphabet, repeat=3)
	if letters != ('G', 'O', 'V')
	for numbers in range(1000)
)

# The equivalent generator function would look something like this
def get_license_plate_numbers():
	for letters in product(alphabet, repeat=3):
		if letters != if letters != ('G', 'O', 'V')
			for numbers in range(1000):
				yield f'{"".join(letters} {numbers:03}'
```

- We can also treat it like list comprehension:

```python
divis_by_three = (n for n in range(100) if n % 3 == 0)
```

- HOWEVER, the generator expression does not support the ‘else’ clause → every compound statement may only have one clause in the generator expression

### List Comprehensions

- If you wrap a generator expression in square brackets, instead of parentheses, you create a list comprehension, which uses the enclosed generator expression to populate the list
- This is the most common and popular use of generator expressions
- However, list comprehensions are eager because lists are eager

```python
orders = ['coffee', 'tea', 'oj', 'juice', 'chai latte', 'medium drip', 'french pres', 'dark roast drip', 'pumpkin spice latte']

drip_orders = [order for order in orders if 'drip' in order]
```

### Set Comprehensions

- You can also create a set comprehension by enclosing in curly braces
- This will use a generator expression to populate a set
- There are no surprises here, just the same as before. Remember sets do not have duplicate numbers, and they are unordered

```python
odd_remainders = {100 % divisor for divisor in range(1, 100, 2)}
```

### Dictionary Comprehensions

- A dictionary comprehension follows almost the same structure as a set comprehension,  but it requires colons.

```python
squares = {n: n**2 for n in range(1, 101)}
print(squares[7]) # 49
```

- Very simple
- Be careful using comprehensions → They do not replace loops, and they can become hard to debug very easily!
    - They can become overused and strung out into huge lines

### Simple Coroutines

- The coroutine is a type of generator that consumes data on demand, instead of producing data, and it waits patiently until it receives it
- There are two types of coroutines, simple and asynchronous coroutines
    - These will refer to the simple ones
- Coroutines are generators - so you can use *close()* and *throw()* with them. You can also *yield*, and control can be handed off to another coroutine using *yield from*
    - Only **next()** will not work here, since you are sending values rather than retrieving them. Instead,  you use **send()**
    
    ```python
    from random import choice
    
    def color_counter(color):
    	matches = 0
    	while True:
    		vehicle = yield
    		if color in vehicle:
    			matches += 1
    		print(f'{matches} so far.')
    
    colors = ['red', 'green', 'blue', 'purple']
    vehicles = ['car', 'truck', 'semi', 'motorcycle']
    
    def traffic():
    	while True:
    		vehicle = choice(vehicles)
    		color = choice(colors)
    		yield f'{color} {vehicle}'
    
    counter = color_counter("red")
    counter.send(None) # Priming the coroutine
    # also can do this: next(counter)
    
    for count, vehicle in enumerate(traffic(), start = 1):
    	if count < 100:
    		counter.send(vehicle)
    	else:
    		counter.close()
    		break
    ```
    
    - Iterate over the traffic() generator, sending each value to the coroutine counter by using the send() method. The coroutine processes the data and prints out the current count of red vehicles.

Returning Values from Coroutines

- Normally we want to be able to retrieve the data being produced
    - Not just printing

```python
def color_counter(color):
	matches = 0
	while True:
		vehicle = yield matches
		if color in vehicle:
			matches += 1
```

- We place the name matches after the yield keyword to indicate that I want to yield the value bound to that variabale
    - A generator can both accept and return values with the **yield** statement

```python
matches = 0
for count, vehicle in enumerate(traffic(), start = 1)
		if count < 100:
			matches = counter.send(vehicle)
		else:
			counter.close()
			break

print(f'We found {matches} matches')
```

- Every time a value is sent with .send(), a value is also returned and assigned to matches

# Chapter 11 Text IO and Context Managers

- Text-based files are the most common way to store data, and is the key to retaining state between program runs
    - *Streams* and *path-like objects*

### Standard Input and Output

**Standard Streams**

- When you use print() you send the string to the standard output stream, a special communication channel provided by your operating system
- The stream behaves like a queue/buffer. You push data, usually strings, to the stream, and these strings can be picked up inn order by other programs or processes, notably the terminal
    - Remember the FD table and streams? Piping between processes? IPC?
- Your system sends all strings from print() to the **standard output stream by default**
- Your system also has a standard error stream to display all error messages.
    - Normal output is sent to the standard output stream, and error related output is sent to the standard error stream
- We can pipe these streams into one another, such as output to the terminal to outputting to a filepath

```python
print("Normal message")
print("A scary error occured")

$ python3 print_error.py > output.txt 2> error.txt
```

- Here, error.txt is empty, as we didn’t write anything to the standard error stream

```python
import sys
print("Normal message")
print("This is a scary message", file=sys.stderr) # this specifies the file stream
		# sys.stdin
		# sys.stdout
		# sys.stderr
```

- This will now output the error message to error.txt
- Lets take a look back at C
    - In C, the file descriptor table is a data structure maintained by the operating system to keep track of all open files in a process. Each **file descriptor** is a small integer that is used to refer to a specific open file
    - Each process has its OWN FD table. This is crutial for I/O in C
- The FD table is an array of file descriptor entries, where each entry contains information about a specific open file. The operating system uses the file descriptor to access the file, and the process can use the file descriptor to read from, write to, or close the file.
    - In C, the standard streams (stdin, stdout, and stderr) → (0, 1, 2) are opened automatically when a program starts, and they are assigned the file descriptors 0, 1, and 2, respectively.
        - These file descriptors are usually used to perform input and output operations in the program
- Note, in python file I/O is buffered by default, so data written to a file may not be immediately visible until the buffer is FLUSHED.

### Flush

- Standard streams are implemented as *buffers*
    - Data can be pushed to a buffer, which behaves like a queue.
    - That data will wait there until it is picked up by the terminal - or whatever process or program is intended to display it.
- In the case of standard output and error - text is pushed to the stream buffer, and then the buffer is flushed when its contents are all printed to the terminal.
    - Sometimes, we don’t see output! This means that the file stream buffer has not been flushed.
    - Remember, like in C, we do not flush the buffer until we get the ‘\n’ character.
    - This is the same with python. If we specify end=’’ in a print() statement, we do not flush the buffer stream.
- The buffer is flushed with the newline character, or at the end of the program (program termination)
- The end= keyword prevents a newline from being printed
    - Flush = determines if the output is going to be flushed, regardless of the characters in the buffer
- If I want all of my print calls to flush every time be default,  you can run python in *unbuffered mode*
    - pass -u to the interpreter

**Printing Multiple Values**

```python
print(val1, val2, val3, ...) # will print all of these values casted to strings and concatenated

# We can specify what to separate each value with with the sep argument
print(val1, val2, val3, sep='\t') 
```

### Streams

- Stream, or file object or file-like object
- Two types of streams
    - Binary Streams
    - Text Streams - Handle the encoding and decoding of text from binary
- We can create a stream with the built-in **open()** function

```python
house = open("my_house_file.txt") # returns a stream object
print(house.read())
house.close()
```

- It is important not to leave the file closing to the garbage collector, as it is neither guaranteed to work nor portable across all python implementations
    - Also, when writing to a file, it is not guaranteed python will flush that buffer stream to file until .close() is called
    - Changes will be lost
- You can only READ from this specific stream. If we want to write to a file, we have to open a new stream.

### Context Manager Basics

- A context manager is a type of object that automatically handles its own cleanup tasks when program execution leaves a certain section of code, or CONTEXT
- This context can be provided with the python **with** statement.
- In the previous example, there is a lot of room for errors and exceptions.
    - i.e., if we hit an exception,  we will never get to the close() action
- To get around this, we can wrap the close() call in the finally clause of a try statement
    
    ```python
    house = open("my_house_file.txt")
    try:
    	print(house.read())
    finally:
    	house.close()
    ```
    
- However, as all stream objects are context managers, we don’t even need to close the file ourselves - the CM handles it for us:
    
    ```python
    with open("my_house.txt") as house:
    	print(house.read())
    ```
    
- Opens the file, binds the stream to house, and then runs the line of code for reading and printing from that file.
- DO NOT EVER USE WITH for STANDARD STREAMS

### File Modes

```

with open("my_file", <mode>)

 r   Open text file for reading.  The stream is positioned at the
     beginning of the file.

 r+  Open for reading and writing.  The stream is positioned at the
     beginning of the file.

 w   Truncate file to zero length or create text file for writing.
     The stream is positioned at the beginning of the file.

 w+  Open for reading and writing.  The file is created if it does not
     exist, otherwise it is truncated.  The stream is positioned at
     the beginning of the file.

 a   Open for writing.  The file is created if it does not exist.  The
     stream is positioned at the end of the file.  Subsequent writes
     to the file will always end up at the then current end of file,
     irrespective of any intervening fseek(3) or similar.

 a+  Open for reading and writing.  The file is created if it does not
		 exist.  The stream is positioned at the end of the file.  Subse-
     quent writes to the file will always end up at the then current
	   end of file, irrespective of any intervening fseek(3) or similar.

                  | r   r+   w   w+   a   a+   x   x+
------------------|----------------------------------
read              | +   +        +        +        +
write             |     +    +   +    +   +    +   +
write after seek  |     +    +   +             +   +
create            |          +   +    +   +
truncate          |          +   +
position at start | +   +    +   +             +   +
position at end   |                   +   +

- read - reading from file is allowed
- write - writing to file is allowed
- create - file is created if it does not exist yet
- truncate - during opening of the file it is made empty (all content of the file is erased)
- position at start - after file is opened, initial position is set to the start of the file
- position at end - after file is opened, initial position is set to the end of the file
```

- In a stream, a *position* dictates where you read to and write from within the file. The **seek()** method allows you to change this position
    - By default, this will start at the beginning or the end of the function
- You can also use the mode= parameter to switch between text mode (t) which is the default, and binary mode (b).
    - mode=’r+t’ opens the file in read-write textmode
        - mode=’r+’ is the same, as the default is textmode
    - mode=’r+b’ opens the file in read-write binary mode

### Reading Files

- To read from a file, you first acquire a stream, opening it in one of the readable modes.
    - There are four ways to read
    1. read()
    2. readline()
    3. readlines()
    4. iteration

**Read()**

```python
with open('my_house_file.txt', 'r') as house:
	contents = house.read(20) # read max number of chars. Leave empty to read in entire file
	print(type(contents)) # class str
	print(contents)
```

- This reads the entire contents into the var contents string
- We can print  a raw string with repr() to see the raw output  → maintains newline characters and special chars
- We can pass in a size argument, which reads the maximum number of characters

**Readline()**

```python
with open('my_file.txt', 'r') as house:
	line1 = house.readLine()
	line2 = house.readLine()
	line3 = house.readLine()
```

- The house stream remembers my position in the file, so after each call to readline(), the stream position is set to the beginning of the next line.
    - Also has a size parameter for the maximum number of characters to read into the buffer

**Readlines()**

```python
with open('my_file.txt', 'r') as house:
	lines = house.readlines()
	for line in lines:
		print(line.strip())
```

- Reads the lines into a list of strings
    - Also has a  size parameter

### **Reading with Iteration**

- Streams are iterators. Therefore, we can iterate over a stream directly!

```python
with open('my_file.txt', 'r') as house:
	for line in house:
		print(line.strip())
```

- This is the same output as readlines()
- This is a cheaper and more efficient soution

### Stream Position

- After every read and write operation, your position in the stream will change.
    - Use the **tell()** and **seek()** methods to work with the stream position.
    - **tell()** returns the current stream position as a positive integer representing the number of characters from the start of the file
    - **seek()** allows you to move back and forth within the stream, character by character
        - Takes a positive number as the new position to move to, represented by the number of characters from the beginning

### Writing Files

- You are always OVERWRITING, never inserting! This is different than appending to a file
- Try not to write to a file in place.
- make sure you know the placement of the stream when writing to a file!

**write()**

- Writes a string to a file, and returns the number of characters written
- Will overwrite anything in the way
    - To prevent data loss, read the file into memory, modify the data there, and then write it back out to the same file

```python
# Open a file in write mode
file = open("example.txt", "w")

file.seek(0) # moves the stream pointer to position 0

# Write some data to the file
file.write("This is some text.\nThis is some more text.")

# Truncate the contents of the file up to the first 1- characters
file.truncate()

# Close the file
file.close()
```

- **[Truncate](https://www.w3schools.com/python/ref_file_truncate.asp)** removes everything from the current stream position through
    - Here, it removes everything from the current stream position through to the end of the file (or the number of bytes to preserve, i.e. the number of characters). This defaults to the current position passed by tell()
    - We do this because if we want to modify the file, the excess data may exceed the length of the string we are writing with

**writelines()**

- Takes an array of strings, writes that array of strings to the file delimited by NOTHING.
- Same output as write(), except can take an array as a parameter

### Writing files with print()

- You can change the output stream by specifying a parameter to the print function
- This is just like in c

```python
# Open a file in write mode
with open('example.txt', 'w') as file:
    # Use print to write to the file
    print('Hello, world!', file=file)
    print('This is a test.', file=file)
```

- Set the file parameter to the file stream object
- Python always handles the \r\n newline delimiter, for windows vs UNIX systems
- Therefore, we only ever have to worry about the newline character

### Context Manager Details

- The with statement is not limited to streams, but can be a context manager in a variety of different situations
    - Especially any try-finally logic
- CMS implement the __enter__() and __exit__() special methods
    - These are known as the **Context Manager Protocol**
- You can use multiple context managers in a with statement.
- Lets say you want to be able to read from two files at once

```python
# Open two files in the same with statement
with open('file1.txt', 'r') as file1, open('file2.txt', 'w') as file2:
    # Read data from file1 and write it to file2
    data = file1.read()
    file2.write(data)
```

## Paths

- UNIX systems use the POSIX file path conventions
    - Windows uses a completely different scheme, with backslashes instead of forward slashes
- Python offers two modules for pathing
    - os - this is the standard for working with filepaths (os.path)
        - Considered a “junk drawer”, as it is a bunch of legacy code for interacting with the operating system
    - pathlib - introduced in Python 3.4 and became fully supported in python 3.6
- Pathlib is better for maintainability, readability, and performance.

### Path Objects

- These are path-like classes, inheriting from the os.Pathlike abstract class
- These are not based on strings, but they are their own objects
- There are two types - pure paths and concrete paths

**Pure Paths**

- Represents a path and allows you to work with it without accessing the underlying filesystem
- Basically, just a string that represents a file path, but does not necessarily correspond to a real file or directory
    - Will automatically instantiate a PurePosixPath or PureWindowsPath under the hood

```python
from pathlib import PurePath
path =  PurePath('../some_file.txt')

with open(path, 'r') as file:
	print(file.read())

# create empty file if non exists
path.touch() # fails on pure paths!
```

- Only useful really for opening

**Concrete Paths**

- Refers to a file path that points to a real file or directory on the file system
    - Provides methods for interacting with the filesystem

```python
from pathlib import Path
path = Path('../some_file.txt')

with open(path, 'r') as file:
	print(file.read())

path.touch() # okay in path!
```

- We can use methods directly on the path  object to interact with the filesystem

### Path Types

- Absolute
    - Starts at the root of a filesystem
    - Always starts with an anchor and end with the name - the full filename
    - Suffix is the filetype, i.e. .txt file or .py
    - stem is the name before the nonleading “.”
    - You can retrieve these parts of the path with pathlib
- Relative

### Parts of a POSIX Path

- The root/anchor only contains the anchor “/”
- The name consists of a stem and a suffix, if there are multiple suffixes
    - Remember, it takes the first dot as the stem. So, if your filename has a dot, be weary

## Creating a Path

- You can define a path using the desired clas initializer, PureWindowsPath or PosixPath, by passing the path as a string.

```python
from pathlib import PosixPath

path = PosixPath('/home/json/.bash_history')
```

- After initializing the path-like object and binding it to path, I can open it.
    - Pass it to open()
    - Use the functionally identical open() method pn the path object

```python
with path.open('r') as file:
	for line in file: 
		continue
	print(line.strip())
```

- We can join two different paths withj the joinpath() method
- You can also use the forward slash operator ‘/’ to make it easier to join path like objects to one another or even to strings

```python
path =  PosixPath.home() / '.bash_history'
# same as
path = PosixPath.joinpath(PosixPath.home(), '.bash_history')
```

### Relative Paths

- A relative path is one that starts from the current position opposed to the root of the filesystem
    - Based on the CWD (current working directory)
    - PWD → print working directory
- Single dot is the current directory
- Double dot is the parent directory

### Paths Relative to Package

- A common trap to fall into is to assume that the current  working directory is the location of the current or main python module
    - That is not the case!
    - If set up correctly, a python module can be set up from anywhere in your system
    - Therefore, all paths must be relative to that location
- We never want to use relative paths for this case. We ALWAYS want to get the full path.
- Again, the current working directory is where the module is invoked from, not where it lives.
    - All paths are relative to that current working directory
- The SOLUTION is to base your relative path off of the special dunder __file__ attribute of modules
    - Contains the absolute path to the module on the current system

```python
from pathlib import Path

path = Path(__file_.resolve()
path = path.parents[1] / Path('resources/context/content.txt')
```

- Convert the file attribute to a path object, and resolve() converts it to the absolute path.

Path Operations

- There are a variety of path operations in the pathlib library for performing many common file operations.
- mkdir, rename, replace, unlink, glob, iterdir, touch, symlink_to, link_to
- exitst(), is_file(), is_dir(), is_symlink(), is_absolute()

- Path.replace() is generally better to use than write9)
    - As if the operation fails (lack of disk space, crashed system, etc.), the original file or directory is left unchanged
    - Write continuously flushes the buffer to the output file. This sort of fault prevention is not in place

### File Formats

- JSON (Javascript Object Notation) is a very common filetype
- The process of converting an object or data structure into a format that can be easily stored or transmitted, such as a byte stream, a string, or a file.
    - The serialized data can then be deserialized, or converted back into the original object or data structure.
- There is a built-in json module that allows to easily convert data between JSON data and many built-in Python data types and collections.
    - However, serialization and deserialization are not perfect inverses

| Python → | JSON → | Python |
| --- | --- | --- |
| dict | object (all keys are strings) | dict |
| list
tuple | array | list |
| bool | boolean | bool
 |
| str | string | str |
| int
int-derived enums | number (int) | int |
| float
float-derived enums | number (real) | float |
| None | null | None |
- Anything direcrly derived from these Python types can also be JSON-serialized, but all other objects cannot be serialized to JSON on their own and must be converted to a type that can be.
    - To make a custom class JSON-serializable, you would need to define a new object that subclasses json.JSONEncoder and overrides its default() method

### Writing to JSON

- json.dump() function to convert data to JSON format and write it to a file
- You can also use json.dumps() to create and write JSON code to a string if you want to wait and write it to a stream later.

```python
import json

nearby_properties = {
    "N. Anywhere Ave.":
    {
        123: 156_852,
        124: 157_923,
        126: 163_812,
        127: 144_121,
        128: 166_356,
    },
    "N. Everywhere St.":
    {
        4567: 175_753,
        4568: 166_212,
        4569: 185_123,
    }
}

with open('nearby.json', 'w') as jsonfile:
    json.dump(nearby_properties, jsonfile)
```

- dumps works the same way, except you just get a string instead of writing to a stream

### Reading from JSON

- You can directly deserialize a JSON file into the corresponding Python object using the json.load() function, which accepts the source stream object as an argument.

```python
import json

with open('nearby.json', 'r') as jsonfile:
    nearby_from_file = json.load(jsonfile)

for k1, v1 in nearby_from_file.items():
    print(repr(k1))
    for k2, v2 in v1.items():
        print(f"{k2!r}: {v2!r}")
```

Other formats

- Csv - comma seprated values
    - one of the most common structured text formats
- INI - storing configuration files
    - use the configparser for working with these formats
- XML - Structured markup language based on the idea of tags, elements, and attributes.
    - Shunned from JSON for simplicity of usage and security
- HTML
- YAML

# Chapter 12 Binary and Serialization

### Binary Notation and Bitwise

- Binary - open and closed, correspond to the open and closed position of gates on circuit boards.
- Sometimes, manipulating it directly is the best way to solve a problem.
-