# Function Definitions - การสร้างชุดคำสั่งสั่งการ

## 🛠️ แนวคิดพื้นฐาน

Function คือ **ชุดคำสั่งที่มีชื่อ** ที่สามารถเรียกใช้ซ้ำๆ ได้ ช่วยให้โค้ดเป็นระเบียบ อ่านง่าย และบำรุงรักษาง่าย

```python
# โครงสร้างพื้นฐาน
def function_name(parameters):
    """คำอธิบายฟังก์ชัน (docstring)"""
    # คำสั่งต่างๆ
    return result

# การเรียกใช้
result = function_name(arguments)
```

---

## 📝 การสร้างฟังก์ชันพื้นฐาน

### ฟังก์ชันแบบง่าย
```python
# ฟังก์ชันไม่มีพารามิเตอร์
def say_hello():
    """ทักทาย"""
    print("Hello, World!")

# ฟังก์ชันมีพารามิเตอร์
def greet(name):
    """ทักทายตามชื่อ"""
    print(f"Hello, {name}!")

# ฟังก์ชันมีการคืนค่า
def add_numbers(a, b):
    """บวกเลขสองตัว"""
    return a + b

# ฟังก์ชันมี default parameter
def greet_with_title(name, title="คุณ"):
    """ทักทายพร้อมคำนำหน้า"""
    return f"สวัสดี{title}{name}"

# การเรียกใช้
say_hello()                           # Hello, World!
greet("สมชาย")                        # Hello, สมชาย!
result = add_numbers(5, 3)             # 8
message = greet_with_title("วิชัย")   # สวัสดีคุณวิชัย
message = greet_with_title("มานี", "นาง")  # สวัสดีนางมานี
```

### พารามิเตอร์หลายแบบ
```python
# Positional arguments
def calculate_area(width, height):
    """คำนวณพื้นที่สี่เหลี่ยม"""
    return width * height

# Keyword arguments
def create_user(name, age, city="กรุงเทพฯ"):
    """สร้างข้อมูลผู้ใช้"""
    return {"name": name, "age": age, "city": city}

# การเรียกใช้แบบต่างๆ
area1 = calculate_area(10, 5)           # positional
area2 = calculate_area(width=10, height=5)  # keyword

user1 = create_user("สมชาย", 25)        # ใช้ default city
user2 = create_user("วิชัย", 30, "เชียงใหม่")  # ระบุทั้งหมด
user3 = create_user(age=28, name="มานี")  # keyword สลับลำดับได้
```

---

## 🎪 พารามิเตอร์ขั้นสูง

### *args และ **kwargs
```python
# *args - รับ arguments จำนวนไม่จำกัด
def sum_all(*numbers):
    """บวกเลขทั้งหมด"""
    total = 0
    for num in numbers:
        total += num
    return total

# **kwargs - รับ keyword arguments จำนวนไม่จำกัด
def print_info(**info):
    """แสดงข้อมูล"""
    for key, value in info.items():
        print(f"{key}: {value}")

# ผสมกัน
def flexible_function(*args, **kwargs):
    """ฟังก์ชันยืดหยุ่น"""
    print("Positional args:", args)
    print("Keyword args:", kwargs)

# การเรียกใช้
total = sum_all(1, 2, 3, 4, 5)        # 15
print_info(name="สมชาย", age=25, city="กรุงเทพฯ")
flexible_function(1, 2, 3, name="test", value=42)
```

### การกำหนดค่าเริ่มต้นซับซ้อน
```python
# ใช้ None เป็น default สำหรับ mutable objects
def process_list(items=None):
    """ประมวลผล list"""
    if items is None:
        items = []
    # ทำงานกับ items
    return len(items)

# ใช้ function เป็น default
def calculate(operation, a, b):
    """คำนวณตาม operation"""
    operations = {
        "add": lambda x, y: x + y,
        "subtract": lambda x, y: x - y,
        "multiply": lambda x, y: x * y,
        "divide": lambda x, y: x / y if y != 0 else None
    }
    return operations.get(operation, lambda x, y: None)(a, b)

# การเรียกใช้
result1 = process_list()               # 0
result2 = process_list([1, 2, 3])      # 3

sum_result = calculate("add", 5, 3)     # 8
div_result = calculate("divide", 10, 2)  # 5.0
```

