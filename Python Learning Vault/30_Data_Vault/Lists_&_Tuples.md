# Lists & Tuples - ลำดับของสรรพสิ่ง

## 📚 แนวคิดพื้นฐาน

Lists และ Tuples คือ **โครงสร้างข้อมูลแบบลำดับ** (sequence) ที่เก็บข้อมูลหลายๆ ค่าไว้ด้วยกัน แต่มีความแตกต่างที่สำคัญคือ Lists **เปลี่ยนแปลงได้** (mutable) ส่วน Tuples **เปลี่ยนแปลงไม่ได้** (immutable)

```python
# List - mutable (เปลี่ยนแปลงได้)
fruits = ["แอปเปิ้ล", "กล้วย", "ส้ม"]
fruits.append("มะละกอ")  # เพิ่มได้

# Tuple - immutable (เปลี่ยนแปลงไม่ได้)
coordinates = (10, 20)
# coordinates[0] = 15  # Error!
```

---

## 📝 Lists (รายการ)

### การสร้าง List
```python
# วิธีต่างๆ ในการสร้าง list
empty_list = []
numbers = [1, 2, 3, 4, 5]
mixed = [1, "hello", 3.14, True]
nested = [[1, 2], [3, 4], [5, 6]]

# ใช้ list() constructor
string_to_list = list("hello")  # ['h', 'e', 'l', 'l', 'o']
range_to_list = list(range(5))  # [0, 1, 2, 3, 4]

# List comprehension
squares = [x**2 for x in range(10)]  # [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
```

### การเข้าถึงข้อมูล
```python
fruits = ["แอปเปิ้ล", "กล้วย", "ส้ม", "มะละกอ", "ลิ้นจี่"]

# Indexing (0-based)
print(fruits[0])        # แอปเปิ้ล
print(fruits[2])        # ส้ม

# Negative indexing
print(fruits[-1])       # ลิ้นจี่
print(fruits[-2])       # มะละกอ

# Slicing
print(fruits[1:4])      # ['กล้วย', 'ส้ม', 'มะละกอ']
print(fruits[:3])       # ['แอปเปิ้ล', 'กล้วย', 'ส้ม']
print(fruits[2:])       # ['ส้ม', 'มะละกอ', 'ลิ้นจี่']
print(fruits[::2])      # ['แอปเปิ้ล', 'ส้ม', 'ลิ้นจี่'] (ทุก 2 ตัว)
print(fruits[::-1])     # ['ลิ้นจี่', 'มะละกอ', 'ส้ม', 'กล้วย', 'แอปเปิ้ล'] (กลับด้าน)
```

### การแก้ไข List
```python
fruits = ["แอปเปิ้ล", "กล้วย", "ส้ม"]

# แก้ไขค่า
fruits[1] = "มะม่วง"   # ['แอปเปิ้ล', 'มะม่วง', 'ส้ม']

# เพิ่มข้อมูล
fruits.append("ลิ้นจี่")        # ['แอปเปิ้ล', 'มะม่วง', 'ส้ม', 'ลิ้นจี่']
fruits.insert(1, "ทุเรียน")     # ['แอปเปิ้ล', 'ทุเรียน', 'มะม่วง', 'ส้ม', 'ลิ้นจี่']

# ขยาย list
fruits.extend(["ฝรั่ง", "มะนาว"])  # เพิ่มหลายตัวพร้อมกัน

# ลบข้อมูล
fruits.remove("ส้ม")           # ลบตามค่า
popped = fruits.pop()           # ลบตัวสุดท้ายและคืนค่า
popped_first = fruits.pop(0)    # ลบตัวแรกและคืนค่า
del fruits[1]                   # ลบตาม index

# ล้าง list
fruits.clear()                  # []
```

### การค้นหาและตรวจสอบ
```python
fruits = ["แอปเปิ้ล", "กล้วย", "ส้ม", "มะละกอ", "กล้วย"]

# ตรวจสอบว่ามีข้อมูลหรือไม่
print("กล้วย" in fruits)        # True
print("มะม่วง" in fruits)       # False

# หาตำแหน่ง
print(fruits.index("ส้ม"))       # 2
# print(fruits.index("มะม่วง"))  # ValueError

# นับจำนวน
print(fruits.count("กล้วย"))     # 2

# ความยาว
print(len(fruits))               # 5
```

