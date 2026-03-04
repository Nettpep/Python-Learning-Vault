# Modules & Packages - การนำเข้าพลังจากภายนอก

## 📦 แนวคิดพื้นฐาน

Module คือ **ไฟล์ Python ที่มีฟังก์ชันและคลาส** ส่วน Package คือ **โฟลเดอร์ที่รวม modules หลายๆ ไฟล์** ช่วยให้จัดระเบียบโค้ดและนำกลับมาใช้ใหม่ได้

```python
# การ import module
import math
print(math.sqrt(16))  # 4.0

# การ import ฟังก์ชันเฉพาะ
from datetime import datetime
now = datetime.now()

# การ import พร้อมชื่อใหม่
import numpy as np
```

---

## 📝 Modules พื้นฐาน

### การสร้าง Module
```python
# my_module.py
"""
Module ตัวอย่างสำหรับการคำนวณพื้นฐาน
"""

PI = 3.14159

def add(a, b):
    """บวกเลขสองตัว"""
    return a + b

def subtract(a, b):
    """ลบเลขสองตัว"""
    return a - b

def multiply(a, b):
    """คูณเลขสองตัว"""
    return a * b

def divide(a, b):
    """หารเลขสองตัว"""
    if b == 0:
        raise ValueError("Cannot divide by zero")
    return a / b

class Calculator:
    """เครื่องคิดเลข"""
    
    def __init__(self):
        self.history = []
    
    def calculate(self, operation, a, b):
        """คำนวณและบันทึกประวัติ"""
        operations = {
            'add': add,
            'subtract': subtract,
            'multiply': multiply,
            'divide': divide
        }
        
        if operation not in operations:
            raise ValueError(f"Unknown operation: {operation}")
        
        result = operations[operation](a, b)
        self.history.append(f"{a} {operation} {b} = {result}")
        return result
    
    def get_history(self):
        """ดูประวัติการคำนวณ"""
        return self.history

# ตัวแปรสำหรับทดสอบ
__version__ = "1.0.0"
__author__ = "Python Learning Vault"

# รันเมื่อ import
if __name__ == "__main__":
    print("This module is being run directly")
    calc = Calculator()
    print(calc.calculate('add', 5, 3))
```

### การใช้ Module
```python
# main.py
import my_module

# ใช้ฟังก์ชันจาก module
result = my_module.add(5, 3)
print(f"5 + 3 = {result}")

# ใช้คลาสจาก module
calc = my_module.Calculator()
calc.calculate('multiply', 4, 6)
print(calc.get_history())

# ใช้ตัวแปรจาก module
print(f"Module version: {my_module.__version__}")
print(f"PI = {my_module.PI}")
```

### วิธีการ Import ต่างๆ
```python
# 1. Import module ทั้งหมด
import math
print(math.sqrt(25))

# 2. Import พร้อม alias
import numpy as np
import pandas as pd

# 3. Import ฟังก์ชัน/คลาสเฉพาะ
from datetime import datetime, timedelta
from collections import Counter, defaultdict

# 4. Import ทั้งหมด (ไม่แนะนำ)
# from math import *

# 5. Import พร้อม alias
from sklearn.model_selection import train_test_split as tts

# 6. Import จาก package ย่อย
from urllib.request import urlopen
from os.path import join, exists

# 7. Dynamic import
module_name = "math"
import importlib
math_module = importlib.import_module(module_name)
```

---

## 🎪 Packages และโครงสร้าง

### การสร้าง Package
```
my_package/
├── __init__.py          # จำเป็นสำหรับ Python 2, optional สำหรับ Python 3
├── math_utils.py
├── string_utils.py
├── data_utils.py
└── subpackage/
    ├── __init__.py
    └── advanced.py
```

