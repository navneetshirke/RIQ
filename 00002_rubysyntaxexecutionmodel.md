````md
# 00002_ruby_syntax_execution_model_ans.md

# Ruby Syntax & Execution Model

## Answer

Understanding Ruby's syntax and execution model is critical to mastering how Ruby programs run, how methods and expressions behave, and how the Ruby virtual machine interprets code. This knowledge lays the foundation for writing efficient, maintainable, and idiomatic Ruby code.

---

### 1. Ruby Syntax Philosophy

Ruby was designed to be:

- **Readable** – Code should resemble natural language.
- **Expressive** – Developers can write concise and meaningful code.
- **Flexible** – Supports multiple ways to accomplish tasks.
- **Consistent** – Encourages conventions but allows optional syntax for simplicity.

Example:

```ruby
puts "Hello, World!"
puts("Hello, World!")  # Both are valid
````

---

### 2. Expressions and Statements

Ruby treats almost everything as an expression. Every expression evaluates to a value.

```ruby
x = if true
      10
    else
      20
    end
# x == 10
```

* Implicit return: The last evaluated expression in a method is returned automatically.
* Explicit return is optional but can be used to exit early.

```ruby
def square(n)
  return n * n if n > 0
  0
end
```

---

### 3. Method Definitions and Calls

Methods are defined using `def`:

```ruby
def greet(name = "Guest")
  "Hello, #{name}"
end
```

* Optional parentheses in calls:

```ruby
greet("Alice")
greet "Alice"
```

* All operators in Ruby are actually methods:

```ruby
1 + 2   # => 3
1.+(2)  # equivalent
```

* Methods ending with `?` conventionally return booleans.
* Methods ending with `!` conventionally modify the receiver.

---

### 4. Variables and Scope

Ruby determines variable scope by prefix:

| Prefix    | Scope             |
| --------- | ----------------- |
| No prefix | Local variable    |
| `@`       | Instance variable |
| `@@`      | Class variable    |
| `$`       | Global variable   |
| Uppercase | Constant          |

Example:

```ruby
class User
  @@count = 0
  def initialize(name)
    @name = name
    @@count += 1
  end
end
```

Local variables exist only inside methods or blocks, instance variables belong to objects, and class variables are shared in the class hierarchy.

---

### 5. Object Model Basics

* **Everything is an object** – numbers, strings, classes, even `nil` and `true`.
* Methods are messages sent to objects.
* Classes themselves are instances of `Class`.

```ruby
5.class       # => Integer
Integer.class # => Class
Class.class   # => Class
```

---

### 6. Code Execution Steps

When Ruby runs a program:

1. **Parsing** – Code is converted to tokens.
2. **AST Generation** – Abstract Syntax Tree is built.
3. **Bytecode Compilation** – YARV converts AST to bytecode instructions.
4. **Execution** – YARV executes bytecode using a stack-based virtual machine.

Example:

```ruby
a = 5 + 2
# Tokenized as: identifier (a), operator (=), integer (5), operator (+), integer (2)
# Executed via bytecode in YARV
```

---

### 7. Blocks, Procs, and Lambdas

Ruby syntax allows blocks for iteration:

```ruby
[1,2,3].each { |n| puts n }
```

* **Proc**: reusable block object.
* **Lambda**: strict Proc with argument checking and return semantics.

```ruby
p = Proc.new { |x| x*2 }
l = ->(x) { x*2 }
```

---

### 8. Conditional Modifiers

Ruby allows postfix conditionals:

```ruby
puts "Adult" if age > 18
puts "Minor" unless age >= 18
```

---

### 9. Loops

* `while` / `until` loops
* `for` loops
* `loop do ... end` infinite loop
* Iterators preferred with `.each`, `.map`, `.select`

---

### 10. Exception Handling Syntax

```ruby
begin
  risky_code
rescue StandardError => e
  puts e.message
else
  puts "No error"
ensure
  puts "Always runs"
end
```

* `begin ... rescue ... end` for runtime errors.
* `ensure` runs regardless of error.

---

### 11. Method Lookup & Visibility

* Public methods are accessible everywhere.
* Private methods can only be called without explicit receiver.
* Protected methods can be called by instances of the same class.

Ruby determines method calls dynamically via the **method lookup chain**, starting from:

1. Singleton class
2. Class of the object
3. Included modules
4. Superclass chain
5. Object → Kernel → BasicObject

---

### 12. File Loading & Execution

* `require` – load file once
* `require_relative` – load relative to current file
* `load` – reloads file every time

```ruby
require 'json'
require_relative './user'
load './config.rb'
```

---

### 13. Comments & Documentation

* Single-line: `# Comment`
* Multi-line: `=begin ... =end`
* RDoc or YARD for generating documentation from special comment syntax.

---

### 14. Summary

Ruby syntax and execution model provide:

* Highly readable, expressive syntax
* Everything as an object
* Flexible method calls, blocks, and argument handling
* Dynamic method lookup
* Implicit and explicit returns
* Exception handling and runtime safety
* Iterators and enumerables for iteration
* File loading and modularization mechanisms

Understanding this model is essential to master **Ruby object model, metaprogramming, and Rails framework internals**.

---

**End of Ruby Syntax & Execution Model**
