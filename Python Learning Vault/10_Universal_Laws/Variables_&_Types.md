# Variables & Types - การนิยามตัวตนของข้อมูล

## 🏷️ แนวคิดพื้นฐาน

### Variable คืออะไร?
Variable คือ **ชื่อที่ใช้อ้างอิงถึงข้อมูล** ในหน่วยความจำ คล้ายกับป้ายชื่อที่ติดไว้บนกล่องข้อมูล

```python
# การสร้าง variable และกำหนดค่า
name = "สมชาย"
age = 25
height = 175.5
is_student = True
```

### การตั้งชื่อ Variable
```python
# ✅ ถูกต้อง
user_name = "John"
total_price = 150.50
is_active = True
_private_var = "secret"

# ❌ ผิดกฎ
2name = "Invalid"      # ขึ้นต้นด้วยตัวเลข
user-name = "Invalid"  # มีตัวอักษรพิเศษ
class = "Invalid"      # ใช้คำสงวน
```

**กฎการตั้งชื่อ:**
- ขึ้นต้นด้วยตัวอักษรหรือ underscore (_)
- ประกอบด้วยตัวอักษร, ตัวเลข, underscore
- ห้ามใช้คำสงวน (reserved words)
- Case-sensitive (name ≠ Name)

---

## 🎯 ชนิดข้อมูลพื้นฐาน (Basic Data Types)

### 1. Numbers (ตัวเลข)

#### Integers (จำนวนเต็ม)
```python
age = 25
temperature = -10
big_number = 1_000_000  # ใช้ underscore ช่วยอ่านง่าย

print(type(age))  # <class 'int'>
```

#### Floats (จำนวนทศนิยม)
```python
price = 19.99
pi = 3.14159
scientific = 1.5e-3  # 0.0015

print(type(price))  # <class 'float'>
```

#### Complex Numbers (จำนวนเชิงซ้อน)
```python
complex_num = 3 + 4j
print(complex_num.real)  # 3.0
print(complex_num.imag)  # 4.0
```

### 2. Strings (ข้อความ)

```python
# การสร้าง string
name = 'สมชาย'
greeting = "สวัสดี"
multiline = """บรรทัดที่ 1
บรรทัดที่ 2
บรรทัดที่ 3"""

# String operations
full_name = name + " ใจดี"
length = len(name)
upper_name = name.upper()

print(f"ชื่อ: {name}, ความยาว: {length}")
```

### 3. Booleans (ตรรกะ)
```python
is_active = True
is_complete = False
has_permission = True

# Boolean operations
print(not is_active)        # False
print(is_active and True)   # True
print(is_active or False)   # True
```

### 4. None (ค่าว่าง)
```python
result = None
user_input = None

# การตรวจสอบ None
if result is None:
    print("ยังไม่มีผลลัพธ์")
```

---

## 🔄 การแปลงชนิดข้อมูล (Type Conversion)

### Implicit Conversion (แปลงอัตโนมัติ)
```python
# Python แปลง int เป็น float อัตโนมัติ
result = 5 + 3.14  # 8.14 (float)
print(type(result))  # <class 'float'>
```

### Explicit Conversion (แปลงด้วยมือ)
```python
# String → Number
age_str = "25"
age_int = int(age_str)
price_float = float("19.99")

# Number → String
score = 95
score_str = str(score)

# Boolean → Number
print(int(True))   # 1
print(int(False))  # 0
print(float(True)) # 1.0

# การแปลงที่อาจเกิด error
try:
    invalid = int("abc")  # ValueError
except ValueError as e:
    print(f"Error: {e}")
```

---

## 🎨 การจัดรูปแบบ String (String Formatting)

### f-string (Python 3.6+)
```python
name = "สมชาย"
age = 25
height = 175.5

message = f"ชื่อ {name} อายุ {age} ปี ส่วนสูง {height:.1f} ซม."
print(message)
# ชื่อ สมชาย อายุ 25 ปี ส่วนสูง 175.5 ซม.
```

### .format() method
```python
template = "สวัสดีครับคุณ {0} อายุ {1} ปี"
filled = template.format("วิชัย", 30)
print(filled)
```

### % formatting (เก่าแต่ยังใช้ได้)
```python
name = "มานี"
age = 28
print("ชื่อ %s อายุ %d ปี" % (name, age))
```

---

## 🔍 การตรวจสอบชนิดข้อมูล

### type() function
```python
value = 42
print(type(value))          # <class 'int'>
print(type(value) == int)  # True
```

### isinstance() function (แนะนำ)
```python
value = "Hello"
print(isinstance(value, str))     # True
print(isinstance(value, (int, float)))  # False

# ตรวจสอบหลายชนิดพร้อมกัน
def process_number(num):
    if isinstance(num, (int, float)):
        return num * 2
    else:
        return "ต้องเป็นตัวเลขเท่านั้น"
```

---

## 📦 Collections (ชนิดข้อมูลแบบกลุ่ม)

