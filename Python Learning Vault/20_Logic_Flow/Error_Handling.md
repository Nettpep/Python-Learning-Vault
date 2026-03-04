# Error Handling - การรับมือกับสิ่งที่ไม่คาดฝัน/Exception

## 🛡️ แนวคิดพื้นฐาน

Error Handling คือ **การจัดการข้อผิดพลาด** ที่อาจเกิดขึ้นระหว่างการทำงานของโปรแกรม เพื่อให้โปรแกรมไม่หยุดทำงานและสามารถจัดการสถานการณ์ได้อย่างเหมาะสม

```python
# โครงสร้างพื้นฐาน
try:
    # โค้ดที่อาจเกิด error
    risky_code()
except Exception as e:
    # จัดการเมื่อเกิด error
    handle_error(e)
finally:
    # ทำเสมอ (ไม่ว่าจะ error หรือไม่)
    cleanup()
```

---

## 🎯 ประเภทของ Errors

### Syntax Errors
```python
# ❌ Syntax Error: ผิดกฎไวยากรณ์
print("Hello World"  # ขาดวงเล็บปิด

# ❌ Syntax Error: การย่อหน้าผิด
if True:
print("Hello")  # ไม่ได้ย่อหน้า

# ❌ Syntax Error: ใช้คำสงวน
class = "Hello"  # class เป็นคำสงวน
```

### Runtime Errors (Exceptions)
```python
# ❌ ZeroDivisionError
result = 10 / 0

# ❌ TypeError
result = "5" + 5

# ❌ ValueError
number = int("abc")

# ❌ IndexError
my_list = [1, 2, 3]
value = my_list[5]

# ❌ KeyError
my_dict = {"name": "John"}
value = my_dict["age"]

# ❌ FileNotFoundError
with open("nonexistent.txt", "r") as f:
    content = f.read()
```

---

## 🔧 Try-Except พื้นฐาน

### การจัดการ Exception แบบพื้นฐาน
```python
# จัดการ exception ทั่วไป
try:
    result = 10 / 0
except Exception as e:
    print(f"เกิดข้อผิดพลาด: {e}")
    print(f"ประเภท error: {type(e).__name__}")

# จัดการ exception ที่ต้องการ
try:
    number = int("abc")
except ValueError as e:
    print(f"ไม่สามารถแปลงเป็นตัวเลข: {e}")

# จัดการหลายประเภท
try:
    # โค้ดที่อาจเกิดหลาย error
    result = 10 / int("0")
except ZeroDivisionError:
    print("หารด้วยศูนย์ไม่ได้")
except ValueError:
    print("ต้องเป็นตัวเลขเท่านั้น")
except Exception as e:
    print(f"เกิด error อื่น: {e}")
```

### การใช้ Else และ Finally
```python
def divide_numbers(a, b):
    """หารตัวเลขพร้อมจัดการ error"""
    try:
        result = a / b
    except ZeroDivisionError:
        print("Error: ไม่สามารถหารด้วยศูนย์"
        return None
    except TypeError:
        print("Error: ต้องเป็นตัวเลขเท่านั้น")
        return None
    else:
        # ทำงานเมื่อไม่มี error
        print(f"ผลลัพธ์: {result}")
        return result
    finally:
        # ทำเสมอ (ไม่ว่าจะ error หรือไม่)
        print("การหารเสร็จสิ้น")

# การใช้งาน
divide_numbers(10, 2)  # สำเร็จ
divide_numbers(10, 0)  # Error
```

---

## 🎪 การจัดการ Exception ขั้นสูง

### การสร้าง Custom Exception
```python
class CustomError(Exception):
    """Custom exception สำหรับโปรแกรม"""
    pass

class ValidationError(CustomError):
    """Exception สำหรับการตรวจสอบข้อมูล"""
    def __init__(self, message, field=None):
        super().__init__(message)
        self.field = field

class InsufficientFundsError(CustomError):
    """Exception สำหรับเงินไม่พอ"""
    def __init__(self, message, balance=None, amount=None):
        super().__init__(message)
        self.balance = balance
        self.amount = amount

# การใช้งาน
def validate_age(age):
    """ตรวจสอบอายุ"""
    if age < 0:
        raise ValidationError("อายุไม่สามารถเป็นลบได้", field="age")
    if age > 150:
        raise ValidationError("อายุไม่น่าจะเกิน 150", field="age")
    return True

def withdraw_money(balance, amount):
    """ถอนเงิน"""
    if amount > balance:
        raise InsufficientFundsError(
            "ยอดเงินไม่พอ",
            balance=balance,
            amount=amount
        )
    return balance - amount
```

