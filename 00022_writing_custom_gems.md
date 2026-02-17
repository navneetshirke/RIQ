````md
# 00022_writing_custom_gems_ans.md

# Writing Custom Gems

## Answer

Writing custom gems in Ruby allows developers to **package reusable code**, distribute it easily, and share functionality across multiple projects. Understanding how to structure, build, and publish a gem is essential for **professional Ruby development**.

---

### 1. What is a Custom Gem?

- A **custom gem** is a **Ruby library** you create.
- It contains:
  - Ruby code (`lib/` directory)
  - Metadata (`.gemspec` file)
  - Documentation (optional)
- Benefits:
  - Reusable code
  - Easy versioning
  - Encapsulated functionality
  - Shareable via RubyGems.org

---

### 2. Prerequisites

- Ruby installed
- Bundler installed:

```bash
gem install bundler
````

* Familiarity with `Gemfile` and standard Ruby project structure.

---

### 3. Generating a Gem Skeleton

* Use Bundler’s gem generator:

```bash
bundle gem my_gem
```

* This creates:

```
my_gem/
├── lib/
│   └── my_gem.rb
├── my_gem.gemspec
├── Rakefile
├── Gemfile
└── README.md
```

* Options:

  * `--test=rspec` for RSpec setup
  * `--coc` to include Contributor Covenant

---

### 4. Implementing Functionality

* Write code inside `lib/my_gem.rb` or nested modules.

```ruby
# lib/my_gem.rb
require "my_gem/version"

module MyGem
  class Error < StandardError; end

  def self.greet(name)
    "Hello, #{name}!"
  end
end
```

* `lib/my_gem/version.rb` defines gem version:

```ruby
module MyGem
  VERSION = "0.1.0"
end
```

---

### 5. Writing Tests

* Test files usually go under `spec/` (RSpec) or `test/` (Minitest):

```ruby
# spec/my_gem_spec.rb
require "spec_helper"

RSpec.describe MyGem do
  it "greets correctly" do
    expect(MyGem.greet("Alice")).to eq("Hello, Alice!")
  end
end
```

* Run tests:

```bash
bundle exec rspec
```

---

### 6. Building the Gem

* Build your gem locally:

```bash
gem build my_gem.gemspec
```

* Output: `my_gem-0.1.0.gem`

* Install locally for testing:

```bash
gem install ./my_gem-0.1.0.gem
```

---

### 7. Using the Gem in a Project

1. Add it to your `Gemfile`:

```ruby
gem "my_gem", path: "/path/to/my_gem"
```

2. Install with Bundler:

```bash
bundle install
```

3. Use the gem:

```ruby
require "my_gem"
puts MyGem.greet("Bob")  # => "Hello, Bob!"
```

---

### 8. Publishing to RubyGems.org

1. Create an account on [RubyGems.org](https://rubygems.org).
2. Build the gem:

```bash
gem build my_gem.gemspec
```

3. Push the gem:

```bash
gem push my_gem-0.1.0.gem
```

* Once pushed, anyone can install it with:

```bash
gem install my_gem
```

---

### 9. Versioning (Semantic Versioning)

* Follow **SemVer**:

```
MAJOR.MINOR.PATCH
```

* Examples:

  * `0.1.0` – initial release
  * `0.1.1` – bug fixes
  * `0.2.0` – added new features
  * `1.0.0` – major release, possible breaking changes

* Update `lib/my_gem/version.rb` before each release.

---

### 10. Best Practices

* Include `README.md` for documentation.
* Use a versioning system (SemVer) for clarity.
* Write tests to ensure reliability.
* Avoid polluting global namespace; wrap in modules.
* Keep gem functionality focused and modular.
* Include `.gemspec` metadata: author, email, summary, license.

---

### 11. Summary

* Custom gems allow **packaging reusable Ruby code**.
* Use **Bundler** to generate skeleton and manage dependencies.
* Implement features in `lib/`, define version, and write tests.
* Build, install locally, and optionally **publish to RubyGems.org**.
* Follow **Semantic Versioning, testing, and modular design** for maintainability.
* Mastering custom gems enhances **code reuse, collaboration, and professional Ruby development**.

---

**End of Writing Custom Gems**

```

This is ready to save as `00022_writing_custom_gems_ans.md`.  

Do you want me to continue with **topic 23: Semantic Versioning & Dependency Resolution** next?
```
