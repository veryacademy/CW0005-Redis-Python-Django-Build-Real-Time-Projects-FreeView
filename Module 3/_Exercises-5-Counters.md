# Fundamentals: Redis Keys & Strings

## Redis Counter Operations Tutorial

## 1. Using INCR and DECR

### Redis CLI:

```sh
# Key doesn't exist yet
INCR page:views
```

**Output:**
```
(integer) 1
```

```sh
INCR page:views
```

**Output:**
```
(integer) 2
```

```sh
DECR page:views
```

**Output:**
```
(integer) 1
```

**Explanation:**  
- If the key doesn’t exist, Redis creates it and starts at `0`.  
- `INCR` increases by 1, `DECR` decreases by 1.

---

### Python:

```python
import redis

r = redis.Redis()

# INCR on new key
print(r.incr("page:views"))  # Output: 1

# INCR again
print(r.incr("page:views"))  # Output: 2

# DECR
print(r.decr("page:views"))  # Output: 1
```

---

## 2. Using INCRBY and DECRBY

### Redis CLI:

```sh
INCRBY likes 5
```

**Output:**
```
(integer) 5
```

```sh
DECRBY likes 2
```

**Output:**
```
(integer) 3
```

**Explanation:**  
- These commands adjust the counter by a specified amount instead of just 1.

---

### Python:

```python
# INCRBY
print(r.incrby("likes", 5))  # Output: 5

# DECRBY
print(r.decrby("likes", 2))  # Output: 3
```

---

## 3. Handling Invalid or Non-integer Values

### Redis CLI:

```sh
SET user:name "Alice"
INCR user:name
```

**Output:**
```
(error) ERR value is not an integer or out of range
```

**Explanation:**  
- Redis only allows `INCR`/`DECR` on keys that contain valid integer strings.  
- If the value is text or not a number, Redis throws an error.

---

### Python:

```python
r.set("user:name", "Alice")

try:
    r.incr("user:name")
except redis.exceptions.ResponseError as e:
    print("Error:", e)
```

**Output:**
```
Error: ERR value is not an integer or out of range
```

### Safe INCR Pattern Using Lua Script (Optional Advanced Way)
```
EVAL "if (redis.call('TYPE', KEYS[1]).ok == 'string') 
     and (tonumber(redis.call('GET', KEYS[1])) ~= nil) 
     then 
       return redis.call('INCR', KEYS[1]) 
     else 
       return 'ERROR: Not an integer string' 
     end" 1 page:views
```