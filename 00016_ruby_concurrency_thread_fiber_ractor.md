````md
# 00013_method_lookup_chain_ans.md

# Method Lookup Chain

## Answer

The **method lookup chain** in Ruby defines the order in which Ruby searches for a method when it is called on an object. Understanding this chain is crucial for **debugging, inheritance, mixins, and metaprogramming** in Ruby and Rails.

---

### 1. Ruby’s Object Model

- Every object in Ruby is an instance of a **class**, and every class is an object of class `Class`.
- Classes can inherit from a **superclass**, forming an inheritance hierarchy.
- Ruby also allows **modules** to be included or prepended into classes, affecting method lookup.

---

### 2. Method Lookup Path

When a method is called:

1. Ruby searches for the method in the **object’s singleton class** (if defined on a single object).
2. Searches the **class of the object**.
3. Searches **included modules** in **reverse order of inclusion**.
4. Moves up to the **superclass**, repeating the module lookup at each level.
5. If no method is found, Ruby raises a `NoMethodError`.

---

### 3. Example of Method Lookup

```ruby
module A
  def hello; "A"; end
end

module B
  def hello; "B"; end
end

class Parent
  def hello; "Parent"; end
end

class Child < Parent
  include A
  include B
end

c = Child.new
c.hello  # => "B"
````

**Explanation:**

* `Child` includes `A` and `B`.
* Modules are searched **last included first**.
* Lookup path: `Child` → `B` → `A` → `Parent` → `Object` → `Kernel` → `BasicObject`.

---

### 4. Using `ancestors` to Inspect Lookup Chain

```ruby
Child.ancestors
# => [Child, B, A, Parent, Object, Kernel, BasicObject]
```

* Returns the exact order Ruby will follow to resolve method calls.

---

### 5. Singleton Methods and Eigenclasses

* Methods defined on a **single object** exist in its **singleton class**:

```ruby
obj = "Hello"
def obj.greet; "Hi"; end
obj.greet  # => "Hi"
```

* Singleton class is **searched first** before the object’s class.

---

### 6. Prepend vs Include

* **Include** – modules are inserted **after the class** in the lookup chain.
* **Prepend** – modules are inserted **before the class**, overriding methods in the class.

```ruby
module M
  def hello; "M"; end
end

class C
  def hello; "C"; end
end

class D < C
  prepend M
end

D.new.hello  # => "M"
```

* Prepending is often used for **decorating or overriding behavior**.

---

### 7. Super Keyword

* Calls the **next method** in the lookup chain:

```ruby
module M
  def greet
    "Hello from M"
  end
end

class C
  include M
  def greet
    super + " and C"
  end
end

C.new.greet  # => "Hello from M and C"
```

* Ensures proper **chain execution** in modules and inheritance.

---

### 8. Kernel and BasicObject

* All Ruby objects eventually inherit from `BasicObject`.
* `Kernel` module provides methods like `puts`, `print`, `p` and is included in `Object`.
* `BasicObject` is the **minimal base class** with very few methods.

---

### 9. Practical Implications

* Understanding method lookup helps in:

  * **Debugging overridden methods**
  * Proper use of **mixins (include/prepend)**
  * **Metaprogramming** with `method_missing` or dynamic method definitions
  * Avoiding conflicts between modules and parent classes

---

### 10. Summary

* Ruby searches for methods using the **method lookup chain**:

  1. Singleton class (if any)
  2. Class of the object
  3. Included modules (last included first)
  4. Superclass and modules
  5. Object → Kernel → BasicObject
* **`ancestors`** shows the exact path.
* **Prepend** overrides class methods, **include** is searched after the class.
* `super` calls the next method in the chain.
* Mastery of the method lookup chain is essential for **inheritance, mixins, and dynamic behavior** in Ruby.

---

**End of Method Lookup Chain**

```

This is ready to save as `00013_method_lookup_chain_ans.md`.  

Do you want me to continue with **topic 14: Reflection & Introspection** next?
```
