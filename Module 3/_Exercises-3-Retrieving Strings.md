# Fundamentals: Redis Keys & Strings

## Tutorial: Retrieving Strings – `GET`, `MGET`, and Key Lookups

---

### 🔹 1. Retrieving a Single String with `GET`

**Objective:** Understand how to use the `GET` command to fetch a single value from Redis.

```bash
SET user:1001:name "Alice"
GET user:1001:name
```

**Output:**
```
"Alice"
```

Try retrieving a key that doesn’t exist:

```bash
GET user:9999:name
```

**Output:**
```
(nil)
```

---

### 🔹 2. Retrieving Multiple Strings with `MGET`

**Objective:** Learn how to fetch multiple values in one command.

```bash
SET user:1002:name "Bob"
SET user:1003:name "Charlie"

MGET user:1001:name user:1002:name user:1003:name
```

**Output:**
```
1) "Alice"
2) "Bob"
3) "Charlie"
```

Missing key example:

```bash
MGET user:1001:name user:9999:name
```

**Output:**
```
1) "Alice"
2) (nil)
```

---

### 🔹 3. Checking Key Existence Before Fetching

**Objective:** Use `EXISTS` to see if a key is present before trying to retrieve it.

```bash
EXISTS user:1001:name
```

**Output:**
```
(integer) 1
```

```bash
EXISTS user:9999:name
```

**Output:**
```
(integer) 0
```

---

### 🔹 4. Handling Missing Keys Gracefully

**Objective:** Write logic that deals with missing values without failing.

```python
import redis

r = redis.Redis()

value = r.get("user:9999:name")
if value:
    print(value.decode())
else:
    print("Key not found")
```

---

### 🔹 5. Using Basic Key Patterns for Lookups

**Objective:** Understand and apply key-naming conventions for easier lookups.

```bash
MGET user:1001:name user:1001:email
```

Example key naming:

- `user:1001:name`
- `user:1001:email`
- `session:abc123:status`
- `cache:item:4821`

---

### 🔹 6. Practical Example: Lookup Flow in Python

**Objective:** Tie everything together in a Python script that checks keys, handles missing data, and retrieves values.

```python
import redis

r = redis.Redis()

user_id = 1001
keys = [f"user:{user_id}:name", f"user:{user_id}:email"]

# Check which keys exist
for key in keys:
    if r.exists(key):
        value = r.get(key)
        print(f"{key} → {value.decode()}")
    else:
        print(f"{key} → Not found")
```