---

## 🎯 Return Values

### การคืนค่าหลายค่า
```python
# คืนค่า tuple
def get_coordinates():
    """ดึงพิกัด"""
    x = 10
    y = 20
    return x, y  # คืน (10, 20)

# คืนค่า list
def get_user_info():
    """ดึงข้อมูลผู้ใช้"""
    return ["สมชาย", 25, "กรุงเทพฯ"]

# คืนค่า dictionary
def create_profile(name, age):
    """สร้างโปรไฟล์"""
    return {
        "name": name,
        "age": age,
        "created_at": datetime.datetime.now()
    }

# การรับค่า
x, y = get_coordinates()               # x=10, y=20
name, age, city = get_user_info()       # tuple unpacking
profile = create_profile("วิชัย", 30)  # dictionary
```

### การคืนค่าแบบมีเงื่อนไข
```python
def divide_numbers(a, b):
    """หารตัวเลขอย่างปลอดภัย"""
    if b == 0:
        return None, "Cannot divide by zero"
    return a / b, "Success"

def find_maximum(numbers):
    """หาค่าสูงสุด"""
    if not numbers:
        return None
    max_val = numbers[0]
    for num in numbers[1:]:
        if num > max_val:
            max_val = num
    return max_val

def get_grade(score):
    """คำนวณเกรด"""
    if score >= 90:
        return "A", "ดีเยี่ยม"
    elif score >= 80:
        return "B", "ดี"
    elif score >= 70:
        return "C", "พอใช้"
    elif score >= 60:
        return "D", "ต้องปรับปรุง"
    else:
        return "F", "ไม่ผ่าน"

# การใช้งาน
result, message = divide_numbers(10, 2)   # 5.0, "Success"
result, message = divide_numbers(10, 0)   # None, "Cannot divide by zero"

max_val = find_maximum([1, 5, 3, 9, 2])    # 9
grade, comment = get_grade(85)             # "B", "ดี"
```

---

## 🛠️ Function Scope และ Lifetime

### Local vs Global Scope
```python
# Global variable
global_var = "I am global"

def test_scope():
    """ทดสอบ scope"""
    local_var = "I am local"
    print(global_var)  # อ่านได้
    print(local_var)   # อ่านได้

test_scope()
# print(local_var)    # Error: local_var ไม่มีใน global scope

# แก้ไข global variable
counter = 0

def increment_counter():
    """เพิ่ม counter"""
    global counter  # บอกว่าจะแก้ไขตัวแปร global
    counter += 1

def reset_counter():
    """รีเซ็ต counter"""
    global counter
    counter = 0

# Nonlocal variable (nested functions)
def outer_function():
    """ฟังก์ชันนอก"""
    outer_var = "outer"
    
    def inner_function():
        nonlocal outer_var  # แก้ไขตัวแปรจากฟังก์ชันนอก
        outer_var = "modified outer"
        print(outer_var)
    
    inner_function()
    print(outer_var)

outer_function()
```

### Closures
```python
def make_multiplier(factor):
    """สร้างฟังก์ชันคูณ"""
    def multiplier(number):
        return number * factor
    return multiplier

# สร้างฟังก์ชันคูณ
double = make_multiplier(2)
triple = make_multiplier(3)

print(double(5))   # 10
print(triple(5))   # 15

# Closure กับ loop
def create_power_functions():
    """สร้างฟังก์ชันยกกำลัง"""
    functions = []
    for i in range(1, 4):
        def power(n, power=i):  # ใช้ default argument เพื่อ capture value
            return n ** power
        functions.append(power)
    return functions

power2, power3, power4 = create_power_functions()
print(power2(3))   # 9
print(power3(3))   # 27
print(power4(3))   # 81
```

---

## 🎨 Decorators