### Exception Chaining
```python
def process_data(data):
    """ประมวลผลข้อมูล"""
    try:
        # ขั้นตอนที่ 1: ตรวจสอบข้อมูล
        if not data:
            raise ValueError("ข้อมูลว่าง")
        
        # ขั้นตอนที่ 2: แปลงข้อมูล
        number = int(data)
        
        # ขั้นตอนที่ 3: คำนวณ
        result = 100 / number
        return result
        
    except ValueError as e:
        # เพิ่มข้อมูลและ raise ต่อ
        raise ValueError(f"ข้อมูลไม่ถูกต้อง: {e}") from e
    except ZeroDivisionError as e:
        raise ZeroDivisionError("ไม่สามารถหารด้วยศูนย์ได้") from e

# การใช้งาน
try:
    result = process_data("0")
except ValueError as e:
    print(f"Error: {e}")
    print(f"Original error: {e.__cause__}")
```

### การจัดการ Context Manager
```python
class FileHandler:
    """Context manager สำหรับจัดการไฟล์"""
    def __init__(self, filename, mode='r'):
        self.filename = filename
        self.mode = mode
        self.file = None
    
    def __enter__(self):
        try:
            self.file = open(self.filename, self.mode, encoding='utf-8')
            return self.file
        except FileNotFoundError:
            raise FileNotFoundError(f"ไม่พบไฟล์: {self.filename}")
        except PermissionError:
            raise PermissionError(f"ไม่มีสิทธิ์เข้าถึงไฟล์: {self.filename}")
    
    def __exit__(self, exc_type, exc_val, exc_tb):
        if self.file:
            self.file.close()
        
        # ถ้ามี exception จะคืน True เพื่อไม่ให้ propagate
        if exc_type is not None:
            print(f"เกิด error: {exc_val}")
            return False  # ให้ exception ดำเนินการต่อ
        return True

# การใช้งาน
try:
    with FileHandler("test.txt", "r") as f:
        content = f.read()
        print(content)
except FileNotFoundError as e:
    print(f"Error: {e}")
```

---

## 🛠️ การใช้งานจริง

### การตรวจสอบ Input ผู้ใช้
```python
def get_user_age():
    """รับอายุผู้ใช้พร้อมตรวจสอบ"""
    while True:
        try:
            age_input = input("กรุณาใส่อายุ (0-150): ")
            age = int(age_input)
            
            # ตรวจสอบช่วง
            if age < 0:
                print("อายุไม่สามารถเป็นลบได้")
                continue
            elif age > 150:
                print("อายุไม่น่าจะเกิน 150")
                continue
            else:
                return age
                
        except ValueError:
            print("กรุณาใส่ตัวเลขเท่านั้น")
        except KeyboardInterrupt:
            print("\nยกเลิกการทำงาน")
            return None
        except Exception as e:
            print(f"เกิดข้อผิดพลาดไม่คาดคิด: {e}")
            return None

# การใช้งาน
age = get_user_age()
if age is not None:
    print(f"อายุของคุณคือ {age} ปี")
```

### การจัดการ API Calls
```python
import requests
import time
from typing import Optional, Dict, Any

class APIClient:
    """Client สำหรับเรียกใช้ API"""
    
    def __init__(self, base_url: str, max_retries: int = 3):
        self.base_url = base_url
        self.max_retries = max_retries
    
    def make_request(self, endpoint: str, method: str = "GET", 
                    data: Optional[Dict] = None) -> Optional[Dict]:
        """เรียก API พร้อม retry"""
        url = f"{self.base_url}/{endpoint}"
        
        for attempt in range(self.max_retries):
            try:
                if method.upper() == "GET":
                    response = requests.get(url, timeout=10)
                elif method.upper() == "POST":
                    response = requests.post(url, json=data, timeout=10)
                else:
                    raise ValueError(f"Method {method} not supported")
                
                # ตรวจสอบ status code
                response.raise_for_status()
                return response.json()
                
            except requests.exceptions.ConnectionError:
                print(f"Attempt {attempt + 1}: Connection error")
            except requests.exceptions.Timeout:
                print(f"Attempt {attempt + 1}: Timeout")
            except requests.exceptions.HTTPError as e:
                if e.response.status_code == 404:
                    print("Resource not found")
                    return None
                elif e.response.status_code >= 500:
                    print(f"Server error: {e.response.status_code}")
                else:
                    print(f"HTTP error: {e.response.status_code}")
                    return None
            except requests.exceptions.RequestException as e:
                print(f"Request error: {e}")
            
            # รอก่อน retry
            if attempt < self.max_retries - 1:
                time.sleep(2 ** attempt)  # Exponential backoff
        
        print("Max retries reached")
        return None

# การใช้งาน
client = APIClient("https://api.example.com")
result = client.make_request("users/123")
if result:
    print("Success:", result)
else:
    print("Failed to get data")
```

