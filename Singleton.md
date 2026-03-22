# 🧠 What is Singleton?

A **Singleton ensures only one instance of a class exists** across your application.

---

# ⚠️ Important (Python Insight)

In Python, Singletons are often implemented using:

* Module-level objects
* Class-based control (`__new__`)
* Decorators

---

# 🚀 1. Config Manager (MOST COMMON)

👉 Used for: environment configs, app settings

```python
class Config:
    _instance = None

    def __new__(cls):
        if cls._instance is None:
            print("Creating config instance")
            cls._instance = super().__new__(cls)
            cls._instance.settings = {"db": "postgres", "debug": True}
        return cls._instance


# Usage
c1 = Config()
c2 = Config()

print(c1 is c2)  # True
```

✅ Why Singleton?

* You don’t want multiple conflicting configs

---

# 🗄️ 2. Database Connection Manager

👉 Used in: FastAPI, Django, microservices

```python
class Database:
    _instance = None

    def __new__(cls):
        if cls._instance is None:
            print("Connecting to DB...")
            cls._instance = super().__new__(cls)
            cls._instance.connection = "DB_CONNECTION"
        return cls._instance


db1 = Database()
db2 = Database()

print(db1 is db2)  # True
```

✅ Why?

* Avoid multiple DB connections
* Saves memory + improves performance

---

# 📊 3. Logger Singleton

👉 Used for centralized logging

```python
class Logger:
    _instance = None

    def __new__(cls):
        if not cls._instance:
            cls._instance = super().__new__(cls)
        return cls._instance

    def log(self, message):
        print(f"[LOG]: {message}")


logger1 = Logger()
logger2 = Logger()

logger1.log("Hello")
print(logger1 is logger2)  # True
```

✅ Why?

* Centralized logging system

---

# ⚙️ 4. Cache Manager (Redis-like)

👉 Used for caching responses / ML results

```python
class Cache:
    _instance = None

    def __new__(cls):
        if not cls._instance:
            cls._instance = super().__new__(cls)
            cls._instance.store = {}
        return cls._instance

    def set(self, key, value):
        self.store[key] = value

    def get(self, key):
        return self.store.get(key)


cache1 = Cache()
cache2 = Cache()

cache1.set("user", "Rohan")
print(cache2.get("user"))  # Rohan
```

✅ Why?

* Shared memory across app

---

# 🌐 5. API Rate Limiter (Real Backend Use Case)

👉 Used in: API gateways, FastAPI middleware

```python
class RateLimiter:
    _instance = None

    def __new__(cls):
        if not cls._instance:
            cls._instance = super().__new__(cls)
            cls._instance.requests = {}
        return cls._instance

    def allow_request(self, user):
        count = self.requests.get(user, 0)
        if count >= 5:
            return False
        self.requests[user] = count + 1
        return True


rl1 = RateLimiter()
rl2 = RateLimiter()

print(rl1.allow_request("user1"))  # True
print(rl2.allow_request("user1"))  # Shared state
```

✅ Why?

* Tracks global request state

---

# 🔥 Bonus: Cleaner Pythonic Singleton (Decorator)

```python
def singleton(cls):
    instances = {}

    def wrapper(*args, **kwargs):
        if cls not in instances:
            instances[cls] = cls(*args, **kwargs)
        return instances[cls]

    return wrapper


@singleton
class Service:
    pass
```

---

# ⚠️ When NOT to Use Singleton

Avoid if:

* You need **multiple instances (testing, scaling)**
* It creates **hidden global state**
* Makes code harder to debug

---
Perfect—this is the **right level of depth**. I’ll give you **multiple examples for each Singleton approach**, all tied to **real backend use cases** so you can actually use them.

---

# 🧱 1. Using `__new__` (Classic + Interview Favorite)

👉 Controls object creation at class level

---

## ✅ Example 1: DB Connection

```python
class Database:
    _instance = None

    def __new__(cls):
        if cls._instance is None:
            print("Initializing DB...")
            cls._instance = super().__new__(cls)
            cls._instance.conn = "DB_CONNECTION"
        return cls._instance


db1 = Database()
db2 = Database()
print(db1 is db2)  # True
```

---

## ✅ Example 2: Config Loader

```python
class Config:
    _instance = None

    def __new__(cls):
        if cls._instance is None:
            cls._instance = super().__new__(cls)
            cls._instance.data = {"env": "prod"}
        return cls._instance


c1 = Config()
c2 = Config()
print(c1.data)
```

---

## ✅ Example 3: Thread-safe Singleton (Advanced 🔥)

```python
import threading

class Singleton:
    _instance = None
    _lock = threading.Lock()

    def __new__(cls):
        with cls._lock:
            if cls._instance is None:
                cls._instance = super().__new__(cls)
        return cls._instance
```

👉 Use in:

* Multi-threaded FastAPI apps

---

# 🎯 When to use `__new__`

* You want **strict control**
* Asked in interviews
* Low-level system design

---

# 🎨 2. Using Decorator (Clean + Reusable)

👉 Best when you want **reuse across many classes**

---

## ✅ Example 1: Generic Singleton Decorator

```python
def singleton(cls):
    instances = {}

    def wrapper(*args, **kwargs):
        if cls not in instances:
            instances[cls] = cls(*args, **kwargs)
        return instances[cls]

    return wrapper
```

---

## ✅ Example 2: Logger using decorator

```python
@singleton
class Logger:
    def log(self, msg):
        print(f"[LOG]: {msg}")


l1 = Logger()
l2 = Logger()

print(l1 is l2)  # True
```

---

## ✅ Example 3: Cache Service

```python
@singleton
class Cache:
    def __init__(self):
        self.store = {}

    def set(self, k, v):
        self.store[k] = v

    def get(self, k):
        return self.store.get(k)


c1 = Cache()
c2 = Cache()

c1.set("x", 10)
print(c2.get("x"))  # 10
```

---

# 🎯 When to use Decorator

* You want **clean + reusable code**
* Multiple singleton classes
* Production-friendly

---

# 🐍 3. Module-Level Singleton (MOST PYTHONIC ⭐⭐⭐)

👉 Python modules are **already singletons by design**

---

## ✅ Example 1: Config Module

### 📁 config.py

```python
settings = {
    "db": "postgres",
    "debug": True
}
```

### 📁 main.py

```python
import config

print(config.settings)
```

👉 Same instance everywhere automatically

---

## ✅ Example 2: DB Connection Module

### 📁 db.py

```python
class DB:
    def __init__(self):
        print("Connecting to DB...")
        self.conn = "DB_CONNECTION"

db = DB()  # Singleton instance
```

### 📁 service.py

```python
from db import db

print(db.conn)
```

---

## ✅ Example 3: Logger Module

```python
# logger.py
class Logger:
    def log(self, msg):
        print(msg)

logger = Logger()
```

```python
# app.py
from logger import logger

logger.log("Hello")
```

---

# 🎯 Why Module-level is BEST in Python

✅ No extra logic
✅ Built-in Singleton behavior
✅ Clean and readable
✅ Used in real production systems

---

# ⚖️ Comparison (Important for Interviews)

| Approach     | Complexity | Use Case          | Pythonic |
| ------------ | ---------- | ----------------- | -------- |
| `__new__`    | Medium     | Low-level control | ❌        |
| Decorator    | Medium     | Reusable pattern  | ✅        |
| Module-level | Simple     | Real-world apps   | ⭐⭐⭐      |

---