```python
# my_package/__init__.py
"""
Package สำหรับ utilities ต่างๆ
"""

# Import ฟังก์ชันหลักมาให้ใช้งานง่าย
from .math_utils import Calculator, add, subtract
from .string_utils import clean_text, format_text
from .data_utils import load_data, save_data

# ตัวแปร package level
VERSION = "1.0.0"
AUTHOR = "Python Learning Vault"

# ฟังก์ชัน package level
def get_info():
    """ดูข้อมูล package"""
    return f"Package v{VERSION} by {AUTHOR}"

# my_package/math_utils.py
def add(a, b):
    return a + b

def subtract(a, b):
    return a - b

class Calculator:
    def __init__(self):
        self.result = 0
    
    def add(self, x):
        self.result += x
        return self
    
    def multiply(self, x):
        self.result *= x
        return self
    
    def get_result(self):
        return self.result

# my_package/string_utils.py
def clean_text(text):
    """ทำความสะอาดข้อความ"""
    import re
    # ลบ whitespace พิเศษ
    text = re.sub(r'\s+', ' ', text.strip())
    # ลบตัวอักษรพิเศษ
    text = re.sub(r'[^\w\s]', '', text)
    return text.lower()

def format_text(text, style='title'):
    """จัดรูปแบบข้อความ"""
    if style == 'title':
        return text.title()
    elif style == 'upper':
        return text.upper()
    elif style == 'lower':
        return text.lower()
    else:
        return text

# my_package/data_utils.py
import json
import csv
from pathlib import Path

def load_data(filename):
    """โหลดข้อมูลจากไฟล์"""
    path = Path(filename)
    
    if not path.exists():
        raise FileNotFoundError(f"File {filename} not found")
    
    if path.suffix == '.json':
        with open(path, 'r', encoding='utf-8') as f:
            return json.load(f)
    elif path.suffix == '.csv':
        with open(path, 'r', encoding='utf-8') as f:
            reader = csv.DictReader(f)
            return list(reader)
    else:
        raise ValueError(f"Unsupported file type: {path.suffix}")

def save_data(data, filename):
    """บันทึกข้อมูลลงไฟล์"""
    path = Path(filename)
    
    if path.suffix == '.json':
        with open(path, 'w', encoding='utf-8') as f:
            json.dump(data, f, indent=2, ensure_ascii=False)
    elif path.suffix == '.csv':
        if not data:
            return
        
        with open(path, 'w', encoding='utf-8', newline='') as f:
            writer = csv.DictWriter(f, fieldnames=data[0].keys())
            writer.writeheader()
            writer.writerows(data)
    else:
        raise ValueError(f"Unsupported file type: {path.suffix}")
```

### การใช้ Package
```python
# การใช้ package
import my_package

# ใช้ฟังก์ชันที่ import มาใน __init__
result = my_package.add(5, 3)
print(f"5 + 3 = {result}")

calc = my_package.Calculator()
result = calc.add(10).multiply(2).get_result()
print(f"Result: {result}")

# ใช้ฟังก์ชันจาก module ย่อย
from my_package.string_utils import clean_text, format_text

text = "  Hello,   World!  "
clean = clean_text(text)
formatted = format_text(clean, 'title')
print(formatted)  # Hello World

# ใช้จาก subpackage
from my_package.subpackage.advanced import advanced_function
```

---

## 🛠️ Standard Library Modules ที่ใช้บ่อย

### Math และ Numbers
```python
import math
import random
import statistics

# Math module
print(math.pi)                    # 3.141592653589793
print(math.sqrt(16))              # 4.0
print(math.factorial(5))           # 120
print(math.gcd(48, 18))            # 6
print(math.log(100, 10))           # 2.0

# Random module
print(random.randint(1, 100))     # สุ่มเลข 1-100
print(random.choice([1, 2, 3, 4, 5]))  # สุ่มเลือก
random.shuffle([1, 2, 3, 4, 5])   # สับไพ่
print(random.random())            # 0.0 - 1.0

# Statistics module
numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
print(statistics.mean(numbers))   # 5.5
print(statistics.median(numbers)) # 5.5
print(statistics.stdev(numbers))  # 3.027...
```

### DateTime และ Time
```python
import datetime
import time
import calendar

# DateTime
now = datetime.datetime.now()
today = datetime.date.today()
current_time = datetime.time(14, 30, 0)

# สร้าง datetime จาก string
date_str = "2023-12-25 15:30:00"
parsed_date = datetime.datetime.strptime(date_str, "%Y-%m-%d %H:%M:%S")

# จัดรูปแบบ
formatted = now.strftime("%d/%m/%Y %H:%M:%S")

# การคำนวณวันที่
future = now + datetime.timedelta(days=30)
past = now - datetime.timedelta(hours=24)

# Time module
print(time.time())                # Unix timestamp
time.sleep(1)                     # รอ 1 วินาที

# Calendar
print(calendar.month(2023, 12))   # แสดงปฏิทินเดือนธันวาคม
print(calendar.isleap(2024))      # True (ปีอธิกสุรทิน)
```