### การเรียงลำดับ
```python
numbers = [3, 1, 4, 1, 5, 9, 2, 6]

# เรียงลำดับ (เปลี่ยน list เดิม)
numbers.sort()                   # [1, 1, 2, 3, 4, 5, 6, 9]
numbers.sort(reverse=True)       # [9, 6, 5, 4, 3, 2, 1, 1]

# เรียงลำดับ (คืน list ใหม่)
sorted_numbers = sorted(numbers)  # [1, 1, 2, 3, 4, 5, 6, 9]

# เรียงลำดับ string
fruits = ["แอปเปิ้ล", "กล้วย", "ส้ม"]
fruits.sort()                    # ['กล้วย', 'แอปเปิ้ล', 'ส้ม']

# เรียงลำดับตามคีย์
words = ["apple", "banana", "cherry"]
words.sort(key=len)              # ['apple', 'banana', 'cherry']
```

---

## 🎯 Tuples (ทูเพิล)

### การสร้าง Tuple
```python
# วิธีต่างๆ ในการสร้าง tuple
empty_tuple = ()
single_item = (42,)              # ต้องมี comma!
coordinates = (10, 20)
rgb = (255, 0, 0)
mixed = (1, "hello", 3.14)

# ไม่ต้องมีวงเล็บ (tuple packing)
point = 10, 20                   # (10, 20)

# ใช้ tuple() constructor
list_to_tuple = tuple([1, 2, 3])  # (1, 2, 3)
string_to_tuple = tuple("hello")  # ('h', 'e', 'l', 'l', 'o')

# Tuple comprehension (ใช้ generator)
squares_tuple = tuple(x**2 for x in range(5))  # (0, 1, 4, 9, 16)
```

### การเข้าถึงข้อมูล
```python
coordinates = (10, 20, 30)

# Indexing และ slicing เหมือน list
print(coordinates[0])            # 10
print(coordinates[1:3])          # (20, 30)
print(coordinates[-1])           # 30

# แต่ไม่สามารถแก้ไขได้
# coordinates[0] = 15            # TypeError!
```

### Tuple Unpacking
```python
# Basic unpacking
point = (10, 20)
x, y = point                     # x = 10, y = 20

# ข้ามค่า
coordinates = (10, 20, 30, 40)
x, y, _, z = coordinates         # _ ข้ามค่าที่ 3

# ใช้ * เพื่อรับค่าที่เหลือ
first, *middle, last = [1, 2, 3, 4, 5]
# first = 1, middle = [2, 3, 4], last = 5

# ในฟังก์ชัน
def get_coordinates():
    return (10.5, 20.3, 15.7)

x, y, z = get_coordinates()
```

### การใช้งาน Tuple ในจริง
```python
# ค่าคงที่
COLORS = {
    "RED": (255, 0, 0),
    "GREEN": (0, 255, 0),
    "BLUE": (0, 0, 255)
}

# พิกัด
points = [(0, 0), (10, 20), (30, 40)]
for x, y in points:
    print(f"Point: ({x}, {y})")

# คืนค่าหลายค่าจากฟังก์ชัน
def calculate_stats(numbers):
    return min(numbers), max(numbers), sum(numbers) / len(numbers)

min_val, max_val, avg = calculate_stats([1, 2, 3, 4, 5])
```

---

## 🎪 การดำเนินการขั้นสูง

### List Comprehension ขั้นสูง
```python
# พื้นฐาน
squares = [x**2 for x in range(10)]

# มีเงื่อนไข
even_squares = [x**2 for x in range(10) if x % 2 == 0]

# ซับซ้อนขึ้น
words = ["hello", "world", "python", "programming"]
result = [word.upper() for word in words if len(word) > 5]

# Nested comprehension
matrix = [[i*j for j in range(3)] for i in range(3)]
# [[0, 0, 0], [0, 1, 2], [0, 2, 4]]

# ใช้กับฟังก์ชัน
numbers = [1, 2, 3, 4, 5]
processed = [str(x).upper() for x in numbers if x > 2]
```

