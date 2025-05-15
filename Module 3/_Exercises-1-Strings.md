# Fundamentals: Redis Keys & Strings

## Setting String Values with SET and Variants

### 🔹 1. Basic Usage of SET

```redis
SET user:1001:name "Alice"                    # Set a basic key with a string value
SET user:1002:email "bob@example.com"         # Another simple key-value pair
```

---

### 🔹 2. Overwriting Existing Keys

```redis
SET user:1001:name "Alice"                    # Initial value
SET user:1001:name "Alicia"                   # Overwrites the previous value
GET user:1001:name                            # Returns "Alicia"
```

---

### 🔹 3. Conditional Set: NX and XX

```redis
SET session:token "abc123" NX                 # Set only if the key does NOT exist
SET session:token "def456" NX                 # Will not overwrite — key already exists
GET session:token                             # Returns "abc123"

SET session:token "updated456" XX             # Set only if the key DOES exist
GET session:token                             # Returns "updated456"

SET new:key "value" XX                        # Will not set — key doesn't exist
GET new:key                                   # Returns (nil)
```

---

### 🔹 4. Setting Expiration: EX and PX

```redis
SET temp:key "temporary" EX 10                # Expires in 10 seconds
TTL temp:key                                  # Check remaining TTL in seconds

SET temp:quick "flash" PX 5000                # Expires in 5000 milliseconds (5s)
PTTL temp:quick                               # Check remaining TTL in milliseconds
```

---

### 🔹 5. Combined Options

#### 🟢 Example 1: Set a one-time verification token

```redis
SET verify:user:123 "abc123token" NX EX 300
```

Creates a verification code that expires in 5 minutes.  
Will not overwrite if a code is already set — good for email/OTP flows.

---

#### 🟢 Example 2: Temporary lock after too many failed logins

```redis
SET lock:user:123 "locked" NX EX 900
```

Creates a lock key for a user after too many failed logins.  
Lock expires in 15 minutes.  
`NX` prevents resetting the timer if it already exists.

---

#### 🟢 Example 3: First-visit flag for onboarding

```redis
SET onboarding:shown:user:123 "yes" NX EX 86400
```

Flags that the onboarding was shown to this user.  
Expires after 1 day.  
Will not set again if already present.

---

#### 🟢 Example 4: Rate limit marker (1 action per 10 seconds)

```redis
SET ratelimit:api:user:123 "1" NX EX 10
```

Tracks when a user makes an API call.  
Only sets if it doesn’t already exist — enforcing a 10-second wait.  
If the key exists, the action is denied.
