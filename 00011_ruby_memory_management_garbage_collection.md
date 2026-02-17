````md
# 00010_ruby_collections_enumerables_ans.md

# Ruby Collections & Enumerables

## Answer

Collections in Ruby are **objects that hold multiple values**, such as arrays, hashes, and ranges. Enumerables provide a **common set of traversal and searching methods** for these collections, making Ruby extremely expressive and concise. Understanding collections and enumerables is essential for writing clean, efficient, and idiomatic Ruby code.

---

### 1. Arrays

- Ordered, indexed collection of objects.
- Can hold mixed types.

```ruby
arr = [1, "hello", :symbol, 3.14]
arr[0]      # => 1
arr[-1]     # => 3.14
arr << 5    # append element
arr.push(6) # append element
arr.pop     # remove last element
arr.shift   # remove first element
````

* Common methods: `length`, `size`, `include?`, `reverse`, `sort`, `uniq`.

---

### 2. Hashes

* Key-value pairs; keys are unique.
* Keys can be symbols, strings, or objects.

```ruby
user = { name: "Alice", age: 25 }
user[:name]   # => "Alice"
user[:email] = "alice@example.com"
user.keys     # => [:name, :age, :email]
user.values   # => ["Alice", 25, "alice@example.com"]
```

* Iteration: `each`, `each_key`, `each_value`, `each_pair`.

---

### 3. Ranges

* Represents a sequence of values.
* Useful for iteration, comparisons, and conditions.

```ruby
(1..5).to_a      # => [1,2,3,4,5]
('a'..'d').to_a  # => ["a", "b", "c", "d"]
(1...5).to_a     # => [1,2,3,4]  # exclusive end
```

* Check inclusion: `(1..5).include?(3)` → `true`

---

### 4. Enumerable Module

* Provides **iteration and collection traversal methods**.
* Included in `Array`, `Hash`, `Range`, and other collection-like classes.
* Requires **`each` method** to be defined in the class.

Common enumerable methods:

1. **map / collect** – transform elements

```ruby
[1,2,3].map { |n| n * 2 }  # => [2,4,6]
```

2. **select / filter** – return elements matching a condition

```ruby
[1,2,3,4].select { |n| n.even? }  # => [2,4]
```

3. **reject** – opposite of select

```ruby
[1,2,3,4].reject { |n| n.even? }  # => [1,3]
```

4. **reduce / inject** – accumulate a value

```ruby
[1,2,3,4].reduce(0) { |sum, n| sum + n }  # => 10
```

5. **each_with_index** – iterate with index

```ruby
["a", "b"].each_with_index { |val, i| puts "#{i}: #{val}" }
```

6. **any? / all? / none? / one?** – predicate checks

```ruby
[1,2,3].any? { |n| n > 2 }  # => true
[1,2,3].all? { |n| n > 0 }  # => true
```

7. **find / detect** – returns first matching element

```ruby
[1,2,3,4].find { |n| n > 2 }  # => 3
```

8. **group_by** – group elements by a block

```ruby
[1,2,3,4,5].group_by { |n| n.even? }
# => {false=>[1,3,5], true=>[2,4]}
```

---

### 5. Hash Enumerables

* Hashes include Enumerable methods:

```ruby
user = {a: 1, b: 2, c: 3}
user.map { |k,v| v*2 }        # => [2,4,6]
user.select { |k,v| v.odd? }  # => {:a=>1, :c=>3}
user.keys.each { |k| puts k } # prints keys
```

* `each_pair`, `each_key`, `each_value` are also useful.

---

### 6. Array and Hash Conversion

* Convert array to hash:

```ruby
arr = [[:a,1], [:b,2]]
hash = arr.to_h  # => {:a=>1, :b=>2}
```

* Convert hash to array:

```ruby
user.to_a  # => [[:a,1], [:b,2], [:c,3]]
```

---

### 7. Chaining Enumerables

Ruby allows **method chaining** for powerful, readable code:

```ruby
[1,2,3,4,5].select { |n| n.even? }.map { |n| n * 10 }  # => [20,40]
```

* Chain as many enumerable methods as needed.
* Avoid complex nested loops with chaining.

---

### 8. Lazy Enumerables

* For **large collections**, use `lazy` to avoid eager evaluation:

```ruby
(1..Float::INFINITY).lazy.select { |n| n.even? }.first(5)
# => [2,4,6,8,10]
```

* Prevents memory issues by generating elements on demand.

---

### 9. Useful Methods Summary

| Method                       | Purpose                     |
| ---------------------------- | --------------------------- |
| `each`                       | iterate over collection     |
| `map / collect`              | transform elements          |
| `select`                     | filter elements             |
| `reject`                     | reject elements             |
| `reduce / inject`            | aggregate values            |
| `find / detect`              | find first matching element |
| `any? / all? / none? / one?` | check conditions            |
| `group_by`                   | group elements by block     |
| `sort / sort_by`             | sort collections            |
| `uniq`                       | remove duplicates           |

---

### 10. Summary

* **Arrays**: ordered lists of objects.
* **Hashes**: key-value mappings.
* **Ranges**: sequences of values.
* **Enumerable module**: provides powerful traversal, transformation, and aggregation methods.
* Chaining enumerables improves **readability** and **expressiveness**.
* Lazy enumerables are useful for **large or infinite collections**.
* Proper use of collections and enumerables is crucial for **efficient and idiomatic Ruby**, especially in Rails data processing and ActiveRecord queries.

---

**End of Ruby Collections & Enumerables**