### การใช้ Built-in Functions
```python
numbers = [3, 1, 4, 1, 5, 9, 2, 6]

# ฟังก์ชันที่ใช้บ่อย
print(len(numbers))               # 8
print(sum(numbers))               # 31
print(min(numbers))               # 1
print(max(numbers))               # 9

# all() และ any()
print(all(x > 0 for x in numbers))    # True (ทุกตัว > 0)
print(any(x > 5 for x in numbers))     # True (มีบางตัว > 5)

# enumerate()
for index, value in enumerate(numbers):
    print(f"{index}: {value}")

# zip()
names = ["สมชาย", "วิชัย", "มานี"]
ages = [25, 30, 28]
for name, age in zip(names, ages):
    print(f"{name} อายุ {age}")

# map() และ filter()
squared = list(map(lambda x: x**2, numbers))
evens = list(filter(lambda x: x % 2 == 0, numbers))
```

### การจัดการ List ขนาดใหญ่
```python
# ใช้ generator แทน list สำหรับข้อมูลจำนวนมาก
def process_large_data():
    # Generator expression
    large_numbers = (x**2 for x in range(1000000))
    
    # ประมวลผลทีละส่วน
    total = 0
    for i, num in enumerate(large_numbers):
        total += num
        if i % 100000 == 0:
            print(f"Processed: {i}")
    
    return total

# ใช้ itertools สำหรับการดำเนินการซับซ้อน
import itertools

# สร้าง combinations
items = ['A', 'B', 'C', 'D']
combinations = list(itertools.combinations(items, 2))
# [('A', 'B'), ('A', 'C'), ('A', 'D'), ('B', 'C'), ('B', 'D'), ('C', 'D')]

# สร้าง permutations
permutations = list(itertools.permutations(items, 2))
# [('A', 'B'), ('A', 'C'), ('A', 'D'), ('B', 'A'), ('B', 'C'), ...]
```

---

## 🛠️ การใช้งานจริง

### การจัดการข้อมูล CSV
```python
def process_csv_data(data_rows):
    """ประมวลผลข้อมูล CSV"""
    headers = data_rows[0]  # แถวหัวตาราง
    processed_data = []
    
    for row in data_rows[1:]:  # ข้ามแถวหัวตาราง
        if len(row) == len(headers):
            # สร้าง dictionary จากแถว
            row_dict = dict(zip(headers, row))
            
            # แปลงข้อมูลตัวเลข
            try:
                row_dict['price'] = float(row_dict['price'])
                row_dict['quantity'] = int(row_dict['quantity'])
                processed_data.append(row_dict)
            except (ValueError, KeyError):
                continue
    
    return processed_data

# ตัวอย่างข้อมูล
csv_data = [
    ['name', 'price', 'quantity'],
    ['แอปเปิ้ล', '25.50', '10'],
    ['กล้วย', '15.00', '20'],
    ['ส้ม', '30.75', '15']
]

processed = process_csv_data(csv_data)
print(processed)
```

### การจัดการ Queue
```python
from collections import deque

class TaskQueue:
    """คิวสำหรับจัดการงาน"""
    
    def __init__(self):
        self.tasks = deque()
        self.completed = []
    
    def add_task(self, task):
        """เพิ่มงาน"""
        self.tasks.append(task)
    
    def get_next_task(self):
        """ดึงงานถัดไป"""
        if self.tasks:
            return self.tasks.popleft()
        return None
    
    def complete_task(self, task):
        """บันทึกงานที่เสร็จ"""
        self.completed.append(task)
    
    def get_pending_count(self):
        """จำนวนงานที่รอดำเนินการ"""
        return len(self.tasks)

# การใช้งาน
queue = TaskQueue()
queue.add_task("ทำงาน A")
queue.add_task("ทำงาน B")
queue.add_task("ทำงาน C")

while queue.get_pending_count() > 0:
    task = queue.get_next_task()
    print(f"กำลังทำ: {task}")
    queue.complete_task(task)
```

---

## 🎯 การเพิ่มประสิทธิภาพ

### การเลือกระหว่าง List และ Tuple
```python
# ✅ ใช้ Tuple เมื่อข้อมูลไม่เปลี่ยน
COLORS = (255, 0, 0)           # สีแดง
COORDINATES = (10, 20, 30)    # พิกัด 3D
DAYS_OF_WEEK = ("จ", "อ", "พ", "พฤ", "ศ", "ส", "อา")

# ✅ ใช้ List เมื่อต้องเปลี่ยนแปลง
shopping_cart = []            # รถเข็นซื้อของ
user_actions = []             # ประวัติการกระทำ
dynamic_data = []             # ข้อมูลที่เปลี่ยนบ่อย

# ประสิทธิภาพ: Tuple เร็วกว่า List
import time

def test_performance():
    # List
    start = time.time()
    for i in range(1000000):
        my_list = [1, 2, 3, 4, 5]
        x = my_list[2]
    list_time = time.time() - start
    
    # Tuple
    start = time.time()
    for i in range(1000000):
        my_tuple = (1, 2, 3, 4, 5)
        x = my_tuple[2]
    tuple_time = time.time() - start
    
    print(f"List time: {list_time:.6f}s")
    print(f"Tuple time: {tuple_time:.6f}s")
```

