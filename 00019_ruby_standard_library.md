````md
# 00019_ruby_standard_library_ans.md

# Ruby Standard Library

## Answer

The **Ruby Standard Library (stdlib)** is a collection of **pre-installed modules and classes** that extend Ruby’s core functionality. It provides **ready-to-use tools** for file handling, networking, data processing, testing, and more, enabling developers to write powerful Ruby applications **without installing external gems**.

---

### 1. Core vs Standard Library

- **Core library**: Always loaded; fundamental classes like `String`, `Array`, `Hash`, `Numeric`.
- **Standard library**: Ships with Ruby but requires `require` to load specific modules.

```ruby
require 'json'
JSON.parse('{"name": "Alice"}')  # => {"name"=>"Alice"}
````

* Standard libraries save development time by providing **common functionality out-of-the-box**.

---

### 2. Categories of Standard Library Modules

1. **Data Handling**

   * `CSV` – read/write CSV files
   * `JSON` – parse and generate JSON
   * `YAML` – handle YAML data

2. **File and I/O**

   * `FileUtils` – file operations
   * `Tempfile` – temporary files
   * `Dir` – directory handling

3. **Networking**

   * `Net::HTTP` – HTTP requests
   * `Socket` – low-level networking
   * `OpenURI` – easy file/network access

4. **Threading & Concurrency**

   * `Thread`, `Queue`, `Mutex` – multithreading primitives
   * `Monitor` – synchronization utility

5. **Date & Time**

   * `Date`, `Time`, `DateTime` – manipulation and formatting

6. **Math & Utilities**

   * `Math` – math functions
   * `BigDecimal` – precise decimal arithmetic
   * `SecureRandom` – cryptographically secure random numbers

7. **Testing**

   * `Test::Unit` – built-in unit testing framework
   * `MiniTest` – lightweight testing framework

8. **Data Structures**

   * `Set` – unordered unique elements
   * `Struct` – lightweight data objects

---

### 3. Loading Libraries

* Use `require` for standard libraries:

```ruby
require 'csv'

CSV.foreach("file.csv") do |row|
  puts row.inspect
end
```

* Some libraries are auto-loaded in Ruby (like `File`, `Math`) and **do not require explicit `require`**.

---

### 4. Examples of Common Libraries

1. **JSON parsing:**

```ruby
require 'json'
data = JSON.parse('{"name": "Alice", "age": 25}')
puts data["name"]  # => "Alice"
```

2. **CSV file handling:**

```ruby
require 'csv'

CSV.foreach("data.csv", headers: true) do |row|
  puts row["name"]
end
```

3. **Secure Random:**

```ruby
require 'securerandom'
puts SecureRandom.hex(16)  # => random 32-character hex string
```

4. **Temporary file:**

```ruby
require 'tempfile'
file = Tempfile.new("example")
file.puts "Hello World"
file.close
```

5. **Date manipulation:**

```ruby
require 'date'
puts Date.today             # => 2026-02-17
puts Date.parse("2026-02-01") + 5  # => 2026-02-06
```

---

### 5. Benefits of Using Standard Library

* Reduces dependency on third-party gems.
* Provides **tested, efficient, and stable implementations**.
* Encourages consistent coding practices.
* Easy to use, well-documented, and maintained with Ruby releases.

---

### 6. Best Practices

* Use standard libraries **whenever possible** to reduce gem bloat.
* Always require libraries explicitly for **clarity and maintainability**.
* Avoid monkey-patching standard libraries unless absolutely necessary.
* Check Ruby version compatibility for certain libraries.

---

### 7. Summary

* The Ruby Standard Library provides **essential tools** for developers.
* Categories include **data handling, I/O, networking, threading, math, testing, and data structures**.
* Familiarity with stdlib allows writing **efficient, maintainable, and robust Ruby applications**.
* Leveraging standard libraries reduces reliance on external gems and promotes **clean code**.

---

**End of Ruby Standard Library**

```

This is ready to save as `00019_ruby_standard_library_ans.md`.  

Do you want me to continue with **topic 20: Ruby Tooling (rbenv, RVM, Bundler)** next?
```