### การจัดการ Database Operations
```python
import sqlite3
from contextlib import contextmanager

class DatabaseManager:
    """จัดการการทำงานกับฐานข้อมูล"""
    
    def __init__(self, db_path: str):
        self.db_path = db_path
    
    @contextmanager
    def get_connection(self):
        """Context manager สำหรับ database connection"""
        conn = None
        try:
            conn = sqlite3.connect(self.db_path)
            conn.row_factory = sqlite3.Row  # ให้ access แบบ dict
            yield conn
        except sqlite3.Error as e:
            if conn:
                conn.rollback()
            raise DatabaseError(f"Database error: {e}")
        finally:
            if conn:
                conn.close()
    
    def execute_query(self, query: str, params: tuple = ()) -> list:
        """Execute query และคืนผลลัพธ์"""
        try:
            with self.get_connection() as conn:
                cursor = conn.cursor()
                cursor.execute(query, params)
                return [dict(row) for row in cursor.fetchall()]
        except sqlite3.Error as e:
            raise DatabaseError(f"Query execution failed: {e}")
    
    def execute_update(self, query: str, params: tuple = ()) -> int:
        """Execute update query"""
        try:
            with self.get_connection() as conn:
                cursor = conn.cursor()
                cursor.execute(query, params)
                conn.commit()
                return cursor.rowcount
        except sqlite3.Error as e:
            raise DatabaseError(f"Update execution failed: {e}")

class DatabaseError(Exception):
    """Custom exception สำหรับ database"""
    pass

# การใช้งาน
db = DatabaseManager("example.db")

try:
    users = db.execute_query("SELECT * FROM users WHERE age > ?", (18,))
    print(f"Found {len(users)} users")
except DatabaseError as e:
    print(f"Database error: {e}")
```

---

## 🎯 การเพิ่มประสิทธิภาพ

### การใช้ Logging กับ Error Handling
```python
import logging
from datetime import datetime

# ตั้งค่า logging
logging.basicConfig(
    level=logging.INFO,
    format='%(asctime)s - %(levelname)s - %(message)s',
    filename='app.log'
)

class ErrorHandler:
    """คลาสสำหรับจัดการ error และ logging"""
    
    @staticmethod
    def log_error(error: Exception, context: str = ""):
        """บันทึก error ลง log"""
        error_info = {
            'timestamp': datetime.now().isoformat(),
            'type': type(error).__name__,
            'message': str(error),
            'context': context
        }
        logging.error(f"Error in {context}: {error_info}")
    
    @staticmethod
    def handle_with_logging(func):
        """Decorator สำหรับจัดการ error และ logging"""
        def wrapper(*args, **kwargs):
            try:
                return func(*args, **kwargs)
            except Exception as e:
                ErrorHandler.log_error(e, func.__name__)
                raise
        return wrapper

# การใช้งาน
@ErrorHandler.handle_with_logging
def risky_operation(data):
    """ฟังก์ชันที่อาจเกิด error"""
    if not data:
        raise ValueError("Data cannot be empty")
    return len(data) * 2

try:
    result = risky_operation("")
except ValueError as e:
    print(f"Caught error: {e}")
```