### การใช้ Memory อย่างมีประสิทธิภาพ
```python
# ❌ ใช้ memory เยอะ
large_list = []
for i in range(1000000):
    large_list.append(i * 2)

# ✅ ประหยัด memory
large_generator = (i * 2 for i in range(1000000))

# ใช้กับ sum, max, min
total = sum(i * 2 for i in range(1000000))

# ใช้ array สำหรับตัวเลขเท่านั้น
import array
numbers_array = array.array('i', [1, 2, 3, 4, 5])  # ประหยัด memory กว่า list
```

---

## 💡 Best Practices

### 1. เลือกโครงสร้างที่เหมาะสม
```python
# ✅ ใช้ tuple สำหรับค่าคงที่
DIRECTIONS = ('N', 'NE', 'E', 'SE', 'S', 'SW', 'W', 'NW')

# ✅ ใช้ list สำหรับข้อมูลที่เปลี่ยนแปลง
user_preferences = ['dark_mode', 'notifications', 'auto_save']
```

### 2. ใช้ List Comprehension อย่างเหมาะสม
```python
# ✅ ดี: อ่านง่าย
squares = [x**2 for x in range(10)]

# ❌ ซับซ้อนเกินไป
complex = [x**2 if x % 2 == 0 else str(x) for x in range(10) if x > 3]

# ✅ แยกเป็นฟังก์ชัน
def process_number(x):
    if x % 2 == 0:
        return x**2
    return str(x)

processed = [process_number(x) for x in range(10) if x > 3]
```

### 3. ระวังการแก้ไข List ระหว่าง Loop
```python
# ❌ อันตราย: แก้ไข list ระหว่างวนลูป
items = [1, 2, 3, 4, 5]
for item in items:
    if item % 2 == 0:
        items.remove(item)  # จะทำให้เกิดปัญหา

# ✅ ดี: สร้าง list ใหม่
items = [1, 2, 3, 4, 5]
filtered = [item for item in items if item % 2 != 0]
```

### 4. ใช้ Type Hints
```python
from typing import List, Tuple, Optional

def process_data(items: List[int]) -> Tuple[int, int]:
    """ประมวลผลข้อมูลและคืนผลรวมกับค่าเฉลี่ย"""
    total = sum(items)
    average = total / len(items) if items else 0
    return total, average

def get_coordinates() -> Tuple[float, float, float]:
    """คืนพิกัด 3D"""
    return (10.5, 20.3, 15.7)
```

---

## 📝 สรุปการเลือกใช้

| สถานการณ์ | เลือกอะไร | เหตุผล |
|-----------|-----------|--------|
| ข้อมูลคงที่ | Tuple | Immutable, เร็วกว่า |
| ข้อมูลเปลี่ยนแปลง | List | Mutable, ยืดหยุ่น |
| Key-Value | Dictionary | เข้าถึงตาม key |
| ข้อมูลไม่ซ้ำ | Set | ตรวจสอบซ้ำได้เร็ว |
| ข้อมูลจำนวนมาก | Generator | ประหยัด memory |

---

## 🎯 แบบฝึกหัด

### แบบฝึกหัด 1: กรองข้อมูล
```python
# เขียนฟังก์ชันกรองเลขคู่จาก list
def filter_even_numbers(numbers):
    pass
```

### แบบฝึกหัด 2: สถิติข้อมูล
```python
# เขียนฟังก์ชันคำนวณสถิติจาก list ตัวเลข
def calculate_statistics(numbers):
    pass
```

### แบบฝึกหัด 3: การจัดการข้อมูล
```python
# เขียนฟังก์ชันจัดการรายการสินค้า
def manage_inventory(products, actions):
    pass
```

---

*สร้างเมื่อ: 4 มีนาคม 2026*  
*ปรับปรุงล่าสุด: 4 มีนาคม 2026*
