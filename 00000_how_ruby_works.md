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
