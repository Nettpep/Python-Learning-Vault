# Conditionals If Else - ทางเลือกของโลก

## 🔀 แนวคิดพื้นฐาน

Conditionals คือ **การควบคุมการไหลของโปรแกรม** โดยการตัดสินใจว่าจะทำอะไรต่อไปตามเงื่อนไขที่กำหนด

```python
# โครงสร้างพื้นฐาน
if condition:
    # ทำอะไรสักอย่างถ้า condition เป็น True
else:
    # ทำอย่างอื่นถ้า condition เป็น False
```

---

## 🎯 If Statement พื้นฐาน

### การใช้งาน If อย่างเดียว
```python
age = 18

if age >= 18:
    print("คุณเป็นผู้ใหญ่แล้ว")
    print("สามารถลงคะแนนได้")

# หลายคำสั่งใน if block
score = 85
if score >= 80:
    print("คะแนนดีมาก!")
    print("ได้เกรด A")
    print("ยินดีด้วย!")
```

### If-Else ครบถ้วน
```python
age = 16

if age >= 18:
    print("คุณเป็นผู้ใหญ่")
else:
    print("คุณยังเป็นเด็ก")

# การใช้กับตัวแปร
temperature = 25
if temperature > 30:
    weather = "ร้อน"
else:
    weather = "ไม่ร้อน"
print(f"วันนี้อากาศ{weather}")
```

### If-Elif-Else หลายทางเลือก
```python
score = 75

if score >= 90:
    grade = "A"
elif score >= 80:
    grade = "B"
elif score >= 70:
    grade = "C"
elif score >= 60:
    grade = "D"
else:
    grade = "F"

print(f"เกรดของคุณคือ {grade}")

# ตัวอย่างจริง
age = 25
if age < 13:
    group = "เด็ก"
elif age < 20:
    group = "วัยรุ่น"
elif age < 60:
    group = "ผู้ใหญ่"
else:
    group = "ผู้สูงอายุ"
print(f"คุณอยู่ในกลุ่ม{group}")
```

---

## 🔍 การตรวจสอบเงื่อนไข

### การเปรียบเทียบพื้นฐาน
```python
x = 10
y = 5

# การเปรียบเทียบตัวเลข
if x > y:
    print("x มากกว่า y")

if x == 10:
    print("x เท่ากับ 10")

if x != y:
    print("x ไม่เท่ากับ y")

# การเปรียบเทียบ string
name = "สมชาย"
if name == "สมชาย":
    print("ยินดีต้อนรับคุณสมชาย")

# การเปรียบเทียบ case-insensitive
input_name = "somchai"
if input_name.lower() == "somchai".lower():
    print("ชื่อตรงกัน")
```

### การเปรียบเทียบแบบ Chain
```python
age = 25

# แทนที่จะเขียน: age >= 18 and age <= 65
if 18 <= age <= 65:
    print("อายุที่เหมาะสมสำหรับทำงาน")

# การเปรียบเทียบหลายค่า
score = 85
if 80 <= score <= 100:
    print("คะแนนดีมาก")

# การเปรียบเทียบ string
text = "Hello"
if "Hello" <= text <= "Help":
    print("Text อยู่ในช่วงที่กำหนด")
```

### การตรวจสอบค่าว่าง
```python
# การตรวจสอบ None
value = None
if value is None:
    print("ค่าว่าง")

# การตรวจสอบ string ว่าง
text = ""
if not text:
    print("String ว่าง")

# การตรวจสอบ list ว่าง
items = []
if not items:
    print("List ว่าง")

# การตรวจสอบค่า 0
number = 0
if number == 0:
    print("ค่าศูนย์")

# การตรวจสอบแบบรวม
def is_empty(value):
    """ตรวจสอบว่าค่าว่างหรือไม่"""
    return value is None or value == "" or value == [] or value == {}
```

---

## 🎪 การใช้งานขั้นสูง

