````md
# 00001 — Language fundamentals (deep-dive, practical)
## 6) Type system & duck typing
What: Ruby is dynamically typed and uses duck typing: the object's methods matter more than its class.

How (detailed):
- Dynamic typing: Variable names are not bound to types; values carry types at runtime. You only discover type mismatches when you call a missing method.
- Duck typing: If an object responds to the methods your code needs (`respond_to?`), you can use it — no formal interface required.
- Optional checks: `is_a?`, `kind_of?`, and `respond_to?` exist for defensive checks, but rely more on tests and documentation for contracts.

Why it matters (practical):
- Flexibility: Your functions can accept different object types as long as they meet the behavioral contract.
- Safety: Since errors happen at runtime, comprehensive tests and clear method contracts prevent surprises.

Example:
```ruby
def speak(entity)
  if entity.respond_to?(:speak)
    entity.speak
  else
    puts "Entity can't speak"
  end
end

class Dog; def speak; 'woof'; end; end
class Person; def speak; 'hello'; end; end

speak(Dog.new)    # => 'woof'
speak(Person.new) # => 'hello'
```

Common pitfalls:
- Overusing `is_a?` makes code less flexible; prefer behavior checks and good tests.
- Assuming the presence of methods without tests can lead to runtime `NoMethodError`.

Interview tip: "Ruby is duck-typed — design around behavior and test contracts rather than strict class hierarchies."

---

## 7) Control flow
What: Standard branching with `if/elsif/else`, `unless`, `case`, postfix conditionals, and ternary operators.

How (detailed):
- `if/elsif/else`: Primary multi-way branching.
- `unless`: Executes when condition is false; good for simple negative checks and guard clauses.
- Ternary operator: `cond ? a : b` for compact conditionals.
- `case`: Uses `===` internally for `when` checks — you can match ranges, regexes, classes, and any custom `===` behavior.

Why it matters (practical):
- Readability: Choose constructs that minimize nesting and make intent clear (guard clauses reduce indentation).
- Power of `case`: Write concise pattern matching logic using ranges and regexes.

Example:
```ruby
def age_group(age)
  case age
  when 0..12 then 'child'
  when 13..19 then 'teen'
  else 'adult'
  end
end
```

Common pitfalls:
- Using `unless` with `else` can be confusing — prefer `if` for that situation.

Interview tip: "Use guard clauses and `case` for clear logic; remember `case` uses `===` internally."

---

## 8) Loops & iteration
What: Prefer `Enumerable` methods (`each`, `map`, `select`, `reduce`) over manual `while` loops for clarity and composability.

How (detailed):
- `Enumerable` mixin: Implement `each` and you get many traversal methods for free.
- `Enumerator` and `lazy`: Create external iterators and lazy evaluation chains for large or infinite sequences.
- `map` vs `each`: `map` transforms and returns a new array; `each` is for side effects and returns the original collection.

Why it matters (practical):
- Fewer bugs: Higher-level operations express intent and reduce off-by-one and mutation errors.
- Performance: `lazy` avoids intermediate arrays in pipelines.

Example:
```ruby
nums = (1..1_000_000)
result = nums.lazy.map { |n| n*2 }.select { |n| n%3==0 }.first(5)
```

Common pitfalls:
- Forgetting to use `lazy` for big data and unintentionally materializing large arrays.
- Using `map` solely for side-effects (use `each` instead).

Interview tip: "Prefer `Enumerable` methods and `lazy` evaluation for pipelines; know when to use `map` vs `each`."

---

## 9) Methods (definition & conventions)
What: Methods define behavior; Ruby supports positional, default, splat, keyword, double-splat, and block arguments.

How (detailed):
- Positional: `def foo(a, b)` — required positional arguments.
- Default: `def foo(a = 1)` — default values for optional args.
- Splat: `def foo(*args)` — captures extra positional args as an array.
- Keyword: `def foo(x:, y: 0)` — explicit named arguments.
- Double splat: `def foo(**kwargs)` — captures extra keyword args as a hash.
- Block: Accept with `&block` or call with `yield`.

Why it matters (practical):
- API design: Keyword args make calls self-documenting and reduce mistakes.
- Backwards compatibility: Adding keyword args should be done carefully to avoid breaking callers.

Example:
```ruby
def build(name:, size: 10, **opts)
  {name: name, size: size}.merge(opts)
end

Common pitfalls:
```

Common pitfalls:
- Mixing positional and keyword args carelessly in older Ruby versions can cause ambiguity.

Interview tip: "Explain default, splat, and keyword args and why keyword args improve clarity."

---

## 10) Blocks, Procs, Lambdas
What: Three ways to pass executable code: block (implicit), `Proc` (object), and `lambda` (strict Proc).

