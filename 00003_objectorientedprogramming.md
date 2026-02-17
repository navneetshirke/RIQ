````md
# 00003_objectoriented_programming_ans.md

# Object-Oriented Programming (OOP)

## Answer

Object-Oriented Programming (OOP) in Ruby is a programming paradigm that organizes code around **objects**, which encapsulate both **data** (state) and **behavior** (methods). Ruby is a **pure object-oriented language**, meaning everything is an object—including numbers, strings, and even classes themselves. Understanding OOP in Ruby is essential for building maintainable, reusable, and scalable applications, especially in Rails.

---

### 1. What is OOP?

OOP focuses on:

- **Objects** – instances representing entities
- **Classes** – blueprints for objects
- **Encapsulation** – hiding internal state
- **Abstraction** – exposing only necessary behavior
- **Inheritance** – sharing behavior across classes
- **Polymorphism** – objects behaving differently via same interface

Ruby uses **classes and modules** to implement OOP principles.

---

### 2. Classes and Objects

**Class** – defines a blueprint for objects:

```ruby
class User
end
````

**Object** – an instance of a class:

```ruby
user = User.new
```

* `User` is a class
* `user` is an object (instance)

---

### 3. Object Creation (Instantiation)

Objects are created using `.new`. When a class is instantiated:

1. Memory is allocated for the object
2. Instance variables are initialized
3. `initialize` method (constructor) is called

```ruby
class User
  def initialize(name)
    @name = name
  end
end

user = User.new("Alice")
```

* `@name` is an instance variable storing object state

---

### 4. Instance Variables and Methods

* **Instance variables (`@var`)** – store object-specific data
* **Instance methods** – define behavior for individual objects

```ruby
class User
  def initialize(name)
    @name = name
  end

  def greet
    "Hello, #{@name}"
  end
end

user = User.new("Alice")
user.greet  # => "Hello, Alice"
```

Each object has its own copy of instance variables.

---

### 5. Encapsulation

Encapsulation hides internal object state and exposes controlled access via **methods**.

Ruby supports method visibility:

* `public` – accessible everywhere
* `private` – accessible only inside object
* `protected` – accessible to instances of same class

Example:

```ruby
class BankAccount
  def deposit(amount)
    update_balance(amount)
  end

  private

  def update_balance(amount)
    @balance += amount
  end
end
```

---

### 6. Accessor Methods

Ruby provides shortcuts to read and write instance variables:

* `attr_reader` – read-only
* `attr_writer` – write-only
* `attr_accessor` – read and write

```ruby
class User
  attr_accessor :name
end

user = User.new
user.name = "Alice"
puts user.name
```

---

### 7. Abstraction

Abstraction hides implementation details and exposes only what is necessary:

```ruby
class Car
  def start
    ignite_engine
  end

  private

  def ignite_engine
    "Engine started"
  end
end
```

Users interact with `start` without worrying about engine mechanics.

---

### 8. Inheritance

Inheritance allows a class to **reuse behavior** of another class:

```ruby
class Animal
  def speak
    "Some sound"
  end
end

class Dog < Animal
end

Dog.new.speak  # => "Some sound"
```

* `Dog` inherits methods from `Animal`
* Supports code reuse and hierarchy modeling

---

### 9. Method Overriding

Child classes can redefine parent methods:

```ruby
class Dog < Animal
  def speak
    "Bark"
  end
end
```

* Ruby uses dynamic dispatch to determine which method to execute at runtime.

---

### 10. `super` Keyword

Calls the parent class implementation:

```ruby
class Dog < Animal
  def speak
    super + " Bark"
  end
end
```

* Combines parent and child behavior

---

### 11. Polymorphism

Polymorphism allows objects of different types to respond to the same method differently:

```ruby
class Cat
  def speak
    "Meow"
  end
end

class Dog
  def speak
    "Bark"
  end
end

[Cat.new, Dog.new].each { |animal| puts animal.speak }
# => Meow
# => Bark
```

* Ruby relies on **duck typing**: “If it quacks like a duck, it’s a duck.”

---

### 12. Duck Typing

Ruby cares about **behavior, not class**:

```ruby
def make_sound(animal)
  animal.speak
end
```

* Any object implementing `speak` works, regardless of class.

---

### 13. Class Methods

Methods that belong to the class itself rather than instances:

```ruby
class User
  def self.count
    10
  end
end

User.count  # => 10
```

Alternative:

```ruby
class << User
  def count; 10; end
end
```

* Implemented via the class’s **singleton (eigen) class**

---

### 14. `self` Keyword

`self` refers to:

* Inside instance method → current object
* Inside class body → class object
* Inside class method → class object

```ruby
class User
  puts self  # => User
end
```

---

### 15. Class Variables and Constants

* **Class variables (`@@var`)** – shared across class hierarchy
* **Constants (`NAME`)** – defined in uppercase, immutable

```ruby
class User
  @@count = 0
  MAX_AGE = 100
end
```

---

### 16. Composition vs Inheritance

* **Inheritance**: “is-a” relationship
  `Dog < Animal` → Dog is an Animal
* **Composition**: “has-a” relationship
  `Car has Engine` → more flexible than deep inheritance chains

---

### 17. Method Visibility

* `public` – default for instance methods
* `private` – cannot use explicit receiver
* `protected` – accessible by instances of same class

```ruby
private :update_balance
```

---

### 18. Object Identity

Every object has a unique ID:

```ruby
obj.object_id
```

Two objects with same value may be different:

```ruby
"a".object_id != "a".object_id
```

---

### 19. Everything is an Object

Ruby treats:

```ruby
1.class         # Integer
true.class      # TrueClass
String.class    # Class
Class.class     # Class
```

Even classes themselves are objects, enabling **metaprogramming**.

---

### 20. Message Passing

Ruby uses **message passing**:

```ruby
user.name
```

* Calls the `name` method on `user`
* Emphasizes OOP paradigm over procedural function calls

---

### 21. Summary

Ruby OOP fundamentals provide:

* Class-based design with objects
* Encapsulation and abstraction
* Inheritance and method overriding
* Polymorphism through duck typing
* Singleton classes and class methods
* Object identity and everything-as-object model

Mastering these concepts is essential for **Rails architecture**, **ActiveRecord design**, and writing reusable, maintainable Ruby code.

---

**End of Object-Oriented Programming**

