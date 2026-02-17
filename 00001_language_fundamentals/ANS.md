Here is a **single complete `.md` file content** — ready to **copy → paste directly** into:

```bash
nano 00001_language_fundamentals.md
```

No extra explanation outside the file. Just paste everything below.

---

````md
# 00001 — Language Fundamentals (Ruby)

Language Fundamentals describe how Ruby works internally as a programming language.
Rails is built on top of Ruby, so understanding these concepts is mandatory for
backend development and technical interviews.

---

# 1. Ruby Execution Model

## What is Ruby?
Ruby is an interpreted, dynamically typed, object-oriented programming language.

Ruby does NOT execute code directly line by line. Instead, it follows these steps:

1. Source code is read.
2. Code is parsed into an Abstract Syntax Tree (AST).
3. AST is compiled into bytecode.
4. Bytecode runs inside the Ruby Virtual Machine (YARV).

## Ruby Implementations
- MRI / CRuby — standard implementation
- JRuby — runs on JVM
- TruffleRuby — high-performance implementation

## Key Concept
Ruby executes through a Virtual Machine, enabling portability and flexibility.

---

# 2. Basic Syntax Rules

Ruby prioritizes readability and developer happiness.

## Rules
- Parentheses are optional.
- Semicolons are optional.
- Everything returns a value.
- Last evaluated expression becomes return value.

Example:

```ruby
def add(a, b)
  a + b
end
````

The result of `a + b` is automatically returned.

---

# 3. Variables & Naming Conventions

Variable type depends on naming style.

| Variable Type | Example  | Scope               |
| ------------- | -------- | ------------------- |
| Local         | name     | method/block        |
| Instance      | @name    | object instance     |
| Class         | @@count  | shared across class |
| Global        | $debug   | entire program      |
| Constant      | MAX_SIZE | global constant     |

Example:

```ruby
class User
  @@count = 0

  def initialize(name)
    @name = name
  end
end
```

---

# 4. Core Data Types

Ruby principle: **Everything is an Object**

## Primitive Objects

* Integer
* Float
* String
* Symbol
* true / false
* nil

## Collections

* Array
* Hash
* Range

Examples:

```ruby
10.class        # Integer
"hello".class   # String
[:a, :b].class  # Array
```

---

# 5. Operators

## Arithmetic

```
+  -  *  /  %
```

## Comparison

```
==  !=  >  <  >=  <=
```

## Logical

```
&&   ||   !
```

## Conditional Assignment

```ruby
name ||= "Guest"
```

Assigns only if variable is nil or false.

## Spaceship Operator

```ruby
5 <=> 3   # => 1
```

Used internally for sorting comparisons.

## Equality Methods

| Method | Meaning                              |
| ------ | ------------------------------------ |
| ==     | value comparison                     |
| eql?   | value and type comparison            |
| equal? | object identity (same memory object) |

---

# 6. Type System & Duck Typing

Ruby is:

* Dynamically typed
* Strongly typed
* Behavior-driven (Duck Typing)

## Duck Typing Principle

Object capability matters, not its class.

```ruby
def print_name(obj)
  obj.name
end
```

Any object responding to `name` works.

---

# 7. Control Flow

## if / elsif / else

```ruby
if age > 18
  "Adult"
elsif age > 13
  "Teen"
else
  "Child"
end
```

## unless

Opposite of if.

```ruby
puts "Valid" unless user.nil?
```

## Ternary Operator

```ruby
status = logged_in? ? "Yes" : "No"
```

## Case Statement

```ruby
case role
when "admin"
  "Full Access"
when "user"
  "Limited Access"
end
```

---

# 8. Loops & Iteration

## while

```ruby
while i < 5
  i += 1
end
```

## until

Runs until condition becomes true.

```ruby
until done
  work
end
```

## loop do

```ruby
loop do
  break if finished
end
```

## Loop Control Keywords

* break → exit loop
* next → skip iteration
* redo → repeat iteration

Preferred Ruby style:

```ruby
[1,2,3].each { |n| puts n }
```

---

# 9. Method Fundamentals

## Method Definition

```ruby
def greet(name)
  "Hello #{name}"
end
```

## Default Arguments

```ruby
def greet(name="Guest")
end
```

## Predicate Methods (?)

Return boolean values.

```
empty?
valid?
present?
```

## Bang Methods (!)

Indicate mutation or dangerous action.

```
map   # non-destructive
map!  # modifies original object
```

---

# 10. Blocks (Core Ruby Feature)

Blocks are anonymous chunks of executable code.

```ruby
3.times { puts "Hello" }
```

## yield

```ruby
def execute
  yield
end
```

## Block Styles

* `{}` → single-line blocks
* `do...end` → multi-line blocks

---

# 11. Input & Output

## Output

```ruby
puts "Hello"   # newline
print "Hello"  # same line
p object       # debug output
```

## Input

```ruby
name = gets.chomp
```

## String Interpolation

```ruby
"Hello #{name}"
```

---

# 12. Comments & Documentation

## Single-line Comment

```ruby
# This is a comment
```

## Multi-line Comment

```ruby
=begin
Multiple
lines
=end
```

---

# 13. Truthiness & Falsiness

Only two values are falsy:

* false
* nil

Everything else is truthy:

```ruby
0      # true
""     # true
[]     # true
```

Important interview concept.

---

# 14. File Loading

## require

Loads file once.

```ruby
require 'json'
```

## require_relative

```ruby
require_relative './user'
```

## load

Reloads file every execution.

---

# 15. Error Basics

## Syntax Error

Occurs before execution.

Example:

```ruby
def test(
```

## Runtime Error

Occurs during execution.

```ruby
10 / 0
# ZeroDivisionError
```

## Common Exceptions

* NoMethodError
* NameError
* ArgumentError
* RuntimeError
* ZeroDivisionError

---

# Why Language Fundamentals Matter in Rails

Rails relies heavily on:

* Ruby blocks
* Dynamic dispatch
* Duck typing
* Object model
* Method lookup

Weak Ruby fundamentals lead to difficulty understanding Rails internals.

---

# Interview Expectations

A strong Rails developer must clearly explain:

* Why everything in Ruby is an object
* Difference between ==, eql?, and equal?
* How Ruby executes code internally
* Truthiness rules
* Blocks and implicit returns

---

# Summary

Language Fundamentals form the base layer of Ruby expertise.
Mastery here enables deeper understanding of:

* Ruby Object Model
* Metaprogramming
* Rails Internals
* ActiveSupport behavior

````