How (detailed):
- Block: Syntactic construct passed to a method, invoked with `yield`. Only one implicit block per method.
- Proc: `proc` or `Proc.new` — lenient arity; `return` inside a proc attempts to return from the enclosing method (can be surprising).
- Lambda: `->(x){}` or `lambda` — strict arity; `return` exits the lambda, not the enclosing method.

Why it matters (practical):
- Control flow: `return` and `break` behave differently between blocks, procs, and lambdas — important when designing callbacks or DSLs.

Example:
```ruby
def wrapper
  p = Proc.new { return 'from proc' }
  p.call
  'after'
end

def wrapper_lambda
  l = -> { return 'from lambda' }
  l.call
  'after'
end

wrapper          # => 'from proc'
wrapper_lambda   # => 'after'
```

Common pitfalls:
- Unexpected `return` behavior from procs.
- Forgetting that a method accepts only one implicit block — use `&` to pass multiple procs explicitly.

Interview tip: "Lambdas enforce arity and local return behavior; procs are more permissive, which affects how library callbacks behave."

---

## 11) I/O and interpolation
What: `puts`, `print`, and `p` for output; `gets.chomp` for input; `"#{expr}"` for interpolation.

How (detailed):
- `puts` converts argument with `to_s` and adds newline.
- `p` calls `inspect` and is better for debugging complex structures.
- `gets` reads a line including newline; chain `chomp` to remove the newline.

Why it matters (practical):
- Debugging: `p` and `pp` reveal object internals; interpolation avoids messy concatenation.
- Security: Never interpolate untrusted input into commands or SQL — always sanitize or use parameterized queries.

Example:
```ruby
name = gets.chomp
puts "Hello #{name}"   # interpolates using to_s
p [1, {a:1}]            # prints inspect form
```

Common pitfalls:
- Using interpolation in SQL strings — prefer parameterized queries to prevent injection.

Interview tip: "Use `p` or `pp` for debugging; never interpolate user input into SQL or shell commands."
- Missing parentheses causing the block to attach to the wrong method call.
- Relying on implicit returns in very long methods — explicit `return` improves readability in those cases.

Interview tip: "Ruby favors readable syntax with optional parentheses and implicit returns; choose explicit forms where ambiguity might arise."

---

## 3) Variables, scope, naming
What: Variable naming prefixes indicate scope and lifetime: local (no prefix), instance (`@`), class (`@@`), global (`$`), and constants begin with uppercase.

How (detailed):
- Local variables: Only visible inside the method or block where they are defined. They do not persist across method calls.
- Instance variables (`@var`): Each object instance has its own set of instance variables. They represent object state.
- Class variables (`@@var`): Shared across a class and its subclasses. They can be surprising when inheritance is involved because subclasses share the same variable.
- Global variables (`$var`): Accessible anywhere in the program. Generally discouraged — they make code harder to reason about and test.
- Constants (`CONST`): Scoped to the module or class. Ruby will warn on reassignment. Constants are commonly used for configuration values and class-level defaults.

Why it matters (practical):
- Encapsulation: Choosing the right scope controls visibility and reduces accidental coupling.
- Testability: Using globals or shared class variables can leak state between tests.

Example:
```ruby
class User
  @@count = 0
  def initialize(name)
    @name = name
    @@count += 1
  end
  def name
    @name
  end
  def self.count
    @@count
  end
end

u = User.new('a')
User.count  # => 1
```

Common pitfalls:
- Using `@@` for shared state without considering subclass interactions.
- Relying on `$` globals in libraries — it makes them fragile and hard to reuse.

Interview tip: "Prefer instance variables for per-object data and constants for configuration; avoid globals and class variables unless you need shared mutable state."

---

## 4) Data types & objects
What: Everything is an object in Ruby — primitives, collections, and even classes. Objects have methods and state.

How (detailed):
- Built-in types: `Integer`, `Float`, `String`, `Symbol`, `Array`, `Hash`, `Range`, `TrueClass`, `FalseClass`, `NilClass`.
- Immediate values: Some values (small integers, symbols, `nil`, `true`, `false`) are represented as immediate values in MRI, not heap objects. This affects `object_id` and GC behavior.
- Mutability: Strings, arrays, and hashes are mutable. Symbols are immutable and unique for their name.
- Classes and modules: Classes are objects (instances of `Class`). You can define methods on classes (class methods) and metaprogram with them.

Why it matters (practical):
- Memory and performance: Frequent creation of temporary strings increases GC load. Use string interpolation carefully and reuse buffers when needed.
- Correctness: Know the difference between object identity (`equal?`) and value equality (`==`) to avoid bugs in comparisons and collections.

Example:
```ruby
s = "hello"
t = s.dup
s.equal?(t)   # => false (different objects)
s == t        # => true (same content)
:sym.object_id == :sym.object_id  # => true (same symbol)
```