### Lists (รายการ)
```python
fruits = ["แอปเปิ้ล", "กล้วย", "ส้ม"]
numbers = [1, 2, 3, 4, 5]
mixed = [1, "Hello", True, 3.14]

print(len(fruits))        # 3
print(fruits[0])          # แอปเปิ้ล
print(fruits[-1])         # ส้ม
```

### Tuples (ทูเพิล - ไม่เปลี่ยนแปลงได้)
```python
coordinates = (10, 20)
rgb_color = (255, 0, 0)
single_item = (42,)  # ต้องมี comma

print(coordinates[0])  # 10
# coordinates[0] = 15  # TypeError!
```

### Dictionaries (พจนานุกรม)
```python
person = {
    "name": "สมชาย",
    "age": 25,
    "city": "กรุงเทพฯ"
}

print(person["name"])        # สมชาย
print(person.get("age"))     # 25
print(person.get("salary", 0)) # 0 (default value)
```

### Sets (เซ็ต - ไม่ซ้ำกัน)
```python
unique_numbers = {1, 2, 3, 2, 1}
print(unique_numbers)  # {1, 2, 3}

# การดำเนินการเซ็ต
set1 = {1, 2, 3}
set2 = {3, 4, 5}
print(set1 & set2)  # {3} (intersection)
print(set1 | set2)  # {1, 2, 3, 4, 5} (union)
```

---

## 🎯 การใช้งานขั้นสูง

### Dynamic Typing
```python
# Python อนุญาตให้เปลี่ยนชนิดข้อมูลได้
x = 10          # int
x = "Hello"     # str
x = [1, 2, 3]   # list

# แต่ไม่ควรทำเพราะสร้างความสับสน
```

### Type Hints (Python 3.5+)
```python
from typing import List, Dict, Optional

def calculate_total(price: float, quantity: int) -> float:
    """คำนวณราคารวม"""
    return price * quantity

def get_user_info(user_id: int) -> Optional[Dict[str, str]]:
    """ดึงข้อมูลผู้ใช้"""
    # implementation
    pass

# การใช้งาน
result: float = calculate_total(19.99, 3)
```

### Constants (ค่าคงที่)
```python
# Python ไม่มี true constants แต่มี convention
PI = 3.14159
MAX_USERS = 1000
DEFAULT_TIMEOUT = 30

# ใช้ตัวพิมพ์ใหญ่ทั้งหมด
```

---

## 🛠️ การ Debug Variables

### การตรวจสอบค่า
```python
# พื้นฐาน
print(f"x = {x}, type = {type(x)}")

# ใช้ dir() ดู methods/attributes
print(dir(x))

# ใช้ help() ดู documentation
help(str)
```

### การตรวจสอบค่าว่าง
```python
# หลายวิธีในการตรวจสอบ
my_var = ""

if not my_var:           # True สำหรับ "", 0, None, False, []
    print("ว่างหรือเป็นเท็จ")

if my_var is None:       # เฉพาะ None เท่านั้น
    print("เป็น None")

if len(my_var) == 0:     # เฉพาะ collection ที่มี len()
    print("ว่างเปล่า")
```

---

## 💡 Best Practices

### 1. ตั้งชื่อที่มีความหมาย
```python
# ✅ ดี
user_age = 25
total_price = 150.50
is_authenticated = True

# ❌ ไม่ดี
x = 25
tp = 150.50
auth = True
```

### 2. ใช้ Type Hints
```python
def process_data(data: List[Dict[str, Any]]) -> Dict[str, int]:
    """ประมวลผลข้อมูล"""
    pass
```

### 3. ตรวจสอบข้อมูลก่อนใช้
```python
def divide_numbers(a: float, b: float) -> Optional[float]:
    """หารตัวเลขพร้อมตรวจสอบ"""
    if b == 0:
        return None
    return a / b
```

### 4. ใช้ constants แทน magic numbers
```python
# ✅ ดี
DISCOUNT_RATE = 0.10
MAX_ATTEMPTS = 3

# ❌ ไม่ดี
price = price * 0.10
for i in range(3):
```

---

## 📝 สรุป

| ชนิดข้อมูล | ตัวอย่าง | การใช้งาน |
|-------------|----------|-------------|
| `int` | `42`, `-10` | จำนวนเต็ม |
| `float` | `3.14`, `2.5` | จำนวนทศนิยม |
| `str` | `"Hello"`, `'สวัสดี'` | ข้อความ |
| `bool` | `True`, `False` | ตรรกะ |
| `None` | `None` | ค่าว่าง |
| `list` | `[1, 2, 3]` | รายการ |
| `tuple` | `(1, 2)` | ทูเพิล |
| `dict` | `{"key": "value"}` | พจนานุกรม |
| `set` | `{1, 2, 3}` | เซ็ต |

---

*สร้างเมื่อ: 4 มีนาคม 2026*  
*ปรับปรุงล่าสุด: 4 มีนาคม 2026*