### พื้นฐาน Decorators
```python
# Decorator พื้นฐาย
def timing_decorator(func):
    """วัดเวลาการทำงาน"""
    import time
    def wrapper(*args, **kwargs):
        start = time.time()
        result = func(*args, **kwargs)
        end = time.time()
        print(f"{func.__name__} took {end - start:.4f} seconds")
        return result
    return wrapper

# ใช้ decorator
@timing_decorator
def slow_function():
    """ฟังก์ชันที่ทำงานช้า"""
    import time
    time.sleep(1)
    return "Done"

# Decorator ที่มีพารามิเตอร์
def repeat_decorator(times):
    """ทำซ้ำฟังก์ชัน"""
    def decorator(func):
        def wrapper(*args, **kwargs):
            results = []
            for _ in range(times):
                result = func(*args, **kwargs)
                results.append(result)
            return results
        return wrapper
    return decorator

@repeat_decorator(3)
def greet(name):
    """ทักทาย"""
    return f"Hello, {name}!"

# การใช้งาน
slow_function()
greetings = greet("สมชาย")  # ["Hello, สมชาย!", "Hello, สมชาย!", "Hello, สมชาย!"]
```

### Decorators ขั้นสูง
```python
# Decorator สำหรับ logging
def log_decorator(func):
    """บันทึกการเรียกฟังก์ชัน"""
    import logging
    logging.basicConfig(level=logging.INFO)
    
    def wrapper(*args, **kwargs):
        logging.info(f"Calling {func.__name__} with args={args}, kwargs={kwargs}")
        result = func(*args, **kwargs)
        logging.info(f"{func.__name__} returned: {result}")
        return result
    return wrapper

# Decorator สำหรับ cache
def cache_decorator(func):
    """เก็บผลลัพธ์ไว้ใน cache"""
    cache = {}
    
    def wrapper(*args, **kwargs):
        key = str(args) + str(kwargs)
        if key not in cache:
            cache[key] = func(*args, **kwargs)
        return cache[key]
    return wrapper

# ใช้หลาย decorators
@log_decorator
@cache_decorator
@timing_decorator
def fibonacci(n):
    """คำนวณ Fibonacci"""
    if n <= 1:
        return n
    return fibonacci(n-1) + fibonacci(n-2)

# รักษาข้อมูลฟังก์ชันด้วย functools
import functools

def my_decorator(func):
    @functools.wraps(func)  # รักษา __name__, __doc__ ฯลฯ
    def wrapper(*args, **kwargs):
        print(f"Before {func.__name__}")
        result = func(*args, **kwargs)
        print(f"After {func.__name__}")
        return result
    return wrapper

@my_decorator
def example_function():
    """ฟังก์ชันตัวอย่าง"""
    pass

print(example_function.__name__)  # example_function
print(example_function.__doc__)   # ฟังก์ชันตัวอย่าง
```

---

## 🎪 Lambda Functions

### พื้นฐาน Lambda
```python
# Lambda พื้นฐาย
square = lambda x: x**2
add = lambda x, y: x + y
is_even = lambda x: x % 2 == 0

# ใช้กับ built-in functions
numbers = [1, 2, 3, 4, 5]
squared = list(map(lambda x: x**2, numbers))
evens = list(filter(lambda x: x % 2 == 0, numbers))

# ใช้กับ sorting
people = [
    {"name": "สมชาย", "age": 25},
    {"name": "วิชัย", "age": 30},
    {"name": "มานี", "age": 28}
]
sorted_by_age = sorted(people, key=lambda x: x["age"])

# Lambda ใน dictionary
operations = {
    "add": lambda x, y: x + y,
    "subtract": lambda x, y: x - y,
    "multiply": lambda x, y: x * y
}
```