### File System และ Path
```python
import os
import pathlib
import shutil
import glob

# OS module
print(os.getcwd())                 # Current directory
os.listdir('.')                    # List files in directory
os.mkdir('new_folder')             # Create directory
os.makedirs('dir1/dir2', exist_ok=True)  # Create nested directories

# Pathlib (แนะนำ)
from pathlib import Path

path = Path("data/input.txt")
print(path.name)                   # input.txt
print(path.stem)                   # input
print(path.suffix)                 # .txt
print(path.parent)                 # data

# ตรวจสอบไฟล์
print(path.exists())               # มีไฟล์หรือไม่
print(path.is_file())              # เป็นไฟล์หรือไม่
print(path.is_dir())               # เป็นโฟลเดอร์หรือไม่

# อ่านเขียนไฟล์
content = path.read_text(encoding='utf-8')
path.write_text("Hello World", encoding='utf-8')

# Glob pattern
txt_files = list(Path('.').glob('*.txt'))  # หาไฟล์ .txt ทั้งหมด
all_files = list(Path('.').rglob('*'))     # หาไฟล์ทั้งหมด (recursive)

# Shutil - การจัดการไฟล์ขั้นสูง
shutil.copy('source.txt', 'backup.txt')    # คัดลอกไฟล์
shutil.move('old.txt', 'new.txt')          # ย้ายไฟล์
shutil.rmtree('temp_folder')               # ลบโฟลเดอร์และข้างใน
```

### Collections และ Data Structures
```python
from collections import Counter, defaultdict, deque, namedtuple, OrderedDict

# Counter - นับค่า
words = ["hello", "world", "hello", "python", "hello"]
word_count = Counter(words)
print(word_count.most_common())  # [('hello', 3), ('world', 1), ('python', 1)]

# defaultdict - dictionary ที่มี default value
dd = defaultdict(list)
dd['fruits'].append('apple')
dd['vegetables'].append('carrot')
print(dd)  # defaultdict(<class 'list'>, {'fruits': ['apple'], 'vegetables': ['carrot']})

# deque - queue สองทิศทาง
dq = deque([1, 2, 3])
dq.append(4)        # เพิ่มท้าย
dq.appendleft(0)    # เพิ่มหัว
dq.pop()            # ลบท้าย
dq.popleft()        # ลบหัว

# namedtuple - tuple ที่มีชื่อ
Point = namedtuple('Point', ['x', 'y'])
p = Point(10, 20)
print(p.x, p.y)     # 10 20

# OrderedDict - dictionary เรียงลำดับ (Python 3.7+ dict เรียงอยู่แล้ว)
od = OrderedDict([('a', 1), ('b', 2), ('c', 3)])
```

---

## 🎯 Third-Party Libraries

### การติดตั้ง Packages
```bash
# ติดตั้ง package
pip install numpy pandas matplotlib

# ติดตั้งจาก requirements.txt
pip install -r requirements.txt

# ติดตั้งเวอร์ชันเฉพาะ
pip install numpy==1.21.0

# ติดตั้งจาก GitHub
pip install git+https://github.com/user/repo.git

# อัปเกรด package
pip install --upgrade numpy

# ถอนการติดตั้ง
pip uninstall numpy

# ดูรายการ packages ที่ติดตั้ง
pip list
pip freeze > requirements.txt
```

### NumPy - การคำนวณตัวเลข
```python
import numpy as np

# สร้าง arrays
arr1 = np.array([1, 2, 3, 4, 5])
arr2 = np.array([[1, 2, 3], [4, 5, 6]])

# สร้าง arrays พิเศษ
zeros = np.zeros((3, 3))
ones = np.ones((2, 4))
random_arr = np.random.rand(3, 3)
range_arr = np.arange(0, 10, 2)

# การคำนวณ
result = arr1 + 10              # [11 12 13 14 15]
result = arr1 * 2               # [ 2  4  6  8 10]
result = np.sqrt(arr1)          # [1. 1.414 1.732 2. 2.236]

# สถิติ
mean = np.mean(arr1)
std = np.std(arr1)
median = np.median(arr1)

# Matrix operations
matrix1 = np.array([[1, 2], [3, 4]])
matrix2 = np.array([[5, 6], [7, 8]])
dot_product = np.dot(matrix1, matrix2)
```

### Pandas - การจัดการข้อมูล
```python
import pandas as pd

# สร้าง DataFrame
data = {
    'name': ['สมชาย', 'วิชัย', 'มานี'],
    'age': [25, 30, 28],
    'city': ['กรุงเทพฯ', 'เชียงใหม่', 'ภูเก็ต']
}
df = pd.DataFrame(data)

# อ่านข้อมูล
# df = pd.read_csv('data.csv')
# df = pd.read_excel('data.xlsx')

# ดูข้อมูล
print(df.head())              # 5 แถวแรก
print(df.info())              # ข้อมูล DataFrame
print(df.describe())          # สถิติพื้นฐาน

# การเลือกข้อมูล
names = df['name']            # Column เดียว
subset = df[['name', 'age']] # หลาย columns
filtered = df[df['age'] > 25] # Filter

# การจัดการข้อมูล
df['age_group'] = df['age'].apply(lambda x: 'young' if x < 30 else 'old')
df['full_name'] = df['name'] + ' (' + df['city'] + ')'

# การจัดกลุ่ม
grouped = df.groupby('city')['age'].mean()

# บันทึกข้อมูล
# df.to_csv('output.csv', index=False)
# df.to_excel('output.xlsx', index=False)
```