Common pitfalls:
- Mutating shared objects unexpectedly (e.g., modifying a default argument that is a mutable object).
- Assuming symbols are always safe to create dynamically — although modern Ruby GC handles symbols, creating too many dynamically can still be a sign of trouble.

Interview tip: "Everything is an object; be mindful of mutability and immediate values when reasoning about performance and identity."

---

## 5) Operators & equality
What: Operators are method calls; Ruby provides `==`, `eql?`, and `equal?` for different equality checks.

How (detailed):
- Operators as methods: `1 + 2` is `1.+(2)`. This means you can override operator behavior in your classes.
- `==`: General value equality. Classes often override it to represent semantic equality.
- `eql?`: Used by `Hash` to determine key equality; often stricter and paired with `hash`.
- `equal?`: Tests object identity (same object in memory).
- `<=>` (spaceship): Returns -1, 0, or 1; used by `Comparable` to implement `<`, `<=`, `>`, `>=`.

Why it matters (practical):
- Implement `eql?` and `hash` consistently when using objects as hash keys.
- Overriding `==` without updating `eql?`/`hash` can make objects behave strangely in sets and hashes.

Example:
```ruby
class Pair
  attr_reader :a, :b
  def initialize(a,b)
    @a, @b = a, b
  end
  def ==(other)
    other.is_a?(Pair) && a == other.a && b == other.b
  end
  def eql?(other)
    self == other
  end
  def hash
    [a, b].hash
  end
end

p1 = Pair.new(1,2)
p2 = Pair.new(1,2)
p1 == p2        # => true
p1.equal?(p2)   # => false
{ p1 => 'x' }[p2] # => 'x' (works because hash/eql? are consistent)
```

Common pitfalls:
- Forgetting to implement `hash` when overriding `eql?` — causes hash lookup failures.
- Confusing `equal?` (object identity) with `==` (value equality).

Interview tip: "`==` is value equality, `eql?` with `hash` controls hash key behavior, and `equal?` checks identity."

---

## 6) Type system & duck typing
What: dynamic, strong typing; duck typing means behavior matters, not class.
How it works:
- Types verified at runtime when you call methods.
- If an object responds to required methods, it works (`respond_to?`).
Why it matters: design flexible APIs; write tests to verify expected interfaces.
Example:
```ruby
def print_name(obj)
  puts obj.name
end
```
Interview tip: "Ruby is duck-typed — prefer interface contracts (tests) over strict classes."

---

## 7) Control flow
What: conditional execution (`if`, `unless`, `case`, ternary).
How it works:
- `if`/`elsif`/`else` branches choose code paths.
- `case` uses `===`, enabling ranges and regex matches.
Why it matters: `case` is powerful for pattern-like checks; `unless` is clearer for negative checks.
Example:
```ruby
case x
when 1..5 then 'small'
when /foo/ then 'has foo'
end
```
Interview tip: "Use `case` for multi-way branches; `unless` for simple negative conditions."

---

## 8) Loops & iteration
What: use iterators (`each`, `map`) over manual `while` where possible.
How it works:
- Enumerables provide `each`, `map`, `select`, `reduce`.
- Blocks pass behavior into methods.
Why it matters: idiomatic, concise, and often more efficient (less error-prone).
Example:
```ruby
[1,2,3].map { |n| n * 2 } # => [2,4,6]
```
Interview tip: "Prefer `Enumerable` methods for collection work; use `lazy` for large/streaming data."

---

## 9) Methods (definition & conventions)
What: methods encapsulate behavior; last expression is returned.
How it works:
- Define with `def name(args)` and `end`.
- Default args, keyword args, splat (`*args`), double splat (`**kwargs`).
Why it matters: clear method signatures make interfaces easier to use and test.
Example:
```ruby
def greet(name = "Guest")
  "Hello #{name}"
end
```
Interview tip: "Explain default and keyword args and when to use them for clarity."

---

## 10) Blocks, Procs, Lambdas
What: ways to pass code: block (one-off), Proc (object), lambda (Proc with strict arity/return).
How it works:
- Block: syntactic, passed to a method; use `yield` or `&block` to call it.
- Proc: `Proc.new` or `proc`; flexible about arguments.
- Lambda: `->(x){}` or `lambda`; enforces arguments and `return` behaves differently.
Why it matters: choose the right form for callbacks, higher-order functions, and DSLs.
Example:
```ruby
def twice
  yield
  yield
end
twice { puts 'hi' }
```
Interview tip: "Lambdas check arity; procs are more lenient — this affects error behavior."

---

## 11) I/O and interpolation
What: `puts`, `print`, `p` for output; `gets.chomp` for input; `"#{..}"` for interpolation.
How it works:
- `puts` appends newline; `p` shows inspect form; interpolation calls `to_s`.
Why it matters: use `p` for debugging; interpolation is safer than concatenation.
Example:
```ruby
name = gets.chomp
puts "Hello #{name}"
```
Interview tip: "Use `p` to inspect values during debugging because it shows structure."

