# 🛡️ Code Security & Quality Report

**File:** `sample_buggy_code.py`  
**Generated:** 2026-04-11 03:52:05  
**Overall Risk:** 🔴 `CRITICAL`

---

## 📋 Executive Summary

Analysis of `sample_buggy_code.py` identified **14 code quality issue(s)** and **10 security vulnerability(ies)**, with **13 automated fix(es)** suggested. The overall security risk level is assessed as **CRITICAL**. OWASP categories affected: A01:2021, A02:2021, A03:2021, A05:2021, A08:2021.


## 📊 Code Metrics

| Metric | Value |
|--------|-------|
| Loc | 101 |
| Blank Lines | 19 |
| Function Count | 12 |
| Class Count | 0 |
| Total Issues | 14 |


## 🐛 Code Quality Issues

### 🟠 [MutableDefaultArgument] — Line 24

**Severity:** `HIGH`  
**Description:** Function 'add_user_to_list' uses mutable default argument — shared across all calls, leading to unexpected state.

```python
def add_user_to_list(user, user_list=[]):
```

### 🟡 [AssertInProduction] — Line 70

**Severity:** `MEDIUM`  
**Description:** Using 'assert' for runtime checks — asserts are stripped when Python runs with -O flag.

```python
    assert amount > 0, "Amount must be positive"  # Stripped by -O flag!
```

### 🟡 [BareExcept] — Line 33

**Severity:** `MEDIUM`  
**Description:** Bare 'except:' catches all exceptions including SystemExit and KeyboardInterrupt — use 'except Exception'.

```python
    except:
```

### 🔴 [ZeroDivision] — Line 40

**Severity:** `CRITICAL`  
**Description:** Explicit division by zero literal detected.

```python
    return total / 0
```

### 🔴 [HardcodedCredential] — Line 17

**Severity:** `CRITICAL`  
**Description:** Possible hardcoded credential detected — use environment variables.

```python
password = "supersecret123"
```

### 🔴 [HardcodedCredential] — Line 18

**Severity:** `CRITICAL`  
**Description:** Possible hardcoded credential detected — use environment variables.

```python
api_key = "sk-prod-abc123456789"
```

### 🟢 [MagicNumber] — Line 62

**Severity:** `LOW`  
**Description:** Magic number '100000' — consider using a named constant.

```python
return str(random.randint(100000, 999999))
```

### 🟢 [TodoComment] — Line 71

**Severity:** `LOW`  
**Description:** Unresolved marker found: return amount * 1.15  # TODO: apply discount logic here

```python
return amount * 1.15  # TODO: apply discount logic here
```

### 🟢 [MagicNumber] — Line 71

**Severity:** `LOW`  
**Description:** Magic number '15' — consider using a named constant.

```python
return amount * 1.15  # TODO: apply discount logic here
```

### 🟢 [MagicNumber] — Line 81

**Severity:** `LOW`  
**Description:** Magic number '5000' — consider using a named constant.

```python
if price > 5000:          # Magic: what is 5000?
```

### 🟢 [MagicNumber] — Line 82

**Severity:** `LOW`  
**Description:** Magic number '85' — consider using a named constant.

```python
return price * 0.85   # Magic: what is 0.85?
```

### 🟢 [MagicNumber] — Line 83

**Severity:** `LOW`  
**Description:** Magic number '1500' — consider using a named constant.

```python
elif price > 1500:        # Magic: what is 1500?
```

### 🟢 [MagicNumber] — Line 84

**Severity:** `LOW`  
**Description:** Magic number '90' — consider using a named constant.

```python
return price * 0.90   # Magic: what is 0.90?
```

### 🟢 [MagicNumber] — Line 101

**Severity:** `LOW`  
**Description:** Magic number '6000' — consider using a named constant.

```python
print(apply_discount(6000))
```


## 🔐 Security Vulnerabilities

### 🔴 [HardcodedSecret] — Line 17

**OWASP:** `A02:2021`  **Severity:** `CRITICAL`  

**Description:** Hardcoded secret/credential detected — store in environment variables or a vault.  

**Remediation:** Move secrets to .env files and load via os.environ.get().

```python
password = "supersecret123"
```

### 🔴 [HardcodedSecret] — Line 18

**OWASP:** `A02:2021`  **Severity:** `CRITICAL`  

**Description:** Hardcoded secret/credential detected — store in environment variables or a vault.  

