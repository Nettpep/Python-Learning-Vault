# Loops For While - วัฏจักรและการทำซ้ำ

## 🔄 แนวคิดพื้นฐาน

Loops คือ **การทำซ้ำคำสั่ง** หลายๆ ครั้ง ช่วยให้เราไม่ต้องเขียนโค้ดซ้ำๆ กันหลายบรรทัด

```python
# ทำซ้ำ 5 ครั้ง
for i in range(5):
    print(f"รอบที่ {i}")

# ทำซ้ำจนกว่าเงื่อนไขจะเป็นเท็จ
count = 0
while count < 5:
    print(f"นับ = {count}")
    count += 1
```

---

## 🎯 For Loop

### การใช้งานพื้นฐาน
```python
# วนลูปตามจำนวนครั้ง
for i in range(5):
    print(f"รอบที่ {i}")

# ระบุ start, stop, step
for i in range(2, 10, 2):  # 2, 4, 6, 8
    print(i)

# วนลูปย้อนหลัง
for i in range(5, 0, -1):  # 5, 4, 3, 2, 1
    print(i)
```

### การวนลูปกับ Collections
```python
# List
fruits = ["แอปเปิ้ล", "กล้วย", "ส้ม"]
for fruit in fruits:
    print(f"ผลไม้: {fruit}")

# Dictionary
person = {"name": "สมชาย", "age": 25, "city": "กรุงเทพฯ"}
for key in person:
    print(f"{key}: {person[key]}")

# วนลูป key-value
for key, value in person.items():
    print(f"{key}: {value}")

# String
text = "Hello"
for char in text:
    print(f"ตัวอักษร: {char}")
```

### การวนลูปพร้อม Index
```python
fruits = ["แอปเปิ้ล", "กล้วย", "ส้ม"]

# ใช้ enumerate
for index, fruit in enumerate(fruits):
    print(f"{index}: {fruit}")

# ระบุ start index
for index, fruit in enumerate(fruits, start=1):
    print(f"ลำดับ {index}: {fruit}")

# แบบดั้งเดิม
for i in range(len(fruits)):
    print(f"{i}: {fruits[i]}")
```

### List Comprehension
```python
# สร้าง list ใหม่จาก list เดิม
numbers = [1, 2, 3, 4, 5]
squared = [x**2 for x in numbers]  # [1, 4, 9, 16, 25]

# กรองข้อมูล
even_numbers = [x for x in numbers if x % 2 == 0]  # [2, 4]

# ซับซ้อนขึ้น
words = ["hello", "world", "python"]
capitalized = [word.upper() for word in words if len(word) > 4]

# Dictionary Comprehension
squares_dict = {x: x**2 for x in range(5)}  # {0: 0, 1: 1, 2: 4, 3: 9, 4: 16}
```

---

## 🔄 While Loop

### การใช้งานพื้นฐาน
```python
# นับจำนวน
count = 0
while count < 5:
    print(f"นับ = {count}")
    count += 1

# ทำซ้ำจนกว่าผู้ใช้จะพิมพ์ 'quit'
user_input = ""
while user_input != "quit":
    user_input = input("พิมพ์อะไรสักอย่าง (พิมพ์ 'quit' เพื่อออก): ")
    print(f"คุณพิมพ์: {user_input}")
```

### การใช้กับตรรกะซับซ้อน
```python
# เกมทายเลข
import random

secret_number = random.randint(1, 100)
guess = 0
attempts = 0

while guess != secret_number and attempts < 10:
    try:
        guess = int(input("ทายเลข 1-100: "))
        attempts += 1
        
        if guess < secret_number:
            print("น้อยไป!")
        elif guess > secret_number:
            print("มากไป!")
        else:
            print(f"ถูกแล้ว! ใช้ {attempts} ครั้ง")
    except ValueError:
        print("กรุณาใส่ตัวเลขเท่านั้น")

if attempts >= 10 and guess != secret_number:
    print(f"หมดเวลา! เลขคือ {secret_number}")
```