### Matplotlib - การสร้างกราฟ
```python
import matplotlib.pyplot as plt
import numpy as np

# ข้อมูล
x = np.linspace(0, 10, 100)
y1 = np.sin(x)
y2 = np.cos(x)

# สร้างกราฟ
plt.figure(figsize=(10, 6))
plt.plot(x, y1, label='sin(x)', color='blue', linewidth=2)
plt.plot(x, y2, label='cos(x)', color='red', linewidth=2)

# ตกแต่งกราฟ
plt.title('Sin and Cos Functions')
plt.xlabel('X')
plt.ylabel('Y')
plt.legend()
plt.grid(True, alpha=0.3)

# บันทึกกราฟ
plt.savefig('plot.png', dpi=300, bbox_inches='tight')
plt.show()

# กราฟแท่ง
categories = ['A', 'B', 'C', 'D']
values = [23, 45, 56, 78]

plt.figure(figsize=(8, 5))
plt.bar(categories, values, color=['red', 'green', 'blue', 'orange'])
plt.title('Bar Chart Example')
plt.xlabel('Categories')
plt.ylabel('Values')
plt.show()
```

---

## 🎪 การจัดการ Dependencies

### requirements.txt
```txt
# requirements.txt
numpy==1.21.0
pandas>=1.3.0
matplotlib>=3.5.0,<4.0.0
requests==2.28.1
scikit-learn>=1.0.0
```

### Virtual Environments
```bash
# สร้าง virtual environment
python -m venv myenv

# Activate (Windows)
myenv\Scripts\activate

# Activate (Linux/Mac)
source myenv/bin/activate

# ติดตั้ง packages
pip install -r requirements.txt

# Deactivate
deactivate
```

### pyproject.toml (Modern Python)
```toml
[build-system]
requires = ["setuptools>=45", "wheel"]
build-backend = "setuptools.build_meta"

[project]
name = "my-package"
version = "1.0.0"
description = "My awesome package"
dependencies = [
    "numpy>=1.21.0",
    "pandas>=1.3.0",
    "matplotlib>=3.5.0"
]

[project.optional-dependencies]
dev = [
    "pytest>=6.0",
    "black>=22.0",
    "flake8>=4.0"
]
```

---

## 🛠️ การสร้าง Package ของตัวเอง

### โครงสร้าง Package
```
my_awesome_package/
├── pyproject.toml
├── README.md
├── LICENSE
├── my_awesome_package/
│   ├── __init__.py
│   ├── module1.py
│   ├── module2.py
│   └── subpackage/
│       ├── __init__.py
│       └── module3.py
└── tests/
    ├── __init__.py
    ├── test_module1.py
    └── test_module2.py
```

### pyproject.toml
```toml
[build-system]
requires = ["setuptools>=45", "wheel"]
build-backend = "setuptools.build_meta"

[project]
name = "my-awesome-package"
version = "0.1.0"
description = "An awesome Python package"
readme = "README.md"
requires-python = ">=3.8"
license = {text = "MIT"}
authors = [
    {name = "Your Name", email = "your.email@example.com"}
]
keywords = ["python", "package", "awesome"]
classifiers = [
    "Development Status :: 3 - Alpha",
    "Intended Audience :: Developers",
    "License :: OSI Approved :: MIT License",
    "Programming Language :: Python :: 3",
    "Programming Language :: Python :: 3.8",
    "Programming Language :: Python :: 3.9",
    "Programming Language :: Python :: 3.10",
    "Programming Language :: Python :: 3.11",
]
dependencies = [
    "requests>=2.25.0",
    "numpy>=1.20.0"
]

[project.optional-dependencies]
dev = [
    "pytest>=6.0",
    "pytest-cov>=2.0",
    "black>=22.0",
    "flake8>=4.0"
]

[project.urls]
Homepage = "https://github.com/yourusername/my-awesome-package"
Documentation = "https://my-awesome-package.readthedocs.io"
Repository = "https://github.com/yourusername/my-awesome-package.git"
"Bug Tracker" = "https://github.com/yourusername/my-awesome-package/issues"

[project.scripts]
my-tool = "my_awesome_package.cli:main"

[tool.setuptools.packages.find]
where = ["."]
include = ["my_awesome_package*"]
exclude = ["tests*"]
```