**Remediation:** Move secrets to .env files and load via os.environ.get().

```python
api_key = "sk-prod-abc123456789"
```

### 🟠 [DebugModeEnabled] — Line 21

**OWASP:** `A05:2021`  **Severity:** `HIGH`  

**Description:** Debug mode enabled in application — discloses stack traces and sensitive info.  

**Remediation:** Set DEBUG=False in production; use environment-based config.

```python
DEBUG = True  # Security: DebugModeEnabled, OWASP A05
```

### 🟠 [WeakCryptography] — Line 44

**OWASP:** `A02:2021`  **Severity:** `HIGH`  

**Description:** Weak hashing algorithm (MD5/SHA1) detected — use SHA-256 or bcrypt for passwords.  

**Remediation:** Use hashlib.sha256() or bcrypt for password hashing.

```python
return hashlib.MD5(pw.encode()).hexdigest()
```

### 🔴 [SQLInjection] — Line 52

**OWASP:** `A03:2021`  **Severity:** `CRITICAL`  

**Description:** Possible SQL injection via string concatenation — use parameterized queries.  

**Remediation:** Use parameterized queries: cursor.execute(sql, (param,))

```python
query = "SELECT * FROM users WHERE name = '" + user + "'"  # noqa
```

### 🔴 [InsecureDeserialization] — Line 58

**OWASP:** `A08:2021`  **Severity:** `CRITICAL`  

**Description:** Deserialization of untrusted data via pickle — use JSON or authenticated serialization.  

**Remediation:** Replace pickle with json.loads() or a signed serialization format.

```python
return pickle.loads(session_bytes)
```

### 🟡 [InsecureRandom] — Line 62

**OWASP:** `A02:2021`  **Severity:** `MEDIUM`  

**Description:** Non-cryptographic random used — use 'secrets' module for security-sensitive values.  

**Remediation:** Replace random with the 'secrets' module for security-sensitive values.

```python
return str(random.randint(100000, 999999))
```

### 🔴 [CodeInjection] — Line 66

**OWASP:** `A03:2021`  **Severity:** `CRITICAL`  

**Description:** Potential code/command injection — user input must NEVER reach exec/eval/system calls.  

**Remediation:** Avoid exec/eval on user input; use ast.literal_eval for safe parsing.

```python
exec(user_input)
```

### 🟠 [TLSVerificationDisabled] — Line 77

**OWASP:** `A02:2021`  **Severity:** `HIGH`  

**Description:** SSL/TLS certificate verification is disabled — MITM attacks are possible.  

**Remediation:** Remove verify=False; configure proper CA bundle.

```python
return requests.get(url, verify=False)  # MITM attack possible!
```

### 🟡 [UnvalidatedInput] — Line 93

**OWASP:** `A01:2021`  **Severity:** `MEDIUM`  

**Description:** User-supplied input used directly — validate and sanitize before use.  

**Remediation:** Validate input length, type, and allowed characters before use.

```python
user_id = flask_request.args["user_id"]  # No validation!
```


## 📌 CVE References