### Nested If (If ซ้อน If)
```python
age = 20
has_license = True

if age >= 18:
    if has_license:
        print("สามารถขับรถได้")
    else:
        print("ต้องไปทำใบขับขี่ก่อน")
else:
    print("อายุไม่พอขับรถ")

# ตัวอย่างซับซ้อน
score = 85
attendance = 90
project_score = 80

if score >= 70:
    if attendance >= 80:
        if project_score >= 70:
            print("ผ่านวิชานี้")
        else:
            print("ไม่ผ่าน - โปรเจคต์ไม่ผ่าน")
    else:
        print("ไม่ผ่าน - ขาดเรียนเยอะเกินไป")
else:
    print("ไม่ผ่าน - คะแนนไม่พอ")
```

### Ternary Operator (Conditional Expression)
```python
# แบบปกติ
age = 18
if age >= 18:
    status = "ผู้ใหญ่"
else:
    status = "เด็ก"

# แบบ ternary (กระชับ)
status = "ผู้ใหญ่" if age >= 18 else "เด็ก"

# การใช้กับฟังก์ชัน
def get_discount(price, member):
    return price * 0.9 if member else price

# ซับซ้อนขึ้น
score = 75
grade = "A" if score >= 90 else "B" if score >= 80 else "C"
print(grade)  # C
```

### การตรวจสอบหลายเงื่อนไขพร้อมกัน
```python
username = "admin"
password = "123456"
is_active = True

# การใช้ and
if username == "admin" and password == "123456":
    print("Login สำเร็จ")

# การใช้ or
day = "Saturday"
if day == "Saturday" or day == "Sunday":
    print("วันหยุด")

# การใช้ not
is_raining = False
if not is_raining:
    print("อากาศดี")

# การผสม
age = 25
has_license = True
if age >= 18 and has_license:
    print("ขับรถได้")
```

---

## 🎨 การใช้งานกับ Data Types

### กับ Numbers
```python
# ตรวจสอบชนิดตัวเลข
value = 42

if isinstance(value, int):
    print("เป็นจำนวนเต็ม")
elif isinstance(value, float):
    print("เป็นจำนวนทศนิยม")

# ตรวจสอบช่วง
temperature = 25
if temperature < 0:
    print("หนาวเย็น")
elif temperature < 20:
    print("อากาศเย็น")
elif temperature < 30:
    print("อากาศดี")
else:
    print("ร้อน")
```

### กับ Strings
```python
text = "Hello World"

# ตรวจสอบความยาว
if len(text) > 10:
    print("ข้อความยาว")

# ตรวจสอบคำ
if "World" in text:
    print("พบคำว่า World")

# ตรวจสอบตัวอักษร
if text.startswith("Hello"):
    print("ขึ้นต้นด้วย Hello")

if text.endswith("World"):
    print("ลงท้ายด้วย World")

# ตรวจสอบกรณี
if text.isupper():
    print("ตัวพิมพ์ใหญ่ทั้งหมด")
elif text.islower():
    print("ตัวพิมพ์เล็กทั้งหมด")
```

### กับ Collections
```python
# List
numbers = [1, 2, 3, 4, 5]

if len(numbers) > 0:
    print("List ไม่ว่าง")

if 3 in numbers:
    print("มีเลข 3")

# Dictionary
person = {"name": "สมชาย", "age": 25}

if "name" in person:
    print("มีข้อมูลชื่อ")

if person.get("age", 0) >= 18:
    print("เป็นผู้ใหญ่")

# Set
unique_numbers = {1, 2, 3, 4, 5}
if 3 in unique_numbers:
    print("มีเลข 3")
```

---

## 🛠️ การใช้งานจริง

### การตรวจสอบ Input
```python
def validate_age(age_input):
    """ตรวจสอบค่าอายุที่รับมา"""
    try:
        age = int(age_input)
        if age < 0:
            return "อายุไม่สามารถเป็นลบได้"
        elif age > 150:
            return "อายุไม่น่าจะเกิน 150"
        else:
            return "อายุถูกต้อง"
    except ValueError:
        return "กรุณาใส่ตัวเลขเท่านั้น"

# การใช้งาน
user_input = input("กรุณาใส่อายุ: ")
result = validate_age(user_input)
print(result)
```

