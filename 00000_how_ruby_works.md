# 🧠 How Ruby Executes a `.rb` File (Complete Mental Model)

This explains **what actually happens internally** when we run a Ruby file.

When you run:

```bash
ruby app.rb
```

Ruby does **NOT** immediately execute code line-by-line.

Instead, Ruby follows a **step-by-step execution pipeline**.

---

# ✅ Big Picture (Remember This First)

Ruby execution flow:

```
Ruby Code (.rb file)
        ↓
Read as Text
        ↓
Tokenization (Lexer)
        ↓
Parsing (Syntax Understanding)
        ↓
AST (Code Structure Tree)
        ↓
Bytecode Compilation (YARV)
        ↓
Ruby Virtual Machine executes code
```

👉 Ruby code is first **understood**, then **converted**, then **executed**.

---

# 1️⃣ Ruby Loads the File

When you run:

```bash
ruby app.rb
```

Ruby:

* starts a Ruby process
* opens the file
* reads file as **plain text**

Example file:

```ruby
puts "Hello"
a = 5 + 2
```

At this moment Ruby only sees:

```
characters
```

NOT programming logic yet.

---

# 2️⃣ Lexer (Tokenization)

Ruby breaks text into **tokens**.

Tokens = smallest meaningful parts of code.

Example:

```ruby
a = 5 + 2
```

Becomes internally:

```
IDENTIFIER(a)
ASSIGN(=)
INTEGER(5)
PLUS(+)
INTEGER(2)
```

👉 Like breaking a sentence into words.

Ruby now understands **symbols**, not raw text.

---

# 3️⃣ Parser (Syntax Check)

Ruby checks:

> “Is this valid Ruby grammar?”

If syntax is wrong:

```ruby
a =
```

Ruby stops immediately:

```
SyntaxError
```

Execution never starts.

---

## AST — Abstract Syntax Tree

Parser builds a structure called **AST**.

Example:

```ruby
a = 5 + 2
```

Ruby understands it like:

```
Assignment
 ├── variable a
 └── addition
      ├── 5
      └── 2
```

👉 Ruby now understands **meaning of code**.

---

# 4️⃣ Compilation to Bytecode (YARV)

Ruby is interpreted, but internally it first compiles code.

AST → **Bytecode**

Ruby uses:

```
YARV = Yet Another Ruby Virtual Machine
```

Example (conceptual):

```
putobject 5
putobject 2
opt_plus
setlocal a
```

Bytecode is:

* lower than Ruby language
* higher than machine code

This makes execution faster.

---

# 5️⃣ Ruby Virtual Machine Executes Code

Now execution really starts.

Ruby VM:

* runs instructions
* manages stack
* calls methods
* creates objects
* handles loops and variables

Example:

```
push 5
push 2
add values
store result
```

Ruby VM works like a small CPU inside Ruby.

---

# 6️⃣ Everything in Ruby is an Object

Very important rule:

> EVERYTHING is an object in Ruby.

Example:

```ruby
5 + 2
```

Ruby actually runs:

```ruby
5.+(2)
```

Meaning:

* `5` → Integer object
* `+` → method call
* result → new object

Operators are just methods.

---

# 7️⃣ Method Lookup

When Ruby sees:

```ruby
puts "Hello"
```

Ruby searches for the method.

Lookup order:

```
Current object
↓
Class
↓
Included modules
↓
Superclass
↓
Kernel
↓
BasicObject
```

It finds:

```
Kernel#puts
```

Then executes it.

---

# 8️⃣ Memory & Object Creation

When objects are created:

```ruby
name = "Ruby"
```

Ruby:

1. creates object in memory (heap)
2. variable stores reference to object

```
name → memory address → object
```

Variables hold references, not raw values.

---

# 9️⃣ Garbage Collection (Automatic Memory Cleanup)

Ruby automatically removes unused objects.

Example:

```ruby
a = "hello"
a = nil
```

Old string has no reference → Garbage Collector deletes it later.

No manual memory management needed.

---

# 🔟 Sequential Execution (Important Interview Point)

Ruby executes code **top → bottom**.

Method exists only when execution reaches it.

Example:

```ruby
puts hello

def hello
  "Hi"
end
```

❌ Error occurs.

Because method is defined later.

Correct:

```ruby
def hello
  "Hi"
end

puts hello
```

Ruby does NOT scan entire file before running.

---

# 1️⃣1️⃣ Program Exit

After execution finishes:

Ruby:

* runs `at_exit` hooks
* cleans memory
* stops process

---

# ✅ Final Execution Order (MEMORIZE THIS)

```
1. Ruby reads file as text
2. Lexer creates tokens
3. Parser checks syntax
4. AST is created
5. Code compiled to YARV bytecode
6. Ruby VM executes bytecode
7. Objects created during runtime
8. Garbage Collector manages memory
9. Program exits
```

---

# 🧠 Best Memory Trick (1-Line Model)

```
Ruby code → Bytecode → Ruby Virtual Machine → Execution
```

---

# ⭐ Interview One-Line Answer

Ruby reads source code, converts it into tokens, builds an AST, compiles it into YARV bytecode, and executes it inside the Ruby Virtual Machine where everything runs as objects and memory is managed by garbage collection.

---

## ✅ How to Remember Forever

Whenever you run Ruby, imagine:

```
READ → UNDERSTAND → CONVERT → EXECUTE
```

That is exactly how Ruby works internally.

---
---

# 🧠 Ruby Execution Internals — Level 2 (Deep Understanding)

This explains what happens **after Ruby starts running your code**.

Most developers know Ruby syntax.
Senior developers understand **how Ruby executes context, loads files, and builds classes at runtime**.

---

# ✅ 1️⃣ Ruby Executes Inside an Object (`main`)

