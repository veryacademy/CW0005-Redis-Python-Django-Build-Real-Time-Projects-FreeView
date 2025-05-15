# Fundamentals: Redis Keys & Strings

## 🧪 Exercises: Managing Expiration with `EXPIRE`, `TTL`, and Lifetimes

---

## 🔹 1. Setting Expiration with `EXPIRE`

```redis
SET session:user:1001 "active"         # Set a session key
EXPIRE session:user:1001 60            # Set to expire after 60 seconds
TTL session:user:1001                  # Check remaining time
```

📝 *Creates a temporary session that lasts one minute.*

---

## 🔹 2. Checking TTL on Existing Keys

```redis
SET temp:data "123"
EXPIRE temp:data 30
TTL temp:data                          # Should return a value <= 30

SET permanent:data "456"
TTL permanent:data                     # Should return -1 (no expiry)
```

📝 *Use TTL to monitor how long keys will stay in Redis.*

---

## 🔹 3. Removing Expiration with `PERSIST`

```redis
SET alert:email "pending"
EXPIRE alert:email 120
PERSIST alert:email                    # Removes the expiration
TTL alert:email                        # Should return -1
```

📝 *Use `PERSIST` when you want to make a temporary key permanent again.*

---

## 🔹 4. Setting and Expiring with `SETEX` (Shortcut)

```redis
SETEX cache:item:9001 15 "cached-value"    # Key expires in 15 seconds
GET cache:item:9001                        # Returns "cached-value"
TTL cache:item:9001                        # Should return <= 15
```

📝 *`SETEX` combines setting a value and expiration in one step.*

---

## 🔹 5. Using `EXPIRE` After Update

```redis
SET login:attempts:user:42 "1"
EXPIRE login:attempts:user:42 300          # Expires after 5 minutes

# Simulate another attempt
INCR login:attempts:user:42
TTL login:attempts:user:42                 # Check that expiry is still active
```

📝 *Updating a key value doesn't reset its expiration unless re-applied.*

---

## 🔹 6. Expiry with Milliseconds: `PEXPIRE` and `PTTL`

```redis
SET flash:notice "hello"
PEXPIRE flash:notice 5000                 # Expires in 5000 ms (5 sec)
PTTL flash:notice                         # Check TTL in milliseconds
```

📝 *Use millisecond precision when finer control is needed.*

---

### 🔹 7. Setting Expiration: EX and PX

```redis
SET temp:key "temporary" EX 10                # Expires in 10 seconds
TTL temp:key                                  # Check remaining TTL in seconds

SET temp:quick "flash" PX 5000                # Expires in 5000 milliseconds (5s)
PTTL temp:quick                               # Check remaining TTL in milliseconds
```