### การ Build และ Publish
```bash
# Install build tools
pip install build twine

# Build package
python -m build

# Test locally
pip install dist/my_awesome_package-0.1.0-py3-none-any.whl

# Upload to PyPI (test)
twine upload --repository testpypi dist/*

# Upload to PyPI (production)
twine upload dist/*
```

---

## 💡 Best Practices

### 1. การ Import อย่างเป็นระเบียบ
```python
# ✅ ดี: Import ตามลำดับมาตรฐาน
# 1. Standard library
import os
import sys
from datetime import datetime

# 2. Third-party libraries
import numpy as np
import pandas as pd
from matplotlib import pyplot as plt

# 3. Local modules
from my_package import utils
from my_package.utils import helper_function

# ❌ ไม่ดี: สลับกัน
import numpy as np
import os
from my_package import utils
import pandas as pd
```

### 2. ใช้ Virtual Environments
```python
# ✅ ดี: แยก environments สำหรับโปรเจคต์ต่างๆ
project1_env/
project2_env/
project3_env/

# ❌ ไม่ดี: ติดตั้งทุกอย่างใน global Python
pip install numpy pandas matplotlib tensorflow django flask
```

### 3. จัดการ Dependencies อย่างเป็นระบบ
```python
# ✅ ดี: ใช้ requirements.txt หรือ pyproject.toml
pip freeze > requirements.txt
git add requirements.txt
git commit -m "Add requirements"

# ❌ ไม่ดี: ไม่มีการจัดการ dependencies
# จำเป็นต้อง install ทีละตัวทุกครั้ง
pip install numpy
pip install pandas
pip install matplotlib
```

### 4. ใช้ Type Hints กับ Modules
```python
# my_module.py
from typing import List, Dict, Optional, Union

def process_data(data: List[Dict[str, Union[str, int]]]) -> Dict[str, int]:
    """ประมวลผลข้อมูล"""
    result = {}
    for item in data:
        key = item.get('key', '')
        value = item.get('value', 0)
        if isinstance(value, int):
            result[key] = value
    return result

class DataProcessor:
    """ตัวประมวลผลข้อมูล"""
    
    def __init__(self, config: Optional[Dict[str, str]] = None):
        self.config = config or {}
    
    def process(self, items: List[str]) -> List[str]:
        """ประมวลผลรายการ"""
        return [item.upper() for item in items]
```

---

## 📝 สรุป Modules ที่ใช้บ่อย

| Category | Modules | การใช้งาน |
|----------|---------|-----------|
| **Math** | `math`, `random`, `statistics` | การคำนวณ |
| **DateTime** | `datetime`, `time`, `calendar` | วันที่และเวลา |
| **File System** | `os`, `pathlib`, `shutil`, `glob` | จัดการไฟล์ |
| **Data Structures** | `collections`, `heapq`, `bisect` | โครงสร้างข้อมูล |
| **Web** | `urllib`, `http`, `requests` | HTTP requests |
| **JSON/XML** | `json`, `xml`, `csv` | การจัดการข้อมูล |
| **Testing** | `unittest`, `pytest`, `doctest` | การทดสอบ |
| **Logging** | `logging` | การบันทึก |
| **Threading** | `threading`, `multiprocessing` | การทำงานขนาน |
| **Database** | `sqlite3`, `pickle` | ฐานข้อมูล |

---

## 🎯 แบบฝึกหัด

### แบบฝึกหัด 1: สร้าง Module
```python
# สร้าง math_operations.py พร้อมฟังก์ชันคำนวณ
# แล้ว import มาใช้ใน main.py
```

### แบบฝึกหัด 2: สร้าง Package
```python
# สร้าง text_processing package มี modules:
# - cleaner.py (ทำความสะอาดข้อความ)
# - analyzer.py (วิเคราะห์ข้อความ)
# - formatter.py (จัดรูปแบบ)
```

### แบบฝึกหัด 3: ใช้ Third-Party Libraries
```python
# ใช้ pandas อ่านข้อมูล CSV
# ใช้ matplotlib สร้างกราฟ
# ใช้ numpy คำนวณสถิติ
```

---

*สร้างเมื่อ: 4 มีนาคม 2026*  
*ปรับปรุงล่าสุด: 4 มีนาคม 2026*