---

## 12) Comments & documentation
What: Use `#` for single-line comments; document public APIs with RDoc or YARD. Comments should explain intent, not repeat code.

How (detailed):
- Inline comments: Use sparingly to explain non-obvious reasons or trade-offs.
- Documentation comments: Use YARD tags (`@param`, `@return`) for library/public methods to generate docs.
- Doc-driven examples: Small examples in docs help readers understand usage quickly.

Why it matters (practical):
- Maintenance: Clear documentation speeds onboarding and reduces misunderstandings.
- Readability: Code should mostly explain itself; comments should add context and reasoning.

Example (YARD):
```ruby
# Calculate area
# @param [Integer] w
# @param [Integer] h
# @return [Integer]
def area(w,h)
  w * h
end
```

Common pitfalls:
- Writing comments that duplicate code; they drift out of sync and become harmful.
- Overdocumenting internal implementation details rather than public behavior.

Interview tip: "Prefer small, well-named methods and concise docs; use YARD for public APIs."

---

## 13) Truthiness
What: Only `false` and `nil` are falsy; every other value (including `0`, `''`, `[]`) is truthy.

How (detailed):
- Condition checks test for Ruby truthiness, not strict boolean `true`/`false`.

Why it matters (practical):
- Porting code from languages where `0` or empty strings are falsy leads to logical errors.

Example:
```ruby
if 0
  puts '0 is truthy'
end
```

Common pitfalls:
- Expecting `0` or empty string to be falsy (common in C-family language devs).

Interview tip: "Only `nil` and `false` are falsy in Ruby — everything else is truthy."

---

## 14) File loading
What: `require` loads a file/gem once; `require_relative` loads relative to the caller file; `load` executes a file every time.

How (detailed):
- `require`: searches `$LOAD_PATH` and tracks loaded files to avoid double-loading.
- `require_relative`: builds a path relative to the current file — handy in small projects and scripts.
- `load`: reads and executes the specified file anew each time; rarely used in production.

Why it matters (practical):
- Autoloading and load paths: In Rails, Zeitwerk autoloads constants and reduces manual requires; understanding load mechanics helps debug missing constant errors.

Example:
```ruby
require 'json'             # standard library
require_relative 'lib/util' # project file
```

Common pitfalls:
- Using `require_relative` inside gems instead of proper `require` with packaged load paths.

Interview tip: "Explain `$LOAD_PATH`, `require_relative`, and Rails autoloading (Zeitwerk) for practical context."

---

## 15) Error handling
What: Handle runtime errors with `begin/rescue/else/ensure`; raise exceptions for unexpected conditions.

How (detailed):
- `begin` / `rescue`: Wrap code that may fail and handle specific exception classes.
- `else`: Runs when no exception was raised (useful to separate success logic from rescue).
- `ensure`: Always runs (useful for cleanup like closing files or releasing resources).
- `raise`: Create exceptions; custom exceptions should inherit from `StandardError` to avoid catching system-exiting exceptions.

Why it matters (practical):
- Reliability: Handling expected errors gracefully improves UX; overly broad rescues hide bugs and complicate debugging.

Example:
```ruby
begin
  content = File.read('file.txt')
rescue Errno::ENOENT => e
  puts "missing file: #{e.message}"
else
  puts "read #{content.length} bytes"
ensure
  puts 'cleanup if necessary'
end
```

Common pitfalls:
- Rescuing `Exception` or `StandardError` too broadly and swallowing important errors.
- Using exceptions for normal control flow — prefer return values for expected cases.

Interview tip: "Rescue specific errors, use `ensure` for cleanup, and define custom errors inheriting from `StandardError`."

---

## 16) Short practical summary
## 16) Short practical summary and study plan
Key takeaways:
- Everything is an object; method calls are message sends.
- Use idiomatic constructs like `Enumerable`, blocks, and clear method signatures.
- Understand execution (AST → bytecode → VM) to find performance hotspots.

Study plan (step-by-step):
1. Open `irb` or `pry` and try the examples in this file.
2. Inspect AST with `Ripper.sexp` and bytecode with `RubyVM::InstructionSequence`.
3. Implement small classes that override `==`, `eql?`, and `hash` and use them as keys in hashes.
4. Practice `Enumerable` pipelines and `lazy` enumerators for large data.
5. Write small scripts demonstrating `begin/rescue/ensure`, and create a custom exception class.

Next steps: Tell me which topic you want to deep dive into first (VM internals, methods & args, blocks & lambdas, performance, or Rails-focused topics). I'll expand that section with hands-on examples and exercises.

End of `00001_languagefundamentals.md` — fully expanded with deep-dive explanations, examples, pitfalls, and interview tips.