# Rails Request Lifecycle --- Deep Memorization & Internals Guide

This document expands the Rails lifecycle into **interview‑level and
senior‑level understanding**.

------------------------------------------------------------------------

# 1. Request Starts (Browser)

-   User enters URL or clicks link
-   Browser builds HTTP request
-   DNS resolves domain → IP
-   TCP connection established
-   HTTP request sent

Example:

    GET /posts HTTP/1.1
    Host: localhost:3000
    Cookie: _session_id=abc123

Key ideas: - HTTP is stateless - Every request is independent - Cookies
restore user context

------------------------------------------------------------------------

# 2. Web Server (Puma)

Puma responsibilities:

-   Accept TCP connections
-   Parse HTTP protocol
-   Manage thread pool
-   Forward request to Rack

Important: - Puma is **multi‑threaded** - One process can handle many
requests concurrently

Concept:

    Client → Puma Thread → Rails App

Configurable via:

    config/puma.rb

------------------------------------------------------------------------

# 3. Rack --- The Foundation of Rails

Rails is a Rack application.

Rack standardizes communication between:

    Web Server ⇄ Ruby Framework

Rack request format:

``` ruby
env = {
  "REQUEST_METHOD" => "GET",
  "PATH_INFO" => "/posts",
  "QUERY_STRING" => "",
  "rack.input" => IO object
}
```

Rack response format:

``` ruby
[status, headers, body]
```

Example:

``` ruby
[200, {"Content-Type"=>"text/html"}, ["<html>...</html>"]]
```

Key Insight: \> Every Rails request is just a Ruby method returning a
Rack array.

------------------------------------------------------------------------

# 4. Middleware Stack (Mini Pipeline)

Middleware acts like layered filters.

Execution order:

    Request ↓
    Middleware A
    Middleware B
    Middleware C
    Response ↑

Common middleware:

-   ActionDispatch::Static
-   ActionDispatch::Cookies
-   ActionDispatch::Session
-   Rack::MethodOverride
-   Rails::Rack::Logger

Example behavior: - Static middleware returns assets instantly - Session
middleware loads user session - Logger records request timing

Command to inspect:

    bin/rails middleware

------------------------------------------------------------------------

# 5. Routing Engine

Router uses optimized lookup tables built during boot.

Routes compiled into internal structures for fast matching.

Example:

``` ruby
resources :posts
```

Generates:

    GET    /posts        → index
    POST   /posts        → create
    GET    /posts/:id    → show

Router extracts parameters automatically.

------------------------------------------------------------------------

# 6. Controller Lifecycle (Deep)

When route matches:

Rails performs:

1.  Instantiate controller
2.  Build request & response objects
3.  Run callbacks
4.  Execute action
5.  Render response

Callback chain:

    before_action
    around_action
    action
    after_action

Controller internally inherits from:

    ActionController::Metal
    → ActionController::Base

Responsibilities: - Authorization - Input validation - Orchestration
logic

------------------------------------------------------------------------

# 7. Params Processing

Rails merges params from:

-   Route params
-   Query params
-   POST body
-   JSON payload

Internally uses:

    ActionDispatch::Request

Strong Parameters protect mass assignment:

``` ruby
params.require(:post).permit(:title)
```

Security Purpose: - Prevent unwanted DB writes

------------------------------------------------------------------------

# 8. Active Record Internals

Active Record responsibilities:

-   ORM mapping
-   Query building
-   Persistence
-   Validations
-   Associations

Query Flow:

    Ruby Method
    → Arel AST
    → SQL Generation
    → DB Adapter
    → Database Execution
    → Result Parsing
    → Ruby Objects

Example:

``` ruby
Post.where(published: true)
```

Lazy evaluation: - Query runs only when needed.

------------------------------------------------------------------------

## N+1 Query Problem

Bad:

    posts.each { |p| p.comments }

Fix:

    Post.includes(:comments)

Why: - Prevent repeated DB queries.

------------------------------------------------------------------------

# 9. View Rendering Engine

Rails uses ActionView.

Rendering steps:

1.  Find template
2.  Compile ERB → Ruby code
3.  Execute Ruby
4.  Produce HTML string

ERB becomes Ruby method internally.

Templates cached in production.

------------------------------------------------------------------------

# 10. Layout + Rendering Pipeline

Rendering order:

    Template
    → Partial rendering
    → Layout wrapping
    → HTML assembly

Partials:

    _rendered multiple times efficiently_

Example:

    render partial: "post"

------------------------------------------------------------------------

# 11. Response Construction

Rails builds response object:

-   Status code
-   Headers
-   Body

Example headers:

    Content-Type
    Cache-Control
    ETag
    Set-Cookie

ETags enable browser caching.

------------------------------------------------------------------------

# 12. Response Sent Back

Reverse pipeline:

    Rails → Rack → Puma → TCP → Browser

Browser parses HTML → builds DOM → renders UI.

------------------------------------------------------------------------

# FULL REQUEST FLOW (CORE MEMORY)

    Browser
    → Puma
    → Rack
    → Middleware
    → Router
    → Controller
    → Model
    → Database
    → View
    → Layout
    → Response
    → Browser

------------------------------------------------------------------------

# Rails Boot Process (Deep)

When running:

    bin/rails server

Rails:

1.  Loads Bundler & gems
2.  Loads application config
3.  Initializes frameworks
4.  Loads routes
5.  Runs initializers
6.  Builds middleware stack
7.  Starts Puma threads

Boot happens once --- requests reuse loaded code.

------------------------------------------------------------------------

# Threading & Concurrency Model

Rails request handling:

    Thread receives request
    Controller executed
    Response returned
    Thread reused

Important: - Avoid global mutable state - Use thread-safe code

------------------------------------------------------------------------

# Memory Lifecycle of a Request

Per request:

-   Controller object created
-   View objects created
-   Query objects created

After response: - Objects eligible for Ruby GC

Meaning: \> Requests are isolated execution contexts.

------------------------------------------------------------------------

# Caching Layers in Rails

1.  Browser caching
2.  HTTP caching (ETag)
3.  Fragment caching
4.  Low-level caching
5.  Database query cache

Example:

``` ruby
cache @post do
  render @post
end
```

------------------------------------------------------------------------

# Performance Bottlenecks

Most common slowdowns:

-   N+1 queries
-   Blocking DB calls
-   Large view rendering
-   Missing indexes
-   Excess allocations

------------------------------------------------------------------------

# Debugging Request Flow

Useful tools:

    rails server logs
    rack-mini-profiler
    bullet gem
    rails middleware

Logs show full lifecycle timing.

------------------------------------------------------------------------

# Scaling Rails Applications

Single server limits:

-   CPU
-   Memory
-   DB connections

Scaling approaches:

-   Horizontal scaling
-   Background jobs
-   Caching
-   CDN
-   Redis pub/sub
-   Read replicas

------------------------------------------------------------------------

# Senior-Level Mental Model

Rails is essentially:

    HTTP → Rack → Ruby Objects → Database → HTML → HTTP

Everything else is automation.

------------------------------------------------------------------------

# 15‑Second Interview Answer (Advanced)

> Rails receives an HTTP request through Puma, converts it into a Rack
> environment, processes it through middleware, routes it to a
> controller action, executes business logic using Active Record,
> renders views via ActionView, constructs a Rack response, and sends
> the result back through the web server to the browser.