# Operators - กฎการคำนวณและเปรียบเทียบ

## 🔢 แนวคิดพื้นฐาน

Operators คือ **สัญลักษณ์พิเศษ** ที่ใช้ดำเนินการกับตัวแปรและค่า (operands) เพื่อสร้างผลลัพธ์ใหม่

```python
# ตัวอย่างพื้นฐาน
result = 5 + 3  # + เป็น operator, 5 และ 3 เป็น operands
is_equal = (10 == 10)  # == เป็น operator
```

---

## 🧮 Arithmetic Operators (ตัวดำเนินการคณิตศาสตร์)

### ตัวดำเนินการพื้นฐาน
```python
# การบวก (+)
result = 10 + 5          # 15
text = "Hello" + " World"  # "Hello World"

# การลบ (-)
result = 10 - 3          # 7

# การคูณ (*)
result = 6 * 7           # 42
text = "Hi" * 3          # "HiHiHi"

# การหาร (/)
result = 10 / 3          # 3.3333333333333335 (float)

# การหารเศษ (%)
result = 10 % 3          # 1

# การยกกำลัง (**)
result = 2 ** 3          # 8
result = 9 ** 0.5        # 3.0

# การหารปัดเศษ (//)
result = 10 // 3         # 3
result = -10 // 3        # -4 (ปัดเข้าหาค่าติดลบ)
```

### ลำดับความสำคัญ (PEMDAS)
```python
# Python ทำตามลำดับ: Parentheses, Exponents, Multiplication/Division, Addition/Subtraction
result = 2 + 3 * 4       # 14 (ไม่ใช่ 20)
result = (2 + 3) * 4     # 20
result = 2 ** 3 * 4      # 32
result = 2 ** (3 * 4)    # 4096
```

---

## 🔄 Assignment Operators (ตัวดำเนินการกำหนดค่า)

### การกำหนดค่าพื้นฐาน
```python
# กำหนดค่าธรรมดา
x = 10

# Multiple assignment
a, b, c = 1, 2, 3
name, age = "สมชาย", 25

# Swap values (Python magic!)
x, y = y, x

# Chain assignment
x = y = z = 0
```

### Compound Assignment
```python
x = 10

# x = x + 5
x += 5          # 15

# x = x - 3
x -= 3          # 12

# x = x * 2
x *= 2          # 24

# x = x / 4
x /= 4          # 6.0

# x = x // 2
x //= 2         # 3.0

# x = x % 2
x %= 2          # 1.0

# x = x ** 3
x **= 3         # 1.0
```

### Walrus Operator (:=) - Python 3.8+
```python
# กำหนดค่าและตรวจสอบในบรรทัดเดียว
if (n := len(my_list)) > 5:
    print(f"List too long: {n} items")

# ใน list comprehension
squares = [y**2 for x in range(10) if (y := x**2) > 25]
```

---

## 🔍 Comparison Operators (ตัวดำเนินการเปรียบเทียบ)

### การเปรียบเทียบค่า
```python
x = 10
y = 5

# เท่ากับ (==)
print(x == 10)     # True
print(x == y)      # False

# ไม่เท่ากับ (!=)
print(x != y)      # True

# มากกว่า (>)
print(x > y)       # True

# น้อยกว่า (<)
print(y < x)       # True

# มากกว่าหรือเท่ากับ (>=)
print(x >= 10)     # True

# น้อยกว่าหรือเท่ากับ (<=)
print(y <= 5)      # True
```

### การเปรียบเทียบแบบ Chain
```python
age = 25

# แทนที่จะเขียน: age >= 18 and age <= 65
if 18 <= age <= 65:
    print("Working age")

# แทนที่จะเขียน: x < y and y < z
if x < y < z:
    print("x < y < z")
```

### การเปรียบเทียบ Objects
```python
# == เปรียบเทียบค่า
list1 = [1, 2, 3]
list2 = [1, 2, 3]
print(list1 == list2)  # True

# is เปรียบเทียบ identity (memory address)
print(list1 is list2)  # False

# กรณีพิเศษ: small integers caching
a = 256
b = 256
print(a is b)  # True (cached)

c = 257
d = 257
print(c is d)  # False (ไม่ cached)
```

