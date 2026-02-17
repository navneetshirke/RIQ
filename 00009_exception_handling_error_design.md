```md
# 00009_exception_handling_error_design_ans.md

# Exception Handling & Error Design

## Answer

Exception handling in Ruby is a robust mechanism to manage **runtime errors**, ensuring programs can respond gracefully rather than crashing. Effective error design improves code reliability, readability, and maintainability, especially in complex applications like Rails.

---

### 1. What is an Exception?

- An **exception** is an object representing an error or unexpected event during program execution.
- All exceptions inherit from `Exception` class.
- Common hierarchy:

```

Exception
├─ StandardError
│   ├─ NoMethodError
│   ├─ ArgumentError
│   ├─ RuntimeError
│   └─ NameError
└─ SystemExit
└─ Interrupt

````

- **StandardError** is the default for `rescue`.

---

### 2. Raising Exceptions

Use `raise` (or `fail`) to signal an error:

```ruby
def divide(a, b)
  raise ArgumentError, "b cannot be zero" if b == 0
  a / b
end

divide(5, 0)
# => ArgumentError: b cannot be zero
````

* `raise` can take:

  * **Exception class**: `raise ZeroDivisionError`
  * **Message string**: `raise "Error occurred"`
  * Both: `raise ArgumentError, "Invalid argument"`

---

### 3. Rescue Clause

Handle exceptions with `rescue`:

```ruby
begin
  result = 10 / 0
rescue ZeroDivisionError => e
  puts "Error: #{e.message}"
end
```

* `=> e` assigns the exception object to a variable.
* Multiple exceptions can be rescued:

```ruby
begin
  # risky code
rescue ZeroDivisionError, ArgumentError => e
  puts e.message
end
```

---

### 4. Ensure Clause

`ensure` executes code regardless of exceptions:

```ruby
begin
  file = File.open("data.txt")
  # process file
rescue => e
  puts "Error: #{e.message}"
ensure
  file.close if file
end
```

* Useful for **resource cleanup** like closing files or DB connections.

---

### 5. Else Clause

`else` runs when **no exceptions occur**:

```ruby
begin
  puts "Trying..."
rescue => e
  puts "Rescued: #{e.message}"
else
  puts "Success!"
end
```

* Helps separate **normal flow** from error handling.

---

### 6. Custom Exceptions

* Define your own exception classes by inheriting from `StandardError`:

```ruby
class InvalidAgeError < StandardError; end

def check_age(age)
  raise InvalidAgeError, "Age must be > 0" if age <= 0
end
```

* Enables **semantic clarity** and targeted rescue.

---

### 7. Exception Hierarchy & Best Practices

* **Rescue StandardError**, not Exception (avoids catching system-exiting signals)
* Use specific exceptions to avoid **masking unrelated errors**
* Avoid empty `rescue` blocks (prevents debugging)
* Use **custom exceptions** for domain-specific errors

```ruby
begin
  # risky code
rescue InvalidAgeError => e
  puts "Domain error: #{e.message}"
end
```

---

### 8. Exception Object Methods

* `message` → returns error message
* `backtrace` → returns array of caller locations
* Example:

```ruby
begin
  1/0
rescue ZeroDivisionError => e
  puts e.message      # "divided by 0"
  puts e.backtrace    # array of lines where exception occurred
end
```

---

### 9. Retry Mechanism

`retry` restarts the `begin` block:

```ruby
count = 0
begin
  count += 1
  raise "Try again" if count < 3
rescue
  retry
end
```

* Use cautiously to avoid infinite loops.

---

### 10. Exception Design in Rails

* Rails uses **exception hierarchy** extensively:

  * `ActiveRecord::RecordNotFound` → missing DB record
  * `ActionController::RoutingError` → invalid route
  * `ActiveRecord::RecordInvalid` → failed validation

* Best practices:

  * **Rescue in controllers** with `rescue_from`:

```ruby
class UsersController < ApplicationController
  rescue_from ActiveRecord::RecordNotFound, with: :not_found

  def not_found
    render json: { error: "User not found" }, status: :not_found
  end
end
```

* Allows **centralized and consistent error handling**.

---

### 11. Logging Exceptions

* Always **log exceptions** for debugging:

```ruby
begin
  # risky code
rescue => e
  Rails.logger.error(e.message)
  Rails.logger.error(e.backtrace.join("\n"))
end
```

* Helps diagnose issues in production environments.

---

### 12. Summary

* **Exceptions** provide a mechanism to handle errors without crashing.
* **Raise** signals errors; **rescue** handles them.
* **Ensure** guarantees cleanup; **else** separates normal flow.
* **Custom exceptions** improve semantic clarity.
* Use **specific exception classes** to avoid hiding bugs.
* Rails leverages exceptions extensively for **ActiveRecord**, **controllers**, and **application-wide error handling**.

Mastering **exception handling and error design** is critical for building **robust, maintainable, and fault-tolerant Ruby and Rails applications**.

---

**End of Exception Handling & Error Design**