### Lambda ขั้นสูง
```python
# Lambda กับ conditional
get_status = lambda score: "Pass" if score >= 50 else "Fail"

# Lambda กับ multiple expressions
process = lambda x: (x * 2, x + 1, x ** 2)  # คืน tuple

# Lambda ใน list comprehension
functions = [lambda x, i=i: x**i for i in range(3)]
# functions[0](2) = 2**0 = 1
# functions[1](2) = 2**1 = 2
# functions[2](2) = 2**2 = 4

# ข้อจำกัดของ lambda
# ❌ ไม่ได้: มีหลายคำสั่ง
# bad_lambda = lambda x: (y = x + 1; y * 2)

# ✅ ดีกว่า: ใช้ฟังก์ชันปกติ
def process_number(x):
    y = x + 1
    return y * 2
```

---

## 🛠️ การใช้งานจริง

### การจัดการข้อมูล
```python
def process_data(data, processors=None):
    """ประมวลผลข้อมูลด้วย processors หลายๆ ตัว"""
    if processors is None:
        processors = []
    
    result = data
    for processor in processors:
        result = processor(result)
    
    return result

# สร้าง processors
clean_data = lambda x: x.strip().lower()
remove_special = lambda x: ''.join(c for c in x if c.isalnum() or c.isspace())
split_words = lambda x: x.split()

# การใช้งาน
text = "  Hello, World!  "
processors = [clean_data, remove_special, split_words]
result = process_data(text, processors)  # ['hello', 'world']
```

### การสร้าง API Client
```python
class APIClient:
    """Client สำหรับเรียก API"""
    
    def __init__(self, base_url):
        self.base_url = base_url
        self.session = requests.Session()
    
    def make_request(self, endpoint, method="GET", **kwargs):
        """เรียก API พร้อมจัดการ error"""
        url = f"{self.base_url}/{endpoint}"
        
        try:
            response = self.session.request(method, url, **kwargs)
            response.raise_for_status()
            return response.json()
        except requests.exceptions.RequestException as e:
            self._handle_error(e)
            return None
    
    def _handle_error(self, error):
        """จัดการ error"""
        print(f"API Error: {error}")
    
    # Decorator methods
    def get(self, endpoint, **kwargs):
        """GET request"""
        return self.make_request(endpoint, "GET", **kwargs)
    
    def post(self, endpoint, **kwargs):
        """POST request"""
        return self.make_request(endpoint, "POST", **kwargs)

# การใช้งาน
client = APIClient("https://api.example.com")
users = client.get("users")
new_user = client.post("users", json={"name": "สมชาย"})
```

---

## 🎯 การเพิ่มประสิทธิภาพ

### การใช้ Memoization
```python
def memoize(func):
    """Decorator สำหรับ memoization"""
    cache = {}
    
    @functools.wraps(func)
    def wrapper(*args):
        if args not in cache:
            cache[args] = func(*args)
        return cache[args]
    
    return wrapper

@memoize
def fibonacci(n):
    """คำนวณ Fibonacci ด้วย memoization"""
    if n <= 1:
        return n
    return fibonacci(n-1) + fibonacci(n-2)

# ทดสอบประสิทธิภาพ
import time
start = time.time()
result = fibonacci(35)
end = time.time()
print(f"Fibonacci(35) = {result}, Time: {end - start:.4f}s")
```

### การใช้ Generator Functions
```python
def fibonacci_generator():
    """สร้างเลข Fibonacci"""
    a, b = 0, 1
    while True:
        yield a
        a, b = b, a + b

def read_large_file(filename):
    """อ่านไฟล์ขนาดใหญ่ทีละบรรทัด"""
    with open(filename, 'r', encoding='utf-8') as f:
        for line in f:
            yield line.strip()

# การใช้งาน
fib_gen = fibonacci_generator()
for _ in range(10):
    print(next(fib_gen))

for line in read_large_file("large_file.txt"):
    # ประมวลผลแต่ละบรรทัด
    process_line(line)
```

---

## 💡 Best Practices