### การใช้ Flag
```python
# ตรวจสอบข้อมูล
data_valid = True
errors = []

while data_valid:
    name = input("ชื่อ: ")
    if not name:
        errors.append("กรุณาใส่ชื่อ")
        data_valid = False
    
    age = input("อายุ: ")
    if not age.isdigit():
        errors.append("อายุต้องเป็นตัวเลข")
        data_valid = False
    
    # ถ้าข้อมูลถูกต้อง ออกจาก loop
    if data_valid:
        break

if not data_valid:
    print("ข้อมูลไม่ถูกต้อง:")
    for error in errors:
        print(f"- {error}")
else:
    print(f"สวัสดี {name} อายุ {age} ปี")
```

---

## 🎪 Loop Control Statements

### Break Statement
```python
# หยุด loop ทันที
numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
for num in numbers:
    if num == 5:
        break
    print(num)  # พิมพ์ 1, 2, 3, 4

# ค้นหาใน list
fruits = ["แอปเปิ้ล", "กล้วย", "ส้ม", "มะละกอ"]
target = "ส้ม"
found = False

for fruit in fruits:
    if fruit == target:
        found = True
        break

if found:
    print(f"พบ {target}")
else:
    print(f"ไม่พบ {target}")
```

### Continue Statement
```python
# ข้ามรอบปัจจุบัน
numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
for num in numbers:
    if num % 2 == 0:
        continue  # ข้ามเลขคู่
    print(num)  # พิมพ์เลขคี่: 1, 3, 5, 7, 9

# กรองข้อมูล
data = ["valid", "", None, "good", "", "bad"]
valid_data = []
for item in data:
    if not item:  # ถ้าเป็นค่าว่าง
        continue
    valid_data.append(item)

print(valid_data)  # ["valid", "good", "bad"]
```

### Pass Statement
```python
# ไม่ทำอะไร (placeholder)
for i in range(5):
    pass  # วนลูปแต่ไม่ทำอะไร

# ใช้กับ conditional ที่ยังไม่ได้ implement
if condition:
    pass  # TODO: implement later
else:
    print("Do something")
```

---

## 🎨 Nested Loops

### Loop ซ้อน Loop
```python
# ตารางคูณ
for i in range(1, 4):  # 1, 2, 3
    for j in range(1, 4):  # 1, 2, 3
        print(f"{i} x {j} = {i * j}")
    print("---")  # คั่นแถว

# ประมวลผลข้อมูล 2D
matrix = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
]

for row in matrix:
    for element in row:
        print(element, end=" ")
    print()  # ขึ้นบรรทัดใหม่
```

### การค้นหาใน 2D
```python
# ค้นหาตัวเลขใน matrix
matrix = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
]
target = 5
found = False

for i, row in enumerate(matrix):
    for j, element in enumerate(row):
        if element == target:
            print(f"พบ {target} ที่ตำแหน่ง [{i}][{j}]")
            found = True
            break
    if found:
        break

if not found:
    print(f"ไม่พบ {target}")
```

---

## 🛠️ การใช้งานขั้นสูง

### Loop กับ Functions
```python
def process_items(items, processor_func):
    """ประมวลผล items ด้วย function ที่กำหนด"""
    results = []
    for item in items:
        try:
            result = processor_func(item)
            results.append(result)
        except Exception as e:
            print(f"Error processing {item}: {e}")
            results.append(None)
    return results

# การใช้งาน
numbers = [1, 2, 3, 4, 5]
squared = process_items(numbers, lambda x: x**2)
print(squared)  # [1, 4, 9, 16, 25]
```

### Loop กับ Generators
```python
def fibonacci_generator(n):
    """สร้างเลขฟีโบนัชชี"""
    a, b = 0, 1
    for _ in range(n):
        yield a
        a, b = b, a + b

# การใช้งาน
for num in fibonacci_generator(10):
    print(num)

# หรือแปลงเป็น list
fib_list = list(fibonacci_generator(10))
print(fib_list)
```

### Loop กับ Iterator
```python
# สร้าง iterator แบบกำหนดเอง
class Countdown:
    def __init__(self, start):
        self.current = start
    
    def __iter__(self):
        return self
    
    def __next__(self):
        if self.current <= 0:
            raise StopIteration
        self.current -= 1
        return self.current + 1

# การใช้งาน
for num in Countdown(5):
    print(num)  # 5, 4, 3, 2, 1
```