### การจัดการสถานะ
```python
class OrderStatus:
    def __init__(self):
        self.status = "pending"
    
    def update_status(self, payment_status, stock_status):
        """อัปเดตสถานะคำสั่งซื้อ"""
        if self.status == "pending":
            if payment_status == "paid" and stock_status == "available":
                self.status = "processing"
            elif payment_status == "unpaid":
                self.status = "awaiting_payment"
            elif stock_status == "unavailable":
                self.status = "out_of_stock"
        
        elif self.status == "processing":
            if payment_status == "paid" and stock_status == "shipped":
                self.status = "completed"
        
        return self.status
```

### การตัดสินใจทางธุรกิจ
```python
def calculate_discount(price, quantity, customer_type):
    """คำนวณส่วนลด"""
    discount = 0
    
    # ส่วนลดตามปริมาณ
    if quantity >= 100:
        discount += 0.20
    elif quantity >= 50:
        discount += 0.15
    elif quantity >= 10:
        discount += 0.10
    
    # ส่วนลดตามประเภทลูกค้า
    if customer_type == "vip":
        discount += 0.05
    elif customer_type == "member":
        discount += 0.02
    
    # ส่วนลดสูงสุด
    if discount > 0.30:
        discount = 0.30
    
    return price * (1 - discount)

# การใช้งาน
final_price = calculate_discount(1000, 75, "vip")
print(f"ราคาสุดท้าย: {final_price:.2f} บาท")
```

---

## 🎯 การเพิ่มประสิทธิภาพ

### การลด Nested If
```python
# ❌ ไม่ดี: Nested ลึกเกินไป
if user is not None:
    if user.is_active:
        if user.has_permission:
            if user.role == "admin":
                print("Admin access granted")

# ✅ ดี: ใช้ Guard Clauses
if user is None:
    return "No user"
if not user.is_active:
    return "User inactive"
if not user.has_permission:
    return "No permission"
if user.role != "admin":
    return "Not admin"
print("Admin access granted")
```

### การใช้ Dictionary แทน If-Else
```python
# ❌ ไม่ดี: If-Else ยาวๆ
def get_grade_description(grade):
    if grade == "A":
        return "ดีเยี่ยม"
    elif grade == "B":
        return "ดี"
    elif grade == "C":
        return "พอใช้"
    elif grade == "D":
        return "ต้องปรับปรุง"
    else:
        return "ไม่ผ่าน"

# ✅ ดี: ใช้ Dictionary
grade_descriptions = {
    "A": "ดีเยี่ยม",
    "B": "ดี",
    "C": "พอใช้",
    "D": "ต้องปรับปรุง"
}

def get_grade_description(grade):
    return grade_descriptions.get(grade, "ไม่ผ่าน")
```

### การใช้ Any/All
```python
numbers = [1, 2, 3, 4, 5]

# ❌ ไม่ดี: ใช้ loop
has_positive = False
for num in numbers:
    if num > 0:
        has_positive = True
        break

# ✅ ดี: ใช้ any()
has_positive = any(num > 0 for num in numbers)

# ตรวจสอบว่าทุกตัวเป็นบวก
all_positive = all(num > 0 for num in numbers)

# การใช้กับเงื่อนไขซับซ้อน
if any(num > 10 for num in numbers) and all(num > 0 for num in numbers):
    print("มีตัวที่มากกว่า 10 และทุกตัวเป็นบวก")
```

---

## 🛡️ การจัดการ Error

### การตรวจสอบก่อนใช้
```python
def divide_numbers(a, b):
    """หารตัวเลขพร้อมตรวจสอบ"""
    if b == 0:
        return "Error: Cannot divide by zero"
    return a / b

# การใช้งาน
result = divide_numbers(10, 0)
if isinstance(result, str) and result.startswith("Error"):
    print(result)
else:
    print(f"Result: {result}")
```

