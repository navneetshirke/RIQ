````md
# 00020_ruby_tooling_rbenv_rvm_bundler_ans.md

# Ruby Tooling (rbenv, RVM, Bundler)

## Answer

**Ruby tooling** refers to tools that simplify **Ruby environment management, gem dependency handling, and project setup**. Key tools like **rbenv, RVM, and Bundler** are widely used in Ruby and Rails development to ensure consistent, maintainable, and isolated environments across projects.

---

### 1. rbenv

**rbenv** is a lightweight Ruby version manager that allows you to **switch between Ruby versions** easily.

- **Installation**:

```bash
# macOS with Homebrew
brew install rbenv

# Ubuntu/Debian
sudo apt install rbenv
````

* **Usage**:

```bash
rbenv install 3.2.2     # install Ruby version
rbenv global 3.2.2      # set default version globally
rbenv local 3.2.2       # set Ruby version for a project
ruby -v                 # check active Ruby version
```

* **Advantages**:

  * Lightweight and simple.
  * Avoids interfering with system Ruby.
  * Easy switching between multiple Ruby versions per project.

---

### 2. RVM (Ruby Version Manager)

**RVM** is a full-featured Ruby version manager with **gemset management** for isolating gem dependencies.

* **Installation**:

```bash
\curl -sSL https://get.rvm.io | bash -s stable
source ~/.rvm/scripts/rvm
```

* **Usage**:

```bash
rvm install 3.2.2
rvm use 3.2.2           # switch Ruby version
rvm gemset create myapp  # isolate gems for a project
rvm gemset use myapp
```

* **Advantages**:

  * Ruby version + gemset management.
  * Provides isolation between projects.
  * Works with multiple Rubies and Rails apps on the same machine.

---

### 3. Bundler

**Bundler** is a gem that manages **project dependencies** to ensure all required gems are installed in the correct versions.

* **Installation**:

```bash
gem install bundler
```

* **Usage**:

1. Create a `Gemfile` in your project:

```ruby
source 'https://rubygems.org'

gem 'rails', '~> 7.1'
gem 'pg', '~> 1.4'
gem 'puma', '~> 6.0'
```

2. Install dependencies:

```bash
bundle install
```

3. Run commands in the bundle context:

```bash
bundle exec rails s   # ensures correct gem versions
```

* **Advantages**:

  * Ensures **consistent gem versions** across development, staging, and production.
  * Supports dependency resolution and version locking.
  * Bundles all required gems in `Gemfile.lock` for reproducible builds.

---

### 4. Comparison: rbenv vs RVM

| Feature               | rbenv                     | RVM                         |
| --------------------- | ------------------------- | --------------------------- |
| Ruby Version Control  | Yes                       | Yes                         |
| Gemset Management     | No                        | Yes                         |
| Installation Overhead | Lightweight               | Heavier                     |
| Scope                 | Simple project-level Ruby | Ruby + gemsets per project  |
| Popularity            | High in modern Rails apps | Historically popular, older |

* **Recommendation**:

  * Use **rbenv** for simplicity and modern Rails workflows.
  * Use **RVM** if gemsets isolation is crucial.

---

### 5. Best Practices

* Always use a **version manager** (rbenv or RVM) to avoid system Ruby conflicts.
* Lock gem versions using **Bundler** (`Gemfile.lock`) for reproducible builds.
* Use `bundle exec` when running project commands to ensure correct gem versions.
* Keep `Gemfile` organized by environment (`group :development`, `group :test`).

---

### 6. Rails Project Workflow Example

```bash
# Setup environment
rbenv install 3.2.2
rbenv local 3.2.2

# Initialize project
rails new myapp -d postgresql

# Install gems
bundle install

# Run server
bundle exec rails s
```

* This ensures **environment isolation, consistent dependencies, and reproducible results** across all developers and servers.

---

### 7. Summary

* **rbenv**: Lightweight Ruby version management.
* **RVM**: Ruby version + gemset management.
* **Bundler**: Dependency management and version locking.
* Together, these tools ensure **stable, reproducible, and isolated Ruby environments**, which is critical for Rails development and production deployment.
* Proper tooling reduces conflicts, version mismatches, and eases project maintenance.

---

**End of Ruby Tooling (rbenv, RVM, Bundler)**