---

## 🎯 Logical Operators (ตัวดำเนินการตรรกะ)

### AND, OR, NOT
```python
x = True
y = False

# AND (และ)
print(x and y)     # False
print(True and True)   # True
print(True and False)  # False

# OR (หรือ)
print(x or y)      # True
print(True or False)   # True
print(False or False)  # False

# NOT (ไม่)
print(not x)        # False
print(not False)    # True
```

### Short-circuit Evaluation
```python
# AND: ถ้าตัวแรกเป็น False จะไม่ตรวจตัวต่อไป
def check():
    print("Checking...")
    return True

False and check()  # ไม่ print "Checking..."

# OR: ถ้าตัวแรกเป็น True จะไม่ตรวจตัวต่อไป
True or check()    # ไม่ print "Checking..."
```

### การใช้งานจริง
```python
# ตรวจสอบค่าว่าง
name = ""
age = 25

if name and age:
    print("มีชื่อและอายุ")
else:
    print("ขาดข้อมูลบางอย่าง")

# ค่า default
username = input("Username: ") or "guest"
print(f"Hello, {username}")
```

---

## 🔧 Bitwise Operators (ตัวดำเนินการระดับบิต)

### ตัวดำเนินการพื้นฐาน
```python
a = 5   # 0101
b = 3   # 0011

# AND (&)
result = a & b    # 0001 (1)

# OR (|)
result = a | b    # 0111 (7)

# XOR (^)
result = a ^ b    # 0110 (6)

# NOT (~)
result = ~a       # 11111111111111111111111111111010 (-6)

# Left Shift (<<)
result = a << 1   # 1010 (10)

# Right Shift (>>)
result = a >> 1   # 0010 (2)
```

### การใช้งานจริง
```python
# ตรวจสอบว่าเลขคู่หรือไม่
def is_even(n):
    return (n & 1) == 0

# สลับค่า
def toggle_bit(n, position):
    return n ^ (1 << position)

# ตรวจสอบ bit
def check_bit(n, position):
    return (n & (1 << position)) != 0
```

---

## 🎪 Identity Operators (ตัวดำเนินการตรวจสอบตัวตน)

### is และ is not
```python
x = [1, 2, 3]
y = [1, 2, 3]
z = x

# is: ตรวจสอบว่าเป็น object เดียวกัน
print(x is y)      # False
print(x is z)      # True
print(x is not y)  # True

# การใช้กับ None (แนะนำ)
value = None
if value is None:
    print("Value is None")

# แทนที่จะใช้ == None
if value == None:  # ใช้ได้แต่ไม่แนะนำ
    print("Value is None")
```

### การใช้กับ Singletons
```python
# Python มี singleton objects: None, True, False
a = None
b = None
print(a is b)  # True

# การตรวจสอบ boolean
flag = True
if flag is True:  # ดีกว่า flag == True
    print("Flag is true")
```

---

## 🏷️ Membership Operators (ตัวดำเนินการตรวจสอบสมาชิก)

### in และ not in
```python
# กับ list
fruits = ["แอปเปิ้ล", "กล้วย", "ส้ม"]
print("กล้วย" in fruits)        # True
print("มะม่วง" not in fruits)   # True

# กับ string
text = "Hello World"
print("World" in text)          # True
print("world" in text)          # False (case-sensitive)

# กับ dictionary
person = {"name": "สมชาย", "age": 25}
print("name" in person)         # True (ตรวจสอบ keys)
print("สมชาย" in person)       # False
print("สมชาย" in person.values()) # True

# กับ set
numbers = {1, 2, 3, 4, 5}
print(3 in numbers)             # True
print(6 not in numbers)         # True
```

### ประสิทธิภาพการค้นหา
```python
import time

# List: O(n) - ช้า
large_list = list(range(1000000))
start = time.time()
999999 in large_list
print(f"List search: {time.time() - start:.6f}s")

# Set: O(1) - เร็วมาก
large_set = set(range(1000000))
start = time.time()
999999 in large_set
print(f"Set search: {time.time() - start:.6f}s")
```

---