---

## 📊 การประมวลผลข้อมูลจริง

### การอ่านไฟล์ทีละบรรทัด
```python
# อ่านไฟล์ CSV
def process_csv_file(filename):
    """ประมวลผลไฟล์ CSV"""
    data = []
    with open(filename, 'r', encoding='utf-8') as file:
        for line_num, line in enumerate(file, 1):
            line = line.strip()
            if line:  # ไม่เป็นบรรทัดว่าง
                try:
                    # แยกข้อมูล
                    parts = line.split(',')
                    if len(parts) >= 2:
                        data.append({
                            'line': line_num,
                            'name': parts[0].strip(),
                            'value': float(parts[1].strip())
                        })
                except (ValueError, IndexError) as e:
                    print(f"Error on line {line_num}: {e}")
    return data

# การใช้งาน
# data = process_csv_file('data.csv')
# for item in data:
#     print(f"{item['name']}: {item['value']}")
```

### การประมวลผล API Response
```python
def process_api_responses(responses):
    """ประมวลผล response จาก API หลายๆ ครั้ง"""
    successful = []
    failed = []
    
    for i, response in enumerate(responses):
        if response.get('status') == 'success':
            successful.append({
                'index': i,
                'data': response.get('data'),
                'timestamp': response.get('timestamp')
            })
        else:
            failed.append({
                'index': i,
                'error': response.get('error'),
                'code': response.get('status_code')
            })
    
    return successful, failed

# ตัวอย่างข้อมูล
responses = [
    {'status': 'success', 'data': {'id': 1}, 'timestamp': '2023-01-01'},
    {'status': 'error', 'error': 'Not found', 'status_code': 404},
    {'status': 'success', 'data': {'id': 2}, 'timestamp': '2023-01-02'}
]

success, fail = process_api_responses(responses)
print(f"สำเร็จ: {len(success)}, ล้มเหลว: {len(fail)}")
```

---

## 🎯 การเพิ่มประสิทธิภาพ

### การใช้ enumerate แทน range(len())
```python
# ❌ ไม่ดี
fruits = ["แอปเปิ้ล", "กล้วย", "ส้ม"]
for i in range(len(fruits)):
    print(f"{i}: {fruits[i]}")

# ✅ ดีกว่า
for i, fruit in enumerate(fruits):
    print(f"{i}: {fruit}")
```

### การใช้ zip วนลูปหลาย list
```python
names = ["สมชาย", "วิชัย", "มานี"]
ages = [25, 30, 28]
cities = ["กรุงเทพฯ", "เชียงใหม่", "ภูเก็ต"]

# วนลูปพร้อมกัน
for name, age, city in zip(names, ages, cities):
    print(f"{name} ({age}) อยู่ที่ {city}")

# ใช้ enumerate ด้วย zip
for i, (name, age) in enumerate(zip(names, ages)):
    print(f"{i+1}. {name} อายุ {age}")
```

### การใช้ List Comprehension แทน Loop
```python
# ❌ ไม่ดี
squares = []
for i in range(10):
    squares.append(i**2)

# ✅ ดีกว่า
squares = [i**2 for i in range(10)]

# กรองและแปลงพร้อมกัน
filtered_squares = [i**2 for i in range(10) if i % 2 == 0]
```

### การใช้ Generator Expression
```python
# สำหรับข้อมูลจำนวนมาก
numbers = range(1000000)

# ❌ ใช้ memory เยอะ
squares_list = [x**2 for x in numbers]  # สร้าง list ทั้งหมด

# ✅ ประหยัด memory
squares_gen = (x**2 for x in numbers)  # สร้าง generator

# ใช้กับ sum, max, min
total = sum(x**2 for x in numbers)  # ไม่เก็บผลลัพธ์ทั้งหมด
```

---

## 🛡️ การจัดการ Error ใน Loops