### การใช้ Circuit Breaker Pattern
```python
import time
from enum import Enum

class CircuitState(Enum):
    CLOSED = "closed"      # ปกติ
    OPEN = "open"          # เปิด (ไม่ให้ผ่าน)
    HALF_OPEN = "half_open"  # ครึ่งเปิด (ทดสอบ)

class CircuitBreaker:
    """Circuit Breaker สำหรับป้องกัน cascade failure"""
    
    def __init__(self, failure_threshold: int = 5, 
                 recovery_timeout: int = 60):
        self.failure_threshold = failure_threshold
        self.recovery_timeout = recovery_timeout
        self.failure_count = 0
        self.last_failure_time = None
        self.state = CircuitState.CLOSED
    
    def __call__(self, func):
        def wrapper(*args, **kwargs):
            if self.state == CircuitState.OPEN:
                if self._should_attempt_reset():
                    self.state = CircuitState.HALF_OPEN
                else:
                    raise Exception("Circuit breaker is OPEN")
            
            try:
                result = func(*args, **kwargs)
                self._on_success()
                return result
            except Exception as e:
                self._on_failure()
                raise
        return wrapper
    
    def _should_attempt_reset(self) -> bool:
        """ตรวจสอบว่าควรพยายาม reset หรือไม่"""
        return (time.time() - self.last_failure_time) >= self.recovery_timeout
    
    def _on_success(self):
        """เมื่อสำเร็จ"""
        self.failure_count = 0
        self.state = CircuitState.CLOSED
    
    def _on_failure(self):
        """เมื่อล้มเหลว"""
        self.failure_count += 1
        self.last_failure_time = time.time()
        
        if self.failure_count >= self.failure_threshold:
            self.state = CircuitState.OPEN

# การใช้งาน
@CircuitBreaker(failure_threshold=3, recovery_timeout=30)
def unreliable_api_call():
    """API call ที่ไม่น่าเชื่อถือ"""
    import random
    if random.random() < 0.7:  # 70% โอกาสล้มเหลว
        raise Exception("API call failed")
    return "Success"

# ทดสอบ
for i in range(10):
    try:
        result = unreliable_api_call()
        print(f"Call {i+1}: {result}")
    except Exception as e:
        print(f"Call {i+1}: {e}")
    time.sleep(1)
```

---

## 💡 Best Practices

### 1. จัดการ Exception ที่จำเพาะ
```python
# ✅ ดี: จัดการ exception ที่ต้องการ
try:
    number = int(user_input)
except ValueError:
    print("กรุณาใส่ตัวเลขเท่านั้น")

# ❌ ไม่ดี: จัดการทุกอย่าง
try:
    number = int(user_input)
except Exception:
    print("เกิดข้อผิดพลาด")
```

### 2. ใช้ Finally สำหรับ Cleanup
```python
# ✅ ดี: ใช้ finally สำหรับ cleanup
file = None
try:
    file = open("data.txt", "r")
    content = file.read()
    # process content
finally:
    if file:
        file.close()

# ✅ ดีกว่า: ใช้ context manager
with open("data.txt", "r") as file:
    content = file.read()
    # process content
```

### 3. สร้าง Custom Exception ที่มีความหมาย
```python
# ✅ ดี: Custom exception ที่ชัดเจน
class InsufficientFundsError(Exception):
    def __init__(self, balance, amount):
        self.balance = balance
        self.amount = amount
        super().__init__(f"ยอดเงินไม่พอ: {balance} < {amount}")

# ❌ ไม่ดี: ใช้ generic exception
raise Exception("เงินไม่พอ")
```

### 4. Log Error อย่างสม่ำเสมอ
```python
# ✅ ดี: บันทึก error พร้อม context
try:
    process_data(data)
except ValueError as e:
    logger.error(f"Data processing failed: {e}, data={data}")
    raise
```

---

## 📝 สรุป Exception Hierarchy

```
BaseException
├── SystemExit
├── KeyboardInterrupt
├── GeneratorExit
└── Exception
    ├── StopIteration
    ├── ArithmeticError
    │   ├── ZeroDivisionError
    │   ├── OverflowError
    │   └── FloatingPointError
    ├── LookupError
    │   ├── IndexError
    │   └── KeyError
    ├── NameError
    ├── UnboundLocalError
    ├── AttributeError
    ├── TypeError
    ├── ValueError
    ├── UnicodeError
    ├── RuntimeError
    ├── OSError
    │   ├── FileNotFoundError
    │   ├── PermissionError
    │   └── TimeoutError
    └── Custom Exceptions
```

---

## 🎯 แบบฝึกหัด

### แบบฝึกหัด 1: ตรวจสอบ Input
```python
# เขียนฟังก์ชันรับอายุพร้อมตรวจสอบ
def get_valid_age():
    pass
```

### แบบฝึกหัด 2: Custom Exception
```python
# สร้าง custom exception สำหรับการถอนเงิน
class BankAccount:
    def withdraw(self, amount):
        pass
```

### แบบฝึกหัด 3: File Processing
```python
# เขียนฟังก์ชันประมวลผลไฟล์พร้อม error handling
def process_file(filename):
    pass
```

---

*สร้างเมื่อ: 4 มีนาคม 2026*  
*ปรับปรุงล่าสุด: 4 มีนาคม 2026*