## 🎨 การใช้งานขั้นสูง

### Operator Overloading (ใน Classes)
```python
class Vector:
    def __init__(self, x, y):
        self.x = x
        self.y = y
    
    def __add__(self, other):
        return Vector(self.x + other.x, self.y + other.y)
    
    def __eq__(self, other):
        return self.x == other.x and self.y == other.y
    
    def __str__(self):
        return f"Vector({self.x}, {self.y})"

v1 = Vector(1, 2)
v2 = Vector(3, 4)
v3 = v1 + v2
print(v3)  # Vector(4, 6)
print(v1 == v2)  # False
```

### Ternary Operator
```python
# แบบปกติ
if age >= 18:
    status = "ผู้ใหญ่"
else:
    status = "เด็ก"

# แบบ ternary
status = "ผู้ใหญ่" if age >= 18 else "เด็ก"

# ซับซ้อนขึ้น
result = "A" if score >= 90 else "B" if score >= 80 else "C"
```

### Operator Precedence Table
| ลำดับ | Operator | คำอธิบาย |
|--------|----------|-----------|
| 1 | `**` | ยกกำลัง |
| 2 | `~ + -` | Bitwise NOT, Unary plus/minus |
| 3 | `* / % //` | คูณ, หาร, เศษ, หารปัดเศษ |
| 4 | `+ -` | บวก, ลบ |
| 5 | `<< >>` | Bitwise shift |
| 6 | `&` | Bitwise AND |
| 7 | `^` | Bitwise XOR |
| 8 | `|` | Bitwise OR |
| 9 | `== != > >= < <= is is not in not in` | เปรียบเทียบ, identity, membership |
| 10 | `not` | Logical NOT |
| 11 | `and` | Logical AND |
| 12 | `or` | Logical OR |

---

## 🛠️ การ Debug Operators

### การตรวจสอบผลลัพธ์
```python
# ใช้ parentheses ชัดเจน
result = (a + b) * (c - d)

# แยกการคำนวณ
step1 = a + b
step2 = c - d
result = step1 * step2

# ใช้ print debug
print(f"a = {a}, b = {b}, a + b = {a + b}")
```

### ข้อผิดพลาดที่พบบ่อย
```python
# ❌ ผิด: ใช้ = แทน ==
if x = 5:  # SyntaxError

# ❌ ผิด: ลืม precedence
result = 10 + 5 * 2  # 20 (ไม่ใช่ 30)

# ❌ ผิด: เปรียบเทียบ floating point
0.1 + 0.2 == 0.3  # False!

# ✅ ถูก: ใช้ tolerance
abs((0.1 + 0.2) - 0.3) < 1e-9  # True
```

---

## 💡 Best Practices

### 1. ใช้ parentheses เพื่อความชัดเจน
```python
# ✅ ดี
result = (a + b) * (c - d)

# ❌ สับสน
result = a + b * c - d
```

### 2. เลือก operator ที่เหมาะสม
```python
# ✅ ดี: ใช้ is กับ None
if value is None:
    pass

# ✅ ดี: ใช้ in กับ collection
if item in my_list:
    pass
```

### 3. ระวัง floating point comparison
```python
# ✅ ดี
import math
if math.isclose(a, b, rel_tol=1e-9):
    pass
```

### 4. ใช้ compound assignment เมื่อเหมาะสม
```python
# ✅ ดี
count += 1
total *= factor

# ❌ ไม่จำเป็น
count = count + 1
total = total * factor
```

---

## 📝 สรุป

| ประเภท | Operator | ตัวอย่าง |
|--------|----------|-----------|
| Arithmetic | `+ - * / % // **` | `5 + 3`, `10 // 3` |
| Assignment | `= += -= *= /= //= %= **=` | `x += 5` |
| Comparison | `== != > < >= <=` | `x == y` |
| Logical | `and or not` | `x and y` |
| Identity | `is is not` | `x is None` |
| Membership | `in not in` | `item in list` |
| Bitwise | `& | ^ << >> ~` | `a & b` |

---

*สร้างเมื่อ: 4 มีนาคม 2026*  
*ปรับปรุงล่าสุด: 4 มีนาคม 2026*
