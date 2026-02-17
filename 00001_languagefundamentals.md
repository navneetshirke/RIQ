````md
# 00001_language_fundamentals_ans.md

# Language Fundamentals

## Answer

Language fundamentals in Ruby form the foundation for understanding how the language works, how code is executed, and how objects, data types, control structures, and methods interact. These fundamentals are critical to mastering both Ruby itself and frameworks built on top of it, such as Rails.

---

### 1. Ruby Execution Model

Ruby is an **interpreted, dynamically typed, object-oriented language**. When you run a Ruby program, it goes through several internal steps:

1. **Source Code Reading** – Ruby reads your `.rb` files as text.
2. **Lexical Analysis** – The lexer converts the text into tokens (identifiers, operators, literals).
3. **Parsing** – The parser converts tokens into an **Abstract Syntax Tree (AST)**, representing the logical structure of the code.
4. **Bytecode Compilation** – The AST is translated into bytecode instructions executed by the **YARV (Yet Another Ruby VM)**.
5. **Execution** – YARV handles method calls, stack management, object creation, and memory allocation.

Ruby supports multiple implementations:

- **MRI / CRuby** – The reference implementation.
- **JRuby** – Runs on JVM and interoperates with Java.
- **TruffleRuby** – Optimized for performance.

Ruby behaves like an interpreted language at the user level but internally performs compilation to bytecode for execution efficiency.

---

### 2. Syntax Rules

Ruby syntax prioritizes readability and expressiveness:

- Parentheses are optional for method calls.
- Semicolons are generally optional.
- Every expression evaluates to a value; the last evaluated expression is automatically returned.
- Blocks, iterators, and method arguments support flexible syntax for concise code.

Example:

```ruby
def sum(a, b)
  a + b
end
# sum returns a + b automatically
````

Statements and expressions are nearly interchangeable:

```ruby
result = if true
  10
else
  20
end
# result is 10
```

---

### 3. Variables, Scope, and Naming Conventions

Ruby determines scope using naming conventions:

| Type              | Prefix   | Scope                         |
| ----------------- | -------- | ----------------------------- |
| Local Variable    | `name`   | Method or block               |
| Instance Variable | `@name`  | Object instance               |
| Class Variable    | `@@name` | Shared across class hierarchy |
| Global Variable   | `$name`  | Entire runtime                |
| Constant          | `NAME`   | Global or class/module scope  |

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

Local variables exist only inside the method or block. Instance variables store object state. Class variables are shared across the class hierarchy. Constants begin with an uppercase letter and are accessible within the scope.

---

### 4. Data Types and Objects

Ruby treats **everything as an object**, including numbers, booleans, and classes. Core object types include:

* **Integer, Float, String, Symbol, true, false, nil**
* **Array, Hash, Range** for collections
* **Custom classes and modules**

Symbols are immutable, memory-efficient identifiers. Strings are mutable.

```ruby
:name.object_id == :name.object_id  # true
"string".object_id != "string".object_id  # false
```

---

### 5. Operators

**Arithmetic:** `+`, `-`, `*`, `/`, `%`
**Comparison:** `==`, `!=`, `<`, `>`, `<=`, `>=`
**Logical:** `&&`, `||`, `!`
**Conditional Assignment:** `||=` assigns only if variable is nil or false
**Spaceship `<=>`** returns -1, 0, or 1 for comparisons

Equality methods:

* `==` compares values
* `eql?` compares values and types
* `equal?` compares object identity

---

### 6. Type System & Duck Typing

Ruby is:

* **Dynamically typed** – types are checked at runtime
* **Strongly typed** – no unsafe implicit conversions
* **Duck typed** – behavior matters more than class

```ruby
def print_name(obj)
  obj.name
end
```

Any object responding to `name` works, regardless of its class.

---

### 7. Control Flow

**If / Else / Elsif**

```ruby
if age > 18
  "Adult"
elsif age > 13
  "Teen"
else
  "Child"
end
```

**Unless** – executes when condition is false:

```ruby
puts "Valid" unless user.nil?
```

**Ternary Operator:** `condition ? if_true : if_false`
**Case Statement:** uses `===` internally for flexible comparisons

---

### 8. Loops & Iterators

**while, until, loop**:

```ruby
while i < 5
  i += 1
end
```

Ruby favors **enumerable iteration**:

```ruby
[1, 2, 3].each { |n| puts n }
```

Control keywords:

* `break` – exits loop
* `next` – skips iteration
* `redo` – repeats iteration without rechecking condition

---

### 9. Methods

Methods define behavior:

```ruby
def greet(name = "Guest")
  "Hello #{name}"
end
```

* Implicit return – last expression is returned
* Predicate methods end with `?`
* Bang methods (`!`) usually modify the receiver

---

### 10. Blocks, Procs & Lambdas

**Blocks** are anonymous chunks of code passed to methods:

```ruby
3.times { puts "Hi" }
```

**Procs** – reusable blocks:

```ruby
p = Proc.new { |x| x * 2 }
p.call(3)  # 6
```

**Lambdas** – stricter form with argument checking:

```ruby
l = ->(x) { x * 2 }
l.call(4)  # 8
```

Blocks, Procs, and Lambdas are fundamental to iterators, callbacks, and DSLs.

---

### 11. Input & Output

* `puts` – prints with newline
* `print` – prints without newline
* `p` – prints debug representation

Input:

```ruby
name = gets.chomp
```

String interpolation:

```ruby
"Hello #{name}"
```

---

### 12. Comments

* Single-line: `# comment`
* Multi-line: `=begin ... =end`

Used for documentation and code clarity.

---

### 13. Truthiness

Only `false` and `nil` are falsy. Everything else evaluates to true:

```ruby
0, "", [], {}  # all true
```

---

### 14. File Loading

* `require` – loads once
* `require_relative` – loads relative to current file
* `load` – loads every time

---

### 15. Error Handling

**Syntax errors:** detected before execution
**Runtime errors:** detected during execution (`NoMethodError`, `ZeroDivisionError`)

Handled with:

```ruby
begin
  risky_operation
rescue => e
  puts e.message
end
```

---

### 16. Summary

Ruby Language Fundamentals cover:

* Execution model (YARV, bytecode)
* Everything is an object
* Variables, constants, and scope
* Data types and operators
* Control structures and loops
* Methods, blocks, Procs, and Lambdas
* Input/output and string interpolation
* Truthiness and error handling
* File loading and module usage

Mastering these basics is critical for moving into **object-oriented design**, **Rails framework internals**, and **advanced Ruby techniques**.

---

**End of Language Fundamentals**

```