### 1. ตั้งชื่อฟังก์ชันที่ชัดเจน
```python
# ✅ ดี: ชื่อบ่งบอกการทำงาน
def calculate_user_age(birth_date):
    """คำนวณอายุผู้ใช้"""
    pass

def validate_email_format(email):
    """ตรวจสอบรูปแบบ email"""
    pass

# ❌ ไม่ดี: ชื่อไม่ชัดเจน
def process(x):
    """ประมวลผลอะไรสักอย่าง"""
    pass

def do_stuff(data):
    """ทำอะไรสักอย่างกับข้อมูล"""
    pass
```

### 2. เขียน Docstring ให้ครบถ้วน
```python
def calculate_compound_interest(principal, rate, time, compounds_per_year=1):
    """คำนวณดอกเบี้ยทบต้น
    
    Args:
        principal (float): เงินต้น
        rate (float): อัตราดอกเบี้ย (เป็นทศนิยม, เช่น 0.05 สำหรับ 5%)
        time (float): ระยะเวลา (ปี)
        compounds_per_year (int): จำนวนครั้งที่ทบต้นต่อปี
    
    Returns:
        float: จำนวนเงินรวมดอกเบี้ย
    
    Example:
        >>> calculate_compound_interest(1000, 0.05, 2, 4)
        1104.94
    """
    amount = principal * (1 + rate/compounds_per_year) ** (compounds_per_year * time)
    return amount
```

### 3. ใช้ Type Hints
```python
from typing import List, Dict, Optional, Union, Callable

def process_numbers(numbers: List[int]) -> List[int]:
    """ประมวลผลเลขจำนวนเต็ม"""
    return [x * 2 for x in numbers]

def get_user_info(user_id: int) -> Optional[Dict[str, Union[str, int]]]:
    """ดึงข้อมูลผู้ใช้"""
    # implementation
    pass

def apply_operation(numbers: List[int], operation: Callable[[int], int]) -> List[int]:
    """ใช้ operation กับทุกตัวเลข"""
    return [operation(num) for num in numbers]
```

### 4. หลีกเลี่ยง Side Effects
```python
# ✅ ดี: Pure function
def add(a, b):
    """บวกเลขสองตัว"""
    return a + b

# ❌ ไม่ดี: มี side effect
counter = 0
def add_with_side_effect(a, b):
    """บวกเลขและเพิ่ม counter"""
    global counter
    counter += 1
    return a + b

# ✅ ดี: คืนค่าใหม่แทนการแก้ไข
def add_to_list(items, new_item):
    """เพิ่มรายการใหม่และคืน list ใหม่"""
    return items + [new_item]

# ❌ ไม่ดี: แก้ไข argument
def add_to_list_mutate(items, new_item):
    """เพิ่มรายการใหม่โดยแก้ไข list เดิม"""
    items.append(new_item)
    return items
```

---

## 📝 สรุป Function Patterns

| Pattern | วัตถุประสงค์ | ตัวอย่าง |
|---------|-------------|-----------|
| Pure Function | ไม่มี side effects | `def add(a, b): return a + b` |
| Higher-Order | รับ/คืน function | `def apply(func, x): return func(x)` |
| Closure | จับตัวแปรภายนอก | `def make_adder(n): return lambda x: x + n` |
| Decorator | ขยายฟังก์ชัน | `@timing_decorator` |
| Generator | สร้างค่าทีละตัว | `def count_up(): yield i` |
| Lambda | ฟังก์ชันสั้นๆ | `lambda x: x * 2` |

---

## 🎯 แบบฝึกหัด

### แบบฝึกหัด 1: Calculator
```python
# เขียนฟังก์ชัน calculator ที่รับ operation และ numbers
def calculator(operation, *numbers):
    pass
```

### แบบฝึกหัด 2: Data Processor
```python
# เขียนฟังก์ชันประมวลผลข้อมูลพร้อม validation
def process_user_data(data, validators=None):
    pass
```

### แบบฝึกหัด 3: Custom Decorator
```python
# เขียน decorator สำหรับ retry การทำงาน
def retry_decorator(max_retries=3, delay=1):
    pass
```

---

*สร้างเมื่อ: 4 มีนาคม 2026*  
*ปรับปรุงล่าสุด: 4 มีนาคม 2026*
