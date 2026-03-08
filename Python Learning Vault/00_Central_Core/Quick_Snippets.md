# Quick Snippets - โค้ดที่ต้องใช้บ่อย

## 🚀 โค้ดเริ่มต้น Python

### Basic Structure
*ใช้เป็นโครงหลักของสคริปต์ Python (ถ้ารันโดยตรง จะเรียก main())*
```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-

def main():
    print("Hello, World!")

if __name__ == "__main__":
    main()
```

### Import ที่ใช้บ่อย
*คัดลอกโมดูลที่ใช้ประจำ (os, sys, json, pathlib ฯลฯ)*
```python
import os
import sys
import json
import datetime
from pathlib import Path
```

---

## 📁 การทำงานกับไฟล์

### อ่านไฟล์ทั้งหมด
*อ่านทั้งไฟล์เป็นสตริงเดียว — เหมาะกับไฟล์ขนาดไม่ใหญ่*
```python
with open('filename.txt', 'r', encoding='utf-8') as f:
    content = f.read()
```

### เขียนไฟล์
*เขียนทับไฟล์เดิม (สร้างใหม่ถ้าไม่มี) — ระบุ encoding='utf-8' สำหรับภาษาไทย*
```python
with open('filename.txt', 'w', encoding='utf-8') as f:
    f.write("Hello, World!")
```

### อ่านไฟล์ทีละบรรทัด
*ประหยัดหน่วยความจำ เหมาะกับไฟล์ใหญ่ — ใช้ .strip() ตัด \\n ต่อบรรทัด*
```python
with open('filename.txt', 'r', encoding='utf-8') as f:
    for line in f:
        print(line.strip())
```

---

## 🌐 การทำงานกับ JSON

### อ่าน JSON
*โหลดไฟล์ .json เป็น dict/list ใน Python*
```python
import json

with open('data.json', 'r', encoding='utf-8') as f:
    data = json.load(f)
```

### เขียน JSON
*บันทึก dict/list เป็นไฟล์ .json (indent, ensure_ascii=False สำหรับภาษาไทย)*
```python
import json

data = {"name": "John", "age": 30}
with open('data.json', 'w', encoding='utf-8') as f:
    json.dump(data, f, indent=2, ensure_ascii=False)
```

---

## 🔄 การทำงานกับ List

### List Comprehension
*สร้าง list ใหม่จาก list เดิมในหนึ่งบรรทัด (กรองหรือแปลงค่า)*
```python
# สร้าง list ใหม่จาก list เดิม
numbers = [1, 2, 3, 4, 5]
squared = [x**2 for x in numbers]

# กรองข้อมูล
even_numbers = [x for x in numbers if x % 2 == 0]
```

### การวนลูปพร้อม index
*ได้ทั้ง index และค่าในแต่ละรอบ (ไม่ต้องใช้ range(len()))*
```python
for i, item in enumerate(my_list):
    print(f"Index {i}: {item}")
```

---

## 📊 การทำงานกับ Dictionary

### สร้าง Dictionary
*เก็บคู่ key–value (ชื่อฟิลด์กับค่า)*
```python
person = {
    "name": "John",
    "age": 30,
    "city": "Bangkok"
}
```

### วนลูป Dictionary
*ไล่ทุก key และ value พร้อมกัน*
```python
for key, value in person.items():
    print(f"{key}: {value}")
```

### ตรวจสอบว่ามี key หรือไม่
*ใช้ in กับ dict จะเช็ค key (ไม่เช็ค value)*
```python
if "name" in person:
    print("Name exists!")
```

---

## 🎯 ฟังก์ชันที่ใช้บ่อย

### ฟังก์ชันพื้นฐาน
```python
def calculate_sum(a, b):
    """คำนวณผลรวมของตัวเลขสองตัว"""
    return a + b

def process_data(data, callback=None):
    """ประมวลผลข้อมูลพร้อม callback function"""
    result = []
    for item in data:
        processed = item.upper()
        result.append(processed)
    
    if callback:
        callback(result)
    
    return result
```

### Lambda Function
```python
# ฟังก์ชันสั้นๆ
square = lambda x: x**2
add = lambda x, y: x + y

# ใช้กับ sorted, map, filter
numbers = [1, 2, 3, 4, 5]
squared = list(map(lambda x: x**2, numbers))
even = list(filter(lambda x: x % 2 == 0, numbers))
```

---

## 🛡️ การจัดการ Error

### Try-Except พื้นฐาน
```python
try:
    result = 10 / 0
except ZeroDivisionError:
    print("Cannot divide by zero!")
except Exception as e:
    print(f"Error occurred: {e}")
finally:
    print("This always runs")
```

### การสร้าง Custom Exception
```python
class CustomError(Exception):
    pass

def validate_age(age):
    if age < 0:
        raise CustomError("Age cannot be negative!")
    return age
```

---

## 📅 การทำงานกับวันที่และเวลา

### วันที่ปัจจุบัน
```python
import datetime

now = datetime.datetime.now()
today = datetime.date.today()
```

### จัดรูปแบบวันที่
```python
formatted = now.strftime("%Y-%m-%d %H:%M:%S")
thai_format = now.strftime("%d/%m/%Y")
```

### แปลง string เป็น datetime
```python
date_string = "2023-12-25"
date_obj = datetime.datetime.strptime(date_string, "%Y-%m-%d")
```

---

## 🌍 การทำงานกับ Path

### ใช้ pathlib (แนะนำ)
```python
from pathlib import Path

# สร้าง path
file_path = Path("data") / "input.txt"

# ตรวจสอบว่ามีไฟล์หรือไม่
if file_path.exists():
    print("File exists!")

# อ่านเขียนไฟล์
content = file_path.read_text(encoding='utf-8')
file_path.write_text("Hello", encoding='utf-8')
```

### การสร้างโฟลเดอร์
```python
import os

# สร้างโฟลเดอร์ (ถ้ายังไม่มี)
os.makedirs("data/output", exist_ok=True)
```

---

## 🎨 การทำงานกับ String

### String Formatting
```python
name = "John"
age = 30

# f-string (แนะนำ)
message = f"My name is {name} and I'm {age} years old"

# .format()
message = "My name is {} and I'm {} years old".format(name, age)
```

### การจัดการ string
```python
text = "  Hello, World!  "

# ตัดช่องว่าง
clean_text = text.strip()

# แบ่ง string
words = text.split(",")

# แทนที่ข้อความ
new_text = text.replace("World", "Python")

# ตรวจสอบว่ามีข้อความหรือไม่
if "Python" in new_text:
    print("Found Python!")
```

---

## 📝 การ Debug

### Print Debug
```python
# พื้นฐาน
print(f"Variable x = {x}")
print(f"Type of x: {type(x)}")

# ดูข้อมูลใน list/dict
import pprint
pprint.pprint(my_dict)
```

### Logging
```python
import logging

logging.basicConfig(level=logging.DEBUG)
logger = logging.getLogger(__name__)

logger.debug("Debug message")
logger.info("Info message")
logger.warning("Warning message")
logger.error("Error message")
```

---

## 💡 Tips & Tricks

### การตรวจสอบ None
```python
if variable is not None:
    print("Variable has value")
```

### การสร้าง default dictionary
```python
from collections import defaultdict

my_dict = defaultdict(list)
my_dict['key'].append('value')
```

### การนับจำนวนใน list
```python
from collections import Counter

word_count = Counter(['apple', 'banana', 'apple', 'orange'])
```

---

*อัปเดตล่าสุด: 4 มีนาคม 2026*  
*เพิ่มเติม snippets ที่ใช้บ่อยๆ ได้เสมอ!*
