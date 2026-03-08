# String Manipulation - การควบคุมตัวอักษรและข้อความ

> 📖 **ทบทวนสรุป/ตัวอย่าง:** [เกร็ดความรู้ สตริง สรุปหลักสำคัญ](../60_Learning_Walkthroughs/เกร็ดความรู้_สตริง_สรุปหลักสำคัญ.md) (Concatenation, in, Comparison, Methods) · [Parsing ดึงค่าหลังโคลอนเป็น float](../60_Learning_Walkthroughs/Parsing_ดึงค่าหลังโคลอน_เป็น_float.md) (find + slice + strip)

## 📝 แนวคิดพื้นฐาน

String คือ **ลำดับของตัวอักษร** (sequence of characters) ที่ใช้เก็บข้อความ ใน Python string เป็น immutable (ไม่เปลี่ยนแปลงได้)

```python
# การสร้าง string
text = "Hello, World!"
thai_text = "สวัสดี ประเทศไทย"
multiline = """บรรทัดที่ 1
บรรทัดที่ 2"""
```

---

## 📑 ในหน้านี้ (สารบัญ)

- [[#🎨 การสร้างและพื้นฐาน String]] · [[#🔄 การดำเนินการกับ String]] · [[#🔧 การจัดรูปแบบ String (String Formatting)]]
- [[#🛠️ การประมวลผล String Methods]] · [[#🎯 การจัดการ Unicode และภาษาไทย]] · [[#📊 การประมวลผลขั้นสูง]]
- [[#🎨 การสร้าง Text Templates]] · [[#🛠️ การ Debug String]] · [[#💡 Best Practices]] · [[#📝 สรุป String Methods ที่ใช้บ่อย]]

---

## 🎨 การสร้างและพื้นฐาน String

### การสร้าง String
```python
# Single quotes
s1 = 'Hello'

# Double quotes
s2 = "World"

# Triple quotes (multiline)
s3 = """Line 1
Line 2
Line 3"""

# Raw string (ไม่ escape)
path = r"C:\Users\name\file.txt"

# Unicode string (default in Python 3)
unicode_text = "สวัสดี 🌍"
```

### การ Escape Characters
```python
# Escape sequences
text = "Line 1\nLine 2"      # Newline
text = "Tab\tSeparated"      # Tab
text = "Quote: \"Hello\""    # Double quote
text = "Backslash: \\"       # Backslash
text = "Unicode: \u0E2A"    # Thai 'ส' (สวัสดี)
```

### การเข้าถึงตัวอักษร
```python
text = "Python Programming"

# Indexing (0-based)
print(text[0])        # 'P'
print(text[6])        # ' '

# Negative indexing
print(text[-1])       # 'g'
print(text[-2])       # 'n'

# Slicing
print(text[0:6])      # 'Python'
print(text[7:])       # 'Programming'
print(text[:6])       # 'Python'
print(text[::2])      # 'Pto rgamn' (every 2nd char)
print(text[::-1])     # 'gnimmargorP nohtyP' (reverse)
```

---

## 🔄 การดำเนินการกับ String

### การต่อ String (Concatenation)
```python
# การใช้ + operator
first = "Hello"
last = "World"
full = first + " " + last  # "Hello World"

# การใช้ f-string (แนะนำ)
name = "สมชาย"
age = 25
message = f"ชื่อ {name} อายุ {age} ปี"

# การใช้ .join() (สำหรับ list)
words = ["Hello", "World", "Python"]
sentence = " ".join(words)  # "Hello World Python"

# การใช้ .format()
template = "ชื่อ {} อายุ {} ปี"
filled = template.format("วิชัย", 30)
```

### การทำซ้ำ String
```python
# การใช้ * operator
border = "=" * 20  # '===================='
pattern = "AB" * 3  # 'ABABAB'

# ใช้กับ list comprehension
numbers = [1, 2, 3]
result = ["num" + str(n) for n in numbers]  # ['num1', 'num2', 'num3']
```

---

## 🔧 การจัดรูปแบบ String (String Formatting)

### f-strings (Python 3.6+)
```python
name = "สมชาย"
score = 85.5
count = 42

# พื้นฐาน
message = f"ชื่อ {name} คะแนน {score}"

# การจัดรูปแบบตัวเลข
price = 1234.5678
formatted = f"ราคา: {price:.2f} บาท"  # ราคา: 1234.57 บาท

# การจัดรูปแบบจำนวนเต็ม
number = 42
padded = f"เลข: {number:04d}"  # เลข: 0042

# การจัดรูปแบบวันที่
import datetime
today = datetime.datetime.now()
date_str = f"วันนี้: {today:%d/%m/%Y}"  # วันนี้: 04/03/2026

# การคำนวณใน f-string
x = 10
y = 20
calc = f"{x} + {y} = {x + y}"  # 10 + 20 = 30
```

### .format() Method
```python
# Positional arguments
template = "ชื่อ {0} อายุ {1} ปี"
filled = template.format("สมชาย", 25)

# Keyword arguments
template = "ชื่อ {name} อายุ {age} ปี"
filled = template.format(name="วิชัย", age=30)

# Mixed
template = "{0} {name} {1} {age}"
filled = template.format("สวัสดี", "ปี", name="คุณ", age=25)

# การจัดรูปแบบ
price = 1234.567
formatted = "ราคา: {0:.2f} บาท".format(price)
```

### % Formatting (เก่าแต่ยังใช้ได้)
```python
name = "สมชาย"
age = 25
price = 19.99

# พื้นฐาน
message = "ชื่อ %s อายุ %d ปี" % (name, age)

# การจัดรูปแบบ
price_str = "ราคา: %.2f บาท" % price

# Dictionary formatting
person = {"name": "วิชัย", "age": 30}
message = "ชื่อ %(name)s อายุ %(age)d ปี" % person
```

---

## 🛠️ การประมวลผล String Methods

### การแปลงกรณี (Case Conversion)
```python
text = "Hello World PYTHON"

# พิมพ์ใหญ่/เล็ก
print(text.upper())      # 'HELLO WORLD PYTHON'
print(text.lower())      # 'hello world python'
print(text.title())      # 'Hello World Python'
print(text.capitalize()) # 'Hello world python'

# สลับกรณี
print(text.swapcase())   # 'hELLO wORLD python'

# ตรวจสอบกรณี
print(text.isupper())    # False
print(text.islower())    # False
print(text.istitle())    # False
```

### การตัดช่องว่าง (Whitespace)
```python
text = "   Hello World   "

# ตัดช่องว่าง
print(text.strip())      # 'Hello World'
print(text.lstrip())     # 'Hello World   '
print(text.rstrip())     # '   Hello World'

# ตัดตัวอักษรที่กำหนด
text = "xxHello Worldxx"
print(text.strip('x'))   # 'Hello World'

# ตรวจสอบ whitespace
print(text.isspace())    # False
print("   ".isspace())   # True
```

### การค้นหาและตรวจสอบ
```python
text = "Hello Python World"

# ค้นหาตำแหน่ง
print(text.find("Python"))    # 6
print(text.find("Java"))      # -1 (ไม่พบ)
print(text.index("Python"))   # 6
# print(text.index("Java"))   # ValueError

# นับจำนวน
print(text.count("o"))        # 2
print(text.count("l"))        # 3

# ตรวจสอบการเริ่ม/จบ
print(text.startswith("Hello"))   # True
print(text.endswith("World"))     # True
print(text.startswith("Python"))  # False

# ตรวจสอบว่ามีอยู่หรือไม่
print("Python" in text)          # True
print("Java" in text)            # False
```

### การแทนที่และแยก
```python
text = "Hello World Hello Python"

# แทนที่
print(text.replace("Hello", "Hi"))      # 'Hi World Hi Python'
print(text.replace("Hello", "Hi", 1))    # 'Hi World Hello Python' (แค่ครั้งเดียว)

# แยก string
words = text.split()                    # ['Hello', 'World', 'Hello', 'Python']
parts = text.split(" ")                 # ['Hello', 'World', 'Hello', 'Python']

# แยกด้วย delimiter
csv = "apple,banana,orange"
fruits = csv.split(",")                 # ['apple', 'banana', 'orange']

# จำกัดจำนวนการแยก
text = "a-b-c-d-e"
parts = text.split("-", 2)              # ['a', 'b', 'c-d-e']
```

---

## 🎯 การจัดการ Unicode และภาษาไทย

### การทำงานกับภาษาไทย
```python
# ภาษาไทยใน Python 3
thai_text = "สวัสดี ประเทศไทย"

# นับตัวอักษร
print(len(thai_text))                   # 18 (รวมช่องว่าง)

# การตัดคำ (ต้องใช้ library)
# pip install pythainlp
import pythainlp

words = pythainlp.word_tokenize("สวัสดีครับยินดีที่ได้รู้จัก")
print(words)  # ['สวัสดี', 'ครับ', 'ยินดี', 'ที่', 'ได้', 'รู้จัก']

# การ normalize
normalized = pythainlp.util.normalize("เเปล")  # "แปล"
```

### การจัดการ Emoji
```python
# Emoji ใน string
text = "Hello 🌍 Python 🐍"

# นับ emoji
import emoji
emoji_count = emoji.emoji_count(text)  # 2

# แทนที่ emoji
text_without_emoji = emoji.replace_emoji(text, "")
```

---

## 📊 การประมวลผลขั้นสูง

### Regular Expressions
```python
import re

text = "Email: test@example.com, Phone: 123-456-7890"

# ค้นหา pattern
emails = re.findall(r'\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Z|a-z]{2,}\b', text)
print(emails)  # ['test@example.com']

# แทนที่
cleaned = re.sub(r'\d', 'X', text)  # แทนที่ตัวเลขด้วย X

# แยก
parts = re.split(r'[,|:]', text)    # แยกด้วย , หรือ :

# ตรวจสอบ pattern
is_email = re.match(r'.*@.*\..*', 'test@example.com')  # True
```

### การจัดการ Text Files
```python
# อ่านไฟล์ภาษาไทย
with open('thai_text.txt', 'r', encoding='utf-8') as f:
    content = f.read()

# เขียนไฟล์ภาษาไทย
with open('output.txt', 'w', encoding='utf-8') as f:
    f.write("สวัสดีครับ")

# ประมวลผลทีละบรรทัด
with open('data.txt', 'r', encoding='utf-8') as f:
    for line in f:
        line = line.strip()
        if line:  # ไม่เป็นบรรทัดว่าง
            print(line.upper())
```

### การทำงานกับ Paths
```python
from pathlib import Path

# สร้าง path
file_path = Path("data") / "input.txt"

# อ่านเขียนไฟล์
content = file_path.read_text(encoding='utf-8')
file_path.write_text("Hello ภาษาไทย", encoding='utf-8')

# จัดการชื่อไฟล์
print(file_path.stem)      # 'input'
print(file_path.suffix)    # '.txt'
print(file_path.name)      # 'input.txt'
```

---

## 🎨 การสร้าง Text Templates

### การใช้ Template Strings
```python
from string import Template

# Basic template
template = Template("ชื่อ $name อายุ $age ปี")
result = template.substitute(name="สมชาย", age=25)

# Template จาก dictionary
data = {"name": "วิชัย", "age": 30, "city": "กรุงเทพฯ"}
template = Template("ชื่อ $name อายุ $age ปี อยู่ $city")
result = template.substitute(data)

# Safe substitution (ไม่ error ถ้าขาด key)
template = Template("ชื่อ $name อายุ $age")
result = template.safe_substitute(name="สมชาย")  # ไม่ error ถ้าไม่มี age
```

### การสร้าง Report Templates
```python
def generate_report(name, scores, average):
    template = f"""
รายงานผลการเรียน
ชื่อ: {name}
{'='*30}
"""
    
    for subject, score in scores.items():
        template += f"{subject}: {score}\n"
    
    template += f"{'='*30}\n"
    template += f"คะแนนเฉลี่ย: {average:.2f}"
    
    return template

scores = {"คณิต": 85, "วิทย์": 90, "อังกฤษ": 78}
report = generate_report("สมชาย", scores, 84.33)
print(report)
```

---

## 🛠️ การ Debug String

### การตรวจสอบปัญหาทั่วไป
```python
# ตรวจสอบ encoding
text = "สวัสดี"
print(text.encode('utf-8'))  # b'\xe0\xb8\xaa\xe0\xb8\xa7\xe0\xb8\xb1\xe0\xb8\xaa\xe0\xb8\x94\xe0\xb8\xb5'

# ตรวจสอบ characters
for i, char in enumerate(text):
    print(f"Position {i}: '{char}' (Unicode: {ord(char)})")

# ตรวจสอบ whitespace
import string
def check_whitespace(s):
    for i, char in enumerate(s):
        if char in string.whitespace:
            print(f"Whitespace at position {i}: '{repr(char)}'")

check_whitespace("Hello\tWorld\n")
```

### การแก้ปัญหาที่พบบ่อย
```python
# ❌ ปัญหา: UnicodeDecodeError
try:
    with open('file.txt', 'r') as f:  # ไม่ระบุ encoding
        content = f.read()
except UnicodeDecodeError:
    # ✅ แก้: ระบุ encoding
    with open('file.txt', 'r', encoding='utf-8') as f:
        content = f.read()

# ❌ ปัญหา: การเปรียบเทียบ string
text1 = "Hello"
text2 = "hello"
print(text1 == text2)  # False

# ✅ แก้: แปลงเป็นกรณีเดียวกัน
print(text1.lower() == text2.lower())  # True

# ❌ ปัญหา: การต่อ string ใน loop
result = ""
for i in range(1000):
    result += str(i)  # ช้ามาก!

# ✅ แก้: ใช้ list แล้ว join
parts = [str(i) for i in range(1000)]
result = "".join(parts)  # เร็วกว่ามาก
```

---

## 💡 Best Practices

### 1. เลือกวิธี formatting ที่เหมาะสม
```python
# ✅ แนะนำ: f-string (Python 3.6+)
name = "สมชาย"
message = f"สวัสดี {name}"

# ✅ ดี: .format() สำหรับ template
template = "ชื่อ {name} อายุ {age}"
filled = template.format(name="วิชัย", age=25)

# ❌ หลีกเลี่ยง: % formatting (เก่า)
message = "สวัสดี %s" % name
```

### 2. ระวังการเปลี่ยนแปลง string
```python
# ❌ ผิด: string เป็น immutable
text = "Hello"
text[0] = "J"  # TypeError!

# ✅ ถูก: สร้าง string ใหม่
text = "J" + text[1:]  # "Jello"
```

### 3. ใช้ method ที่เหมาะสม
```python
# ✅ ดี: ใช้ .join() สำหรับ list
words = ["Hello", "World", "Python"]
sentence = " ".join(words)

# ❌ ไม่ดี: ใช้ + ใน loop
sentence = ""
for word in words:
    sentence += word + " "
```

### 4. จัดการ encoding ให้ถูกต้อง
```python
# ✅ ระบุ encoding เสมอ
with open('file.txt', 'r', encoding='utf-8') as f:
    content = f.read()

# ✅ ใช้ pathlib
from pathlib import Path
content = Path('file.txt').read_text(encoding='utf-8')
```

---

## 📝 สรุป String Methods ที่ใช้บ่อย

| Category | Methods | ตัวอย่าง |
|----------|---------|-----------|
| Case | `.upper()`, `.lower()`, `.title()`, `.capitalize()` | `"hello".upper()` |
| Whitespace | `.strip()`, `.lstrip()`, `.rstrip()` | `"  hello  ".strip()` |
| Search | `.find()`, `.index()`, `.count()`, `.startswith()` | `"hello".find("e")` |
| Replace | `.replace()` | `"hello".replace("e", "a")` |
| Split | `.split()`, `.rsplit()` | `"a,b,c".split(",")` |
| Join | `.join()` | `",".join(["a", "b", "c"])` |
| Check | `.isalpha()`, `.isdigit()`, `.isalnum()` | `"123".isdigit()` |
| Format | `.format()`, f-strings | `f"Hello {name}"` |

---

*สร้างเมื่อ: 4 มีนาคม 2026*  
*ปรับปรุงล่าสุด: 4 มีนาคม 2026*