This surprises many developers.

When Ruby starts executing a file:

```bash
ruby app.rb
```

Ruby does NOT run code in empty space.

It creates a special object called:

```
main
```

Your entire top-level code runs inside this object.

---

## Example

```ruby
puts self
```

Output:

```
main
```

Meaning:

```
self = main
```

So Ruby execution always has a current object.

---

## Why This Matters

When you write:

```ruby
def hello
  "Hi"
end
```

You are actually defining a method on:

```
Object class (via main context)
```

That’s why methods become globally accessible.

---

# ✅ 2️⃣ What is `self` During Execution?

`self` means:

> “Current object executing the code”

Ruby changes `self` depending on context.

---

## Top-level

```ruby
self
# => main
```

---

## Inside a class

```ruby
class User
  puts self
end
```

Output:

```
User
```

Here:

```
self = User class object
```

---

## Inside instance method

```ruby
class User
  def show
    self
  end
end

User.new.show
```

Now:

```
self = instance of User
```

---

### Memory Rule (Very Important)

```
Top level → self = main
Inside class → self = class
Inside method → self = object instance
```

Interviewers LOVE this question.

---

# ✅ 3️⃣ How Ruby Loads Files (`require`, `load`, `autoload`)

Rails boot heavily depends on this.

---

## 🔹 `require`

```ruby
require "json"
```

### Behavior:

* loads file ONLY once
* remembers loaded files
* prevents duplicate loading

Ruby stores loaded files in:

```ruby
$LOADED_FEATURES
```

Example:

```ruby
puts $LOADED_FEATURES
```

Why important?

👉 Prevents redefining classes multiple times.

---

## 🔹 `load`

```ruby
load "test.rb"
```

### Behavior:

* loads file EVERY time
* no memory of previous load

Used mainly for:

* development
* reloading code

---

## 🔹 `autoload`

Lazy loading mechanism.

```ruby
autoload :User, "user.rb"
```

File loads only when constant is first used:

```ruby
User.new
```

Rails autoloading (Zeitwerk) is built on this concept.

---

### Simple Memory Rule

```
require   → load once
load      → load every time
autoload  → load when needed
```

---

# ✅ 4️⃣ How Class Definitions Actually Execute

This is a BIG hidden concept.

Many developers think class is just structure.

❌ Wrong.

In Ruby:

> A class definition is executable code.

---

## Example

```ruby
class User
  puts "Class is executing"
end
```

Output immediately:

```
Class is executing
```

Why?

Because Ruby executes class body at runtime.

---

### What Ruby Actually Does

```ruby
User = Class.new do
  puts "Class is executing"
end
```

Class is just an object creation.

---

## Important Consequence

Inside class:

```
self = class object
```

So:

```ruby
class User
  def self.info
    "hello"
  end
end
```

means:

```ruby
User.define_singleton_method(:info)
```

---

# ✅ 5️⃣ How Method Definitions Work Internally

When Ruby reaches:

```ruby
def hello
  "Hi"
end
```

Ruby:

1. creates method object
2. attaches it to current class
3. updates method lookup table

Method exists ONLY after execution reaches that line.

---

## Example (Important)

```ruby
hello

def hello
  "Hi"
end
```

Error occurs because method not defined yet.

Ruby executes sequentially.

---

# ✅ 6️⃣ Constant Lookup (Hidden Ruby Rule)

Ruby resolves constants using nesting.

Example:

```ruby
module A
  VALUE = 10

  class B
    puts VALUE
  end
end
```

Ruby searches:

```
Current scope
→ Parent scope
→ Object
```

This is why Rails namespaces work.

---

# ✅ 7️⃣ How Rails Boot Uses Ruby Execution

Rails boot is just Ruby execution repeated many times.

When you run:

```bash
rails server
```

Ruby basically does:

```
require config/boot.rb
require rails
require application.rb
load initializers
load models/controllers
start server
```

Rails is NOT magic.

It is just thousands of `require` calls.

---

# ✅ 8️⃣ Why Rails Boot Takes Time

Because Ruby:

* reads many files
* tokenizes each
* builds AST
* compiles bytecode
* executes class bodies

Large Rails apps = thousands of files loaded.

---

# ✅ FINAL MENTAL MODEL (VERY IMPORTANT)

Ruby execution always follows:

```
Code runs inside an object (self)
Classes are executed code
Methods are added during execution
Files are loaded dynamically
Everything happens at runtime
```

---

# ⭐ Senior Interview One-Line Answer

Ruby executes code inside a current object (`self`), loads files dynamically using require/load, treats class definitions as executable code, defines methods during runtime, and resolves constants and methods using lookup chains.

---

# 🧠 Memory Shortcut (Remember Forever)

```
Ruby is not compiled structure.
Ruby is runtime object construction.
```

---

Next Level (🔥 REAL SENIOR RUBY — most devs never learn):

✅ Method lookup chain deep dive (ancestors, modules, prepend)
✅ How Ruby finds a method internally step-by-step
✅ Singleton classes & eigenclass (MOST CONFUSING TOPIC)
✅ Why DSLs like Rails work (`has_many`, `validates` magic)

Say **“next level”** and we go into the part that makes Ruby finally “click” permanently.


✅ Rails Boot Process (What Happens When Server Starts)

When you run:

bin/rails server

Rails:

1 Loads Ruby environment

2 Loads gems (Bundler)

3 Loads application config

4 Loads routes

5 Loads initializers

5 Builds middleware stack

6 Starts Puma server

7 After boot, Rails waits for requests.


Request Lifecycle Summary (Memorize)
HTTP request
 → Puma
 → Rack
 → Middleware
 → Router
 → Controller
 → Model/DB
 → View render
 → Layout
 → Response
 → Browser