### การใช้ Try-Except กับ Conditionals
```python
def get_user_age():
    """รับอายุผู้ใช้พร้อมตรวจสอบ"""
    try:
        age_input = input("กรุณาใส่อายุ: ")
        age = int(age_input)
        
        if age < 0:
            return "อายุไม่สามารถเป็นลบได้"
        elif age > 150:
            return "อายุไม่น่าจะเกิน 150"
        else:
            return f"อายุของคุณคือ {age} ปี"
            
    except ValueError:
        return "กรุณาใส่ตัวเลขเท่านั้น"
    except KeyboardInterrupt:
        return "ยกเลิกการทำงาน"
```

---

## 💡 Best Practices

### 1. ตั้งชื่อเงื่อนไขที่ชัดเจน
```python
# ✅ ดี
is_adult = age >= 18
has_permission = user.role == "admin"
is_valid_input = len(username) > 3

if is_adult and has_permission:
    print("Access granted")

# ❌ ไม่ดี
if age >= 18 and user.role == "admin":
    print("Access granted")
```

### 2. ใช้ Guard Clauses
```python
# ✅ ดี
def process_data(data):
    if data is None:
        return None
    if len(data) == 0:
        return []
    # ทำงานหลัก
    return processed_data

# ❌ ไม่ดี
def process_data(data):
    if data is not None:
        if len(data) > 0:
            # ทำงานหลัก
            return processed_data
```

### 3. หลีกเลี่ยง Nested ลึกเกินไป
```python
# ✅ ดี: แยกเป็นฟังก์ชัน
def is_valid_user(user):
    return user is not None and user.is_active

def has_permission(user, action):
    return action in user.permissions

if is_valid_user(user) and has_permission(user, "read"):
    # ทำงาน
    pass
```

### 4. ใช้ Type Hints
```python
from typing import Optional

def check_age(age: int) -> str:
    """ตรวจสอบอายุและคืนคำอธิบาย"""
    if age < 0:
        return "อายุไม่ถูกต้อง"
    elif age < 18:
        return "เด็ก"
    elif age < 60:
        return "ผู้ใหญ่"
    else:
        return "ผู้สูงอายุ"
```

---

## 📝 สรุปเทคนิค

| เทคนิค | วัตถุประสงค์ | ตัวอย่าง |
|---------|-------------|-----------|
| Guard Clauses | ลด nested | `if not condition: return` |
| Ternary | กระชับ | `result = "yes" if condition else "no"` |
| Dictionary | แทน if-else ยาว | `mapping.get(key, default)` |
| Any/All | ตรวจสอบ collection | `any(x > 0 for x in items)` |
| Early Return | ออกจากฟังก์ชันเร็ว | `if error: return None` |

---

## 🎯 แบบฝึกหัด

### แบบฝึกหัด 1: ตรวจสอบคะแนน
```python
# เขียนฟังก์ชันตรวจสอบเกรด
def check_grade(score):
    # 90+ = A, 80+ = B, 70+ = C, 60+ = D, ต่ำกว่า = F
    pass
```

### แบบฝึกหัด 2: ตรวจสอบวันหยุด
```python
# เขียนฟังก์ชันตรวจสอบวันหยุด
def is_weekend(day):
    # "Saturday", "Sunday" = True, อื่น = False
    pass
```

### แบบฝึกหัด 3: คำนวณราคา
```python
# เขียนฟังก์ชันคำนวณราคาพร้อมส่วนลด
def calculate_total(price, quantity, is_member):
    # 10+ ชิ้น = ลด 10%, member = ลดอีก 5%
    pass
```

---

*สร้างเมื่อ: 4 มีนาคม 2026*  
*ปรับปรุงล่าสุด: 4 มีนาคม 2026*