### การใช้ Try-Except ใน Loop
```python
def safe_process_items(items):
    """ประมวลผล items อย่างปลอดภัย"""
    results = []
    errors = []
    
    for i, item in enumerate(items):
        try:
            # ทำงานที่อาจเกิด error
            result = risky_operation(item)
            results.append(result)
        except ValueError as e:
            errors.append(f"Item {i}: ValueError - {e}")
        except TypeError as e:
            errors.append(f"Item {i}: TypeError - {e}")
        except Exception as e:
            errors.append(f"Item {i}: Unexpected error - {e}")
    
    return results, errors

def risky_operation(item):
    """ฟังก์ชันที่อาจเกิด error"""
    if not isinstance(item, (int, float)):
        raise TypeError("Item must be number")
    if item < 0:
        raise ValueError("Item must be positive")
    return item ** 2
```

### การจัดการ KeyboardInterrupt
```python
def long_running_process():
    """กระบวนการที่ใช้เวลานาน"""
    try:
        for i in range(1000000):
            # ทำงานบางอย่าง
            result = complex_calculation(i)
            
            # แสดงความคืบหน้า
            if i % 10000 == 0:
                print(f"Progress: {i//10000}%")
                
    except KeyboardInterrupt:
        print("\nผู้ใช้ยกเลิกการทำงาน")
        return None
    except Exception as e:
        print(f"Error: {e}")
        return None
    else:
        print("เสร็จสิ้นการทำงาน")
        return result
```

---

## 💡 Best Practices

### 1. เลือก Loop ที่เหมาะสม
```python
# ✅ ใช้ for loop เมื่อรู้จำนวนรอบ
for i in range(10):
    print(i)

# ✅ ใช้ while loop เมื่อไม่รู้จำนวนรอบ
while user_input != "quit":
    user_input = input("Enter command: ")
```

### 2. หลีกเลี่ยง Infinite Loop
```python
# ❌ อันตราย
while True:
    print("This will never end!")

# ✅ ปลอดภัย
attempts = 0
max_attempts = 10
while attempts < max_attempts:
    # do something
    attempts += 1
```

### 3. ใช้ List Comprehension เมื่อเหมาะสม
```python
# ✅ ดี: ง่ายและอ่านง่าย
squares = [x**2 for x in range(10)]

# ❌ ซับซ้อนเกินไป
complex = [x**2 if x % 2 == 0 else x**3 for x in range(10) if x > 5]
```

### 4. ตั้งชื่อตัวแปรที่ชัดเจน
```python
# ✅ ดี
for student in students:
    for subject in subjects:
        process_grade(student, subject)

# ❌ ไม่ชัดเจน
for i in items:
    for j in things:
        do_something(i, j)
```

---

## 📝 สรุป Loop Patterns

| Pattern | วัตถุประสงค์ | ตัวอย่าง |
|---------|-------------|-----------|
| `for item in items` | วนลูป collection | `for fruit in fruits` |
| `for i in range(n)` | วนลูป n ครั้ง | `for i in range(5)` |
| `while condition` | วนจนกว่าเงื่อนไขเป็นเท็จ | `while count < 10` |
| `enumerate()` | วนลูปพร้อม index | `for i, item in enumerate(items)` |
| `zip()` | วนหลาย list พร้อมกัน | `for a, b in zip(list1, list2)` |
| `list comprehension` | สร้าง list ใหม่ | `[x*2 for x in items]` |
| `break` | หยุด loop | `if found: break` |
| `continue` | ข้ามรอบ | `if invalid: continue` |

---

## 🎯 แบบฝึกหัด

### แบบฝึกหัด 1: คำนวณผลรวม
```python
# เขียนฟังก์ชันคำนวณผลรวมเลขคู่ใน list
def sum_even_numbers(numbers):
    pass
```

### แบบฝึกหัด 2: ค้นหาค่าสูงสุด
```python
# เขียนฟังก์ชันหาค่าสูงสุดโดยไม่ใช้ max()
def find_maximum(numbers):
    pass
```

### แบบฝึกหัด 3: กรองข้อมูล
```python
# เขียนฟังก์ชันกรอง string ที่ยาว > 5
def filter_long_strings(strings):
    pass
```

---

*สร้างเมื่อ: 4 มีนาคม 2026*  
*ปรับปรุงล่าสุด: 4 มีนาคม 2026*