- [CVE-2014-0160](https://nvd.nist.gov/vuln/detail/CVE-2014-0160)

- [CVE-2016-2183](https://nvd.nist.gov/vuln/detail/CVE-2016-2183)

- [CVE-2017-16516](https://nvd.nist.gov/vuln/detail/CVE-2017-16516)

- [CVE-2019-0232](https://nvd.nist.gov/vuln/detail/CVE-2019-0232)

- [CVE-2020-14145](https://nvd.nist.gov/vuln/detail/CVE-2020-14145)

- [CVE-2021-37580](https://nvd.nist.gov/vuln/detail/CVE-2021-37580)

- [CVE-2021-41773](https://nvd.nist.gov/vuln/detail/CVE-2021-41773)

- [CVE-2021-44228](https://nvd.nist.gov/vuln/detail/CVE-2021-44228)

- [CVE-2022-22965](https://nvd.nist.gov/vuln/detail/CVE-2022-22965)

- [CVE-2022-26134](https://nvd.nist.gov/vuln/detail/CVE-2022-26134)

- [CVE-2023-23638](https://nvd.nist.gov/vuln/detail/CVE-2023-23638)


## ⚠️ OWASP Top 10 Categories Triggered

- `A01:2021`

- `A02:2021`

- `A03:2021`

- `A05:2021`

- `A08:2021`


## 🔧 Suggested Fixes

### Fix for [MutableDefaultArgument] — Line 24 — ✅ Auto-applied

**Confidence:** 85%  
**Explanation:** Replace mutable default with None and initialize inside the function body:
    if param is None:
        param = []


**Before:**
```python
def add_user_to_list(user, user_list=[]):
```

**After:**
```python
def add_user_to_list(user, user_list= None  # was: [] (mutable default)):
```

### Fix for [AssertInProduction] — Line 70 — ✅ Auto-applied

**Confidence:** 80%  
**Explanation:** Replace assert statements with explicit conditional raises — assert is stripped when Python is run with -O (optimize) flag.


**Before:**
```python
    assert amount > 0, "Amount must be positive"  # Stripped by -O flag!
```

**After:**
```python
    if not (amount > 0): raise AssertionError(, "Amount must be positive"  # Stripped by -O flag!)
```

### Fix for [BareExcept] — Line 33 — ✅ Auto-applied

**Confidence:** 95%  
**Explanation:** Replace bare 'except:' with 'except Exception:' to avoid swallowing system-level exceptions like KeyboardInterrupt.


**Before:**
```python
    except:
```

**After:**
```python
    except Exception:
```

### Fix for [ZeroDivision] — Line 40 — ⚠️ Manual review required

**Confidence:** 90%  
**Explanation:** Wrap division in a zero-guard conditional before performing the operation.


**Before:**
```python
    return total / 0
```

**After:**
```python
    # FIX: Guard against division by zero
    if divisor != 0:
        result = numerator / divisor
    else:
        result = None  # or raise ValueError
```

### Fix for [HardcodedCredential] — Line 17 — ✅ Auto-applied

**Confidence:** 92%  
**Explanation:** Load secrets from environment variables. Add 'import os' at the top and store the value in a .env file (never commit to version control).


**Before:**
```python
password = "supersecret123"
```

**After:**
```python
password = os.environ.get("PASSWORD")
```

### Fix for [HardcodedCredential] — Line 18 — ✅ Auto-applied

**Confidence:** 92%  
**Explanation:** Load secrets from environment variables. Add 'import os' at the top and store the value in a .env file (never commit to version control).


**Before:**
```python
api_key = "sk-prod-abc123456789"
```

**After:**
```python
api_key = os.environ.get("API_KEY")
```

### Fix for [HardcodedSecret] — Line 17 — ✅ Auto-applied

**Confidence:** 92%  
**Explanation:** Store secrets in environment variables or a secrets manager (e.g., AWS Secrets Manager).


**Before:**
```python
password = "supersecret123"
```

**After:**
```python
password = os.environ.get("PASSWORD")
```

### Fix for [HardcodedSecret] — Line 18 — ✅ Auto-applied

**Confidence:** 92%  
**Explanation:** Store secrets in environment variables or a secrets manager (e.g., AWS Secrets Manager).


**Before:**
```python
api_key = "sk-prod-abc123456789"
```

**After:**
```python
api_key = os.environ.get("API_KEY")
```

### Fix for [DebugModeEnabled] — Line 21 — ✅ Auto-applied

**Confidence:** 90%  
**Explanation:** Control debug mode via environment variable — never hardcode True in production.


**Before:**
```python
DEBUG = True  # Security: DebugModeEnabled, OWASP A05
```

**After:**
```python
DEBUG = os.environ.get("DEBUG", "False") == "True"  # Security: DebugModeEnabled, OWASP A05
```

### Fix for [WeakCryptography] — Line 44 — ✅ Auto-applied

**Confidence:** 88%  
**Explanation:** Replace MD5/SHA1 with SHA-256 for data integrity, or bcrypt/argon2 for passwords.


**Before:**
```python
return hashlib.MD5(pw.encode()).hexdigest()
```

**After:**
```python
return hashlib.hashlib.sha256(pw.encode()).hexdigest()
```

### Fix for [SQLInjection] — Line 52 — ⚠️ Manual review required

**Confidence:** 70%  
**Explanation:** NEVER concatenate user input into SQL strings. Use parameterized queries or an ORM (SQLAlchemy, Django ORM) to prevent SQL injection.


**Before:**
```python
query = "SELECT * FROM users WHERE name = '" + user + "'"  # noqa
```

**After:**
```python
# FIX: Use parameterized query
cursor.execute('SELECT * FROM table WHERE id = %s', (user_input,))
```

### Fix for [InsecureRandom] — Line 62 — ✅ Auto-applied

**Confidence:** 85%  
**Explanation:** Replace the 'random' module with the 'secrets' module for cryptographically secure random number generation.


**Before:**
```python
return str(random.randint(100000, 999999))
```

**After:**
```python
return str(secrets.randint(100000, 999999))
```

### Fix for [TLSVerificationDisabled] — Line 77 — ✅ Auto-applied

**Confidence:** 97%  
**Explanation:** Re-enable SSL verification. If using a custom CA, pass verify='/path/to/ca-bundle.crt'.


**Before:**
```python
return requests.get(url, verify=False)  # MITM attack possible!
```

**After:**
```python
return requests.get(url, verify=True)  # MITM attack possible!
```


## 📝 Patched Source Code

```python
"""
sample_buggy_code.py
====================
Intentionally flawed Python file used to demonstrate the CTSE MAS pipeline.
Contains examples of code quality bugs AND security vulnerabilities.

DO NOT USE THIS CODE IN PRODUCTION.
"""

import os
import random
import pickle
import hashlib
import sqlite3

# ── Hardcoded credentials (Security: HardcodedSecret, OWASP A02) ─────────────
password = os.environ.get("PASSWORD")
api_key = os.environ.get("API_KEY")
DB_HOST = "localhost"

DEBUG = os.environ.get("DEBUG", "False") == "True"  # Security: DebugModeEnabled, OWASP A05

# ── Mutable default argument (Bug: MutableDefaultArgument) ───────────────────
def add_user_to_list(user, user_list= None  # was: [] (mutable default)):
    user_list.append(user)
    return user_list

# ── Bare except (Bug: BareExcept) ─────────────────────────────────────────────
def read_config(path):
    try:
        with open(path) as f:
            return f.read()
    except Exception:
        return None

# ── Division by zero (Bug: ZeroDivision) ──────────────────────────────────────
def bad_average():
    total = 100
    count = 0
    return total / 0

# ── Weak crypto (Security: WeakCryptography, OWASP A02) ──────────────────────
def hash_password(pw):
return hashlib.hashlib.sha256(pw.encode()).hexdigest()

# ── SQL Injection (Security: SQLInjection, OWASP A03) ─────────────────────────
def get_user(username):
    conn = sqlite3.connect("users.db")
    cur = conn.cursor()
    # DANGER: string concatenation with user input
    user = username
    query = "SELECT * FROM users WHERE name = '" + user + "'"  # noqa
    cur.execute(query)
    return cur.fetchall()

# ── Insecure deserialization (Security: InsecureDeserialization, OWASP A08) ───
def load_session(session_bytes):
    return pickle.loads(session_bytes)

# ── Insecure random (Security: InsecureRandom, OWASP A02) ────────────────────
def generate_token():
return str(secrets.randint(100000, 999999))

# ── Code injection (Security: CodeInjection, OWASP A03) ──────────────────────
def run_command(user_input):
    exec(user_input)

# ── Assert in production (Bug: AssertInProduction) ────────────────────────────
def process_order(amount):
    if not (amount > 0): raise AssertionError(, "Amount must be positive"  # Stripped by -O flag!)
    return amount * 1.15  # TODO: apply discount logic here

# ── TLS verification disabled (Security: TLSVerificationDisabled, OWASP A02) ─
import requests

def fetch_data(url):
return requests.get(url, verify=True)  # MITM attack possible!

# ── Magic numbers (Bug: MagicNumber) ─────────────────────────────────────────
def apply_discount(price):
    if price > 5000:          # Magic: what is 5000?
        return price * 0.85   # Magic: what is 0.85?
    elif price > 1500:        # Magic: what is 1500?
        return price * 0.90   # Magic: what is 0.90?
    return price

# ── Unvalidated user input (Security: UnvalidatedInput, OWASP A01) ────────────
from flask import Flask, request as flask_request
app = Flask(__name__)

@app.route("/profile")
def profile():
    user_id = flask_request.args["user_id"]  # No validation!
    return f"<h1>User {user_id}</h1>"

if __name__ == "__main__":
    print("Running sample...")
    print(add_user_to_list("Alice"))
    print(hash_password("mypassword"))
    print(generate_token())
    print(apply_discount(6000))
```


---

*Report generated by the CTSE MAS — Multi-Agent Software Analysis System*
