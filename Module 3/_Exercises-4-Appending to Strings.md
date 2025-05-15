# Fundamentals: Redis Keys & Strings

## Redis String Operations Tutorial

## 1. Using the APPEND Command

### Redis CLI Examples:

```sh
SET welcome "Hello"
APPEND welcome ", world!"
GET welcome
```

**Expected Output:**
```
"Hello, world!"
```

**Explanation:**
- `SET` creates the key with an initial string.
- `APPEND` adds to the existing value.
- `GET` retrieves the updated value.

```sh
APPEND user:bio "I love chips"
```

**Explanation:**
- If `user:bio` does not exist, Redis creates it and sets the value.
- `APPEND` can initialize a key if it’s missing.

### Python Example:

```python
import redis

r = redis.Redis()
r.set("greeting", "Hi")
r.append("greeting", ", there!")
print(r.get("greeting").decode())  # Output: Hi, there!
```

---

## 2. Measuring Strings with STRLEN

### Redis CLI Examples:

```sh
SET message "Redis is fast"
STRLEN message
```

**Expected Output:**
```
13
```

**Explanation:**
- Measures the length in bytes (not words or characters).
- Spaces and punctuation count.

```sh
STRLEN unknown:key
```

**Expected Output:**
```
0
```

**Explanation:**
- `STRLEN` on a non-existent key returns `0`, not an error.

### Python Example:

```python
r = redis.Redis()
r.set("title", "Engineer")
length = r.strlen("title")
print(length)  # Output: 8
```

---

## 3. Handling Non-existent Keys

### Redis CLI Examples:

```sh
APPEND newkey "Initial"
GET newkey
```

**Expected Output:**
```
"Initial"
```

**Explanation:**
- `APPEND` creates the key if it doesn't exist, just like `SET`.

```sh
STRLEN missing
```

**Expected Output:**
```
0
```

**Explanation:**
- Safe to call even if the key hasn't been set—returns `0`.

### Python Example:

```python
length = r.strlen("nonexistent")
print(length)  # Output: 0

r.append("nonexistent", "Starts now.")
print(r.get("nonexistent").decode())  # Output: Starts now.
```
