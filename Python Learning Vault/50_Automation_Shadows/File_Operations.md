# File Operations - การอ่าน-เขียนโลกภายนอก

> 📖 **สรุปบทเรียน (Ch7):** [สรุปบทเรียน_Ch7_การจัดการไฟล์](../60_Learning_Walkthroughs/สรุปบทเรียน_Ch7_การจัดการไฟล์.md) — แนวคิด File Handle, `\n`, Open/Read/Write/Close

## 📁 แนวคิดพื้นฐาน

File Operations คือ **การทำงานกับไฟล์** บนระบบคอมพิวเตอร์ ทั้งการอ่าน เขียน สร้าง ลบ และจัดการไฟล์ต่างๆ เพื่อเก็บข้อมูลถาวรและแลกเปลี่ยนข้อมูลกับโปรแกรมอื่น

```python
# การอ่านไฟล์พื้นฐาน
with open('filename.txt', 'r', encoding='utf-8') as file:
    content = file.read()

# การเขียนไฟล์พื้นฐาน
with open('output.txt', 'w', encoding='utf-8') as file:
    file.write("Hello, World!")
```

---

## 📑 ในหน้านี้ (สารบัญ)

- [[#📝 การอ่านและเขียนไฟล์พื้นฐาน]] · [[#🎪 การจัดการไฟล์ขั้นสูง]] · [[#🛠️ การทำงานกับรูปแบบไฟล์ต่างๆ]]
- [[#🎯 การใช้งานจริง]] · [[#🎪 การจัดการ Error และ Validation]] · [[#💡 Best Practices]] · [[#📝 สรุป File Operations]] · [[#🎯 แบบฝึกหัด]]

---

## 📝 การอ่านและเขียนไฟล์พื้นฐาน

### การเปิดไฟล์
```python
# โหมดการเปิดไฟล์
modes = {
    'r': 'read (อ่าน)',           # อ่านเท่านั้น
    'w': 'write (เขียน)',         # เขียนทับ (สร้างใหม่ถ้าไม่มี)
    'a': 'append (เพิ่ม)',        # เพิ่มต่อท้าย
    'r+': 'read+write (อ่าน+เขียน)', # อ่านและเขียน
    'x': 'create (สร้าง)',         # สร้างใหม่เท่านั้น (error ถ้ามีอยู่)
}

# แบบดั้งเดิม (ต้องปิดเอง)
file = open('data.txt', 'r', encoding='utf-8')
content = file.read()
file.close()

# แบบแนะนำ (ปิดอัตโนมัติ)
with open('data.txt', 'r', encoding='utf-8') as file:
    content = file.read()
    # file.close() ไม่ต้องทำ
```

### การอ่านไฟล์
```python
# อ่านทั้งไฟล์
with open('data.txt', 'r', encoding='utf-8') as file:
    content = file.read()
    print(content)

# อ่านทีละบรรทัด
with open('data.txt', 'r', encoding='utf-8') as file:
    for line in file:
        print(line.strip())  # ตัด \n ออก

# อ่านเป็น list ของบรรทัด
with open('data.txt', 'r', encoding='utf-8') as file:
    lines = file.readlines()
    for i, line in enumerate(lines):
        print(f"Line {i+1}: {line.strip()}")

# อ่านทีละบรรทัดพร้อมเลขบรรทัด
with open('data.txt', 'r', encoding='utf-8') as file:
    for line_num, line in enumerate(file, 1):
        print(f"{line_num}: {line.strip()}")

# อ่านไฟล์ขนาดใหญ่ทีละช่วง
def read_large_file(filename, chunk_size=1024):
    """อ่านไฟล์ขนาดใหญ่ทีละช่วง"""
    with open(filename, 'r', encoding='utf-8') as file:
        while True:
            chunk = file.read(chunk_size)
            if not chunk:
                break
            yield chunk

# การใช้งาน
for chunk in read_large_file('large_file.txt'):
    process_chunk(chunk)
```

### การเขียนไฟล์
```python
# เขียนทั้งไฟล์ (ทับของเก่า)
with open('output.txt', 'w', encoding='utf-8') as file:
    file.write("Hello, World!\n")
    file.write("สวัสดีภาษาไทย\n")

# เพิ่มข้อมูลต่อท้าย
with open('output.txt', 'a', encoding='utf-8') as file:
    file.write("เพิ่มบรรทัดใหม่\n")

# เขียนหลายบรรทัด
lines = ["บรรทัดที่ 1\n", "บรรทัดที่ 2\n", "บรรทัดที่ 3\n"]
with open('lines.txt', 'w', encoding='utf-8') as file:
    file.writelines(lines)

# เขียนโดยใช้ print
with open('output.txt', 'w', encoding='utf-8') as file:
    print("Hello, World!", file=file)
    print("สวัสดีภาษาไทย", file=file)

# การเขียนข้อมูลต่างๆ
data = {
    "name": "สมชาย",
    "age": 25,
    "city": "กรุงเทพฯ"
}

with open('data.txt', 'w', encoding='utf-8') as file:
    for key, value in data.items():
        file.write(f"{key}: {value}\n")
```

---

## 🎪 การจัดการไฟล์ขั้นสูง

### การทำงานกับ Binary Files
```python
# อ่านไฟล์ binary
with open('image.jpg', 'rb') as file:
    image_data = file.read()

# เขียนไฟล์ binary
with open('copy.jpg', 'wb') as file:
    file.write(image_data)

# คัดลอกไฟล์ binary ทีละช่วย
def copy_binary_file(source, destination, chunk_size=1024):
    """คัดลอกไฟล์ binary"""
    with open(source, 'rb') as src, open(destination, 'wb') as dst:
        while True:
            chunk = src.read(chunk_size)
            if not chunk:
                break
            dst.write(chunk)

# อ่านข้อมูลจากไฟล์ pickle
import pickle

data = {"name": "สมชาย", "scores": [85, 90, 78]}

# บันทึก object
with open('data.pkl', 'wb') as file:
    pickle.dump(data, file)

# อ่าน object
with open('data.pkl', 'rb') as file:
    loaded_data = pickle.load(file)
```

### การจัดการ Path ด้วย pathlib
```python
from pathlib import Path
import os

# สร้าง Path objects
current_dir = Path.cwd()
file_path = Path("data/input.txt")
directory_path = Path("data/output")

# การทำงานกับ paths
print(file_path.name)        # input.txt
print(file_path.stem)        # input
print(file_path.suffix)      # .txt
print(file_path.parent)      # data
print(file_path.exists())    # True/False
print(file_path.is_file())   # True/False
print(file_path.is_dir())    # True/False

# สร้างโฟลเดอร์
directory_path.mkdir(parents=True, exist_ok=True)

# อ่านเขียนไฟล์ด้วย pathlib
content = file_path.read_text(encoding='utf-8')
file_path.write_text("Hello, World!", encoding='utf-8')

# เขียน binary
binary_data = b'\x00\x01\x02'
file_path.write_bytes(binary_data)
loaded_binary = file_path.read_bytes()

# การค้นหาไฟล์
txt_files = list(Path('.').glob('*.txt'))      # หา .txt ในปัจจุบัน
all_files = list(Path('.').rglob('*'))        # หาทุกอย่าง (recursive)
python_files = list(Path('.').glob('**/*.py')) # หา .py ทุกระดับ

# การรวม path
data_dir = Path("data")
file_path = data_dir / "input.txt"  # แทน os.path.join
```

### การจัดการ Context Managers
```python
# Custom context manager
class FileManager:
    """Context manager สำหรับจัดการไฟล์"""
    
    def __init__(self, filename, mode='r', encoding='utf-8'):
        self.filename = filename
        self.mode = mode
        self.encoding = encoding
        self.file = None
    
    def __enter__(self):
        try:
            self.file = open(self.filename, self.mode, encoding=self.encoding)
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
            print(f"เกิดข้อผิดพลาด: {exc_val}")
            return False
        return True

# การใช้งาน
try:
    with FileManager('data.txt', 'r') as file:
        content = file.read()
        print(content)
except Exception as e:
    print(f"Error: {e}")

# Context manager สำหรับ backup
class FileBackup:
    """Context manager สำหรับ backup ไฟล์ก่อนแก้ไข"""
    
    def __init__(self, filename):
        self.filename = filename
        self.backup_filename = f"{filename}.backup"
    
    def __enter__(self):
        if Path(self.filename).exists():
            # สร้าง backup
            import shutil
            shutil.copy2(self.filename, self.backup_filename)
        return self
    
    def __exit__(self, exc_type, exc_val, exc_tb):
        if exc_type is not None:
            # ถ้าเกิด error คืนค่าจาก backup
            if Path(self.backup_filename).exists():
                import shutil
                shutil.move(self.backup_filename, self.filename)
            print("คืนค่าไฟล์จาก backup เนื่องจากเกิดข้อผิดพลาด")
        elif Path(self.backup_filename).exists():
            # ถ้าสำเร็จ ลบ backup
            Path(self.backup_filename).unlink()
        return False

# การใช้งาน
with FileBackup('important.txt'):
    with open('important.txt', 'w') as file:
        file.write("ข้อมูลใหม่")
    # ถ้าเกิด error ที่นี่ จะคืนค่าจาก backup
```

---

## 🛠️ การทำงานกับรูปแบบไฟล์ต่างๆ

### CSV Files
```python
import csv
from pathlib import Path

# เขียน CSV
data = [
    ["ชื่อ", "อายุ", "เมือง"],
    ["สมชาย", "25", "กรุงเทพฯ"],
    ["วิชัย", "30", "เชียงใหม่"],
    ["มานี", "28", "ภูเก็ต"]
]

with open('people.csv', 'w', newline='', encoding='utf-8') as file:
    writer = csv.writer(file)
    writer.writerows(data)

# อ่าน CSV
with open('people.csv', 'r', encoding='utf-8') as file:
    reader = csv.reader(file)
    for row in reader:
        print(row)

# ใช้ DictWriter
with open('people_dict.csv', 'w', newline='', encoding='utf-8') as file:
    fieldnames = ['ชื่อ', 'อายุ', 'เมือง']
    writer = csv.DictWriter(file, fieldnames=fieldnames)
    
    writer.writeheader()
    writer.writerow({'ชื่อ': 'สมชาย', 'อายุ': '25', 'เมือง': 'กรุงเทพฯ'})
    writer.writerow({'ชื่อ': 'วิชัย', 'อายุ': '30', 'เมือง': 'เชียงใหม่'})

# อ่านด้วย DictReader
with open('people_dict.csv', 'r', encoding='utf-8') as file:
    reader = csv.DictReader(file)
    for row in reader:
        print(f"{row['ชื่อ']} ({row['อายุ']}) จาก{row['เมือง']}")
```

### JSON Files
```python
import json
from datetime import datetime

# เขียน JSON
data = {
    "name": "สมชาย",
    "age": 25,
    "city": "กรุงเทพฯ",
    "hobbies": ["อ่านหนังสือ", "เล่นกีฬา"],
    "scores": [85, 90, 78, 92]
}

with open('data.json', 'w', encoding='utf-8') as file:
    json.dump(data, file, indent=2, ensure_ascii=False)

# อ่าน JSON
with open('data.json', 'r', encoding='utf-8') as file:
    loaded_data = json.load(file)
    print(loaded_data)

# จัดการ datetime ใน JSON
class DateTimeEncoder(json.JSONEncoder):
    def default(self, obj):
        if isinstance(obj, datetime):
            return obj.isoformat()
        return super().default(obj)

data_with_date = {
    "name": "สมชาย",
    "created_at": datetime.now()
}

with open('data_with_date.json', 'w', encoding='utf-8') as file:
    json.dump(data_with_date, file, cls=DateTimeEncoder, indent=2, ensure_ascii=False)
```

### XML Files
```python
import xml.etree.ElementTree as ET

# สร้าง XML
root = ET.Element("people")

person1 = ET.SubElement(root, "person")
person1.set("id", "1")
name1 = ET.SubElement(person1, "name")
name1.text = "สมชาย"
age1 = ET.SubElement(person1, "age")
age1.text = "25"

person2 = ET.SubElement(root, "person")
person2.set("id", "2")
name2 = ET.SubElement(person2, "name")
name2.text = "วิชัย"
age2 = ET.SubElement(person2, "age")
age2.text = "30"

# เขียน XML
tree = ET.ElementTree(root)
tree.write("people.xml", encoding='utf-8', xml_declaration=True)

# อ่าน XML
tree = ET.parse("people.xml")
root = tree.getroot()

for person in root.findall("person"):
    person_id = person.get("id")
    name = person.find("name").text
    age = person.find("age").text
    print(f"ID: {person_id}, ชื่อ: {name}, อายุ: {age}")
```

### Excel Files (pandas)
```python
import pandas as pd

# สร้าง DataFrame
data = {
    "ชื่อ": ["สมชาย", "วิชัย", "มานี"],
    "อายุ": [25, 30, 28],
    "เมือง": ["กรุงเทพฯ", "เชียงใหม่", "ภูเก็ต"],
    "เงินเดือน": [25000, 30000, 28000]
}

df = pd.DataFrame(data)

# เขียน Excel
df.to_excel("people.xlsx", index=False, sheet_name="ข้อมูลผู้ใช้")

# อ่าน Excel
df_read = pd.read_excel("people.xlsx", sheet_name="ข้อมูลผู้ใช้")
print(df_read)

# เขียนหลาย sheets
with pd.ExcelWriter("multi_sheet.xlsx", engine='openpyxl') as writer:
    df.to_excel(writer, sheet_name="ผู้ใช้", index=False)
    df.to_excel(writer, sheet_name="Backup", index=False)
```

---

## 🎯 การใช้งานจริง

### การสร้าง Log System
```python
import logging
from datetime import datetime
from pathlib import Path

class Logger:
    """คลาสสำหรับบันทึก log"""
    
    def __init__(self, log_file="app.log"):
        self.log_file = Path(log_file)
        self.log_file.parent.mkdir(parents=True, exist_ok=True)
        
        # ตั้งค่า logging
        logging.basicConfig(
            level=logging.INFO,
            format='%(asctime)s - %(levelname)s - %(message)s',
            handlers=[
                logging.FileHandler(self.log_file, encoding='utf-8'),
                logging.StreamHandler()  # แสดงบน console ด้วย
            ]
        )
        self.logger = logging.getLogger(__name__)
    
    def info(self, message):
        """บันทึกข้อมูล info"""
        self.logger.info(message)
    
    def error(self, message):
        """บันทึกข้อมูล error"""
        self.logger.error(message)
    
    def warning(self, message):
        """บันทึกข้อมูล warning"""
        self.logger.warning(message)
    
    def debug(self, message):
        """บันทึกข้อมูล debug"""
        self.logger.debug(message)

# การใช้งาน
logger = Logger("logs/app.log")
logger.info("แอปพลิเคชันเริ่มทำงาน")
logger.warning("พบข้อมูลที่ไม่สมบูรณ์")
logger.error("เกิดข้อผิดพลาดในการเชื่อมต่อฐานข้อมูล")
```

### การสำรองข้อมูล
```python
import shutil
import zipfile
from datetime import datetime
from pathlib import Path

class BackupManager:
    """จัดการการสำรองข้อมูล"""
    
    def __init__(self, source_dir, backup_dir):
        self.source_dir = Path(source_dir)
        self.backup_dir = Path(backup_dir)
        self.backup_dir.mkdir(parents=True, exist_ok=True)
    
    def backup_files(self, pattern="*"):
        """สำรองไฟล์ตาม pattern"""
        timestamp = datetime.now().strftime("%Y%m%d_%H%M%S")
        backup_name = f"backup_{timestamp}"
        backup_path = self.backup_dir / backup_name
        backup_path.mkdir(exist_ok=True)
        
        files_copied = 0
        for file_path in self.source_dir.glob(pattern):
            if file_path.is_file():
                dest_path = backup_path / file_path.name
                shutil.copy2(file_path, dest_path)
                files_copied += 1
                print(f"คัดลอก: {file_path} -> {dest_path}")
        
        print(f"สำรองข้อมูลสำเร็จ {files_copied} ไฟล์")
        return backup_path
    
    def create_zip_backup(self, pattern="*"):
        """สร้าง backup เป็น zip"""
        timestamp = datetime.now().strftime("%Y%m%d_%H%M%S")
        zip_filename = f"backup_{timestamp}.zip"
        zip_path = self.backup_dir / zip_filename
        
        with zipfile.ZipFile(zip_path, 'w', zipfile.ZIP_DEFLATED) as zipf:
            for file_path in self.source_dir.glob(pattern):
                if file_path.is_file():
                    zipf.write(file_path, file_path.name)
        
        print(f"สร้าง zip backup: {zip_path}")
        return zip_path
    
    def restore_backup(self, backup_path):
        """คืนค่าข้อมูลจาก backup"""
        backup_path = Path(backup_path)
        
        if backup_path.is_file() and backup_path.suffix == '.zip':
            # คืนจาก zip
            with zipfile.ZipFile(backup_path, 'r') as zipf:
                zipf.extractall(self.source_dir)
        elif backup_path.is_dir():
            # คืนจากโฟลเดอร์
            for file_path in backup_path.glob("*"):
                if file_path.is_file():
                    dest_path = self.source_dir / file_path.name
                    shutil.copy2(file_path, dest_path)
        
        print(f"คืนค่าข้อมูลจาก: {backup_path}")

# การใช้งาน
backup_manager = BackupManager("data", "backups")
backup_manager.backup_files("*.txt")
backup_manager.create_zip_backup("*.json")
```

### การประมวลผลไฟล์ขนาดใหญ่
```python
import mmap
from pathlib import Path

def process_large_file(filename, processor_func, chunk_size=1024*1024):
    """ประมวลผลไฟล์ขนาดใหญ่"""
    file_path = Path(filename)
    
    if not file_path.exists():
        raise FileNotFoundError(f"ไม่พบไฟล์: {filename}")
    
    file_size = file_path.stat().st_size
    processed = 0
    
    with open(filename, 'r', encoding='utf-8') as file:
        with mmap.mmap(file.fileno(), 0, access=mmap.ACCESS_READ) as mmapped_file:
            while processed < file_size:
                chunk = mmapped_file.read(chunk_size)
                if not chunk:
                    break
                
                # แปลง bytes เป็น string (ถ้าจำเป็น)
                if isinstance(chunk, bytes):
                    chunk = chunk.decode('utf-8', errors='ignore')
                
                processor_func(chunk)
                processed += len(chunk)
                
                # แสดงความคืบหน้า
                progress = (processed / file_size) * 100
                print(f"\rประมวลผล: {progress:.1f}%", end='', flush=True)
    
    print(f"\nประมวลผลเสร็จสิ้น: {filename}")

def count_words_in_large_file(filename):
    """นับคำในไฟล์ขนาดใหญ่"""
    word_count = 0
    
    def count_chunk(chunk):
        nonlocal word_count
        words = chunk.split()
        word_count += len(words)
    
    process_large_file(filename, count_chunk)
    return word_count

# การใช้งาน
total_words = count_words_in_large_file("large_text_file.txt")
print(f"จำนวนคำทั้งหมด: {total_words}")
```

---

## 🎪 การจัดการ Error และ Validation

### การตรวจสอบไฟล์
```python
import os
from pathlib import Path

class FileValidator:
    """ตรวจสอบความถูกต้องของไฟล์"""
    
    @staticmethod
    def validate_file_exists(file_path):
        """ตรวจสอบว่ามีไฟล์หรือไม่"""
        path = Path(file_path)
        if not path.exists():
            raise FileNotFoundError(f"ไม่พบไฟล์: {file_path}")
        return True
    
    @staticmethod
    def validate_file_readable(file_path):
        """ตรวจสอบว่าอ่านไฟล์ได้หรือไม่"""
        path = Path(file_path)
        FileValidator.validate_file_exists(path)
        
        if not os.access(path, os.R_OK):
            raise PermissionError(f"ไม่มีสิทธิ์อ่านไฟล์: {file_path}")
        return True
    
    @staticmethod
    def validate_file_writable(file_path):
        """ตรวจสอบว่าเขียนไฟล์ได้หรือไม่"""
        path = Path(file_path)
        
        if path.exists():
            if not os.access(path, os.W_OK):
                raise PermissionError(f"ไม่มีสิทธิ์เขียนไฟล์: {file_path}")
        else:
            # ตรวจสอบว่าสามารถสร้างไฟล์ในโฟลเดอร์ได้หรือไม่
            parent = path.parent
            if not os.access(parent, os.W_OK):
                raise PermissionError(f"ไม่มีสิทธิ์สร้างไฟล์ใน: {parent}")
        
        return True
    
    @staticmethod
    def validate_file_size(file_path, max_size_mb=None):
        """ตรวจสอบขนาดไฟล์"""
        path = Path(file_path)
        FileValidator.validate_file_exists(path)
        
        size_bytes = path.stat().st_size
        
        if max_size_mb:
            max_bytes = max_size_mb * 1024 * 1024
            if size_bytes > max_bytes:
                raise ValueError(f"ไฟล์ใหญ่เกินไป: {size_bytes} bytes (สูงสุด {max_bytes} bytes)")
        
        return size_bytes
    
    @staticmethod
    def validate_file_extension(file_path, allowed_extensions):
        """ตรวจสอบนามสกุลไฟล์"""
        path = Path(file_path)
        extension = path.suffix.lower()
        
        if extension not in [ext.lower() for ext in allowed_extensions]:
            raise ValueError(f"นามสกุลไฟล์ไม่อนุญาต: {extension} (อนุญาต: {allowed_extensions})")
        
        return True

# การใช้งาน
try:
    FileValidator.validate_file_exists("data.txt")
    FileValidator.validate_file_readable("data.txt")
    FileValidator.validate_file_extension("data.txt", [".txt", ".csv"])
    FileValidator.validate_file_size("data.txt", max_size_mb=10)
except Exception as e:
    print(f"Validation error: {e}")
```

### การจัดการ File Operations อย่างปลอดภัย
```python
import os
import tempfile
from pathlib import Path
from contextlib import contextmanager

@contextmanager
def safe_file_operation(filename, mode='w', encoding='utf-8'):
    """Context manager สำหรับการทำงานกับไฟล์อย่างปลอดภัย"""
    temp_file = None
    original_file = Path(filename)
    
    try:
        # สร้างไฟล์ชั่วคราว
        temp_file = tempfile.NamedTemporaryFile(
            mode=mode, 
            encoding=encoding, 
            delete=False,
            suffix='.tmp'
        )
        
        yield temp_file
        
        # ถ้าสำเร็จ ย้ายไฟล์ชั่วคราวไปที่ตำแหน่งจริง
        if 'w' in mode or 'a' in mode:
            if original_file.exists():
                # สร้าง backup ก่อน
                backup_file = original_file.with_suffix('.bak')
                shutil.copy2(original_file, backup_file)
            
            # ย้ายไฟล์
            shutil.move(temp_file.name, filename)
    
    except Exception as e:
        # ถ้าเกิด error ลบไฟล์ชั่วคราว
        if temp_file and os.path.exists(temp_file.name):
            os.unlink(temp_file.name)
        raise e
    finally:
        # ทำความสะอาด
        if temp_file and os.path.exists(temp_file.name):
            os.unlink(temp_file.name)

# การใช้งาน
try:
    with safe_file_operation("important.txt", 'w') as file:
        file.write("ข้อมูลสำคัญ")
        # ถ้าเกิด error ที่นี่ ไฟล์ต้นฉบับจะไม่ถูกแก้ไข
        raise ValueError("Simulated error")
except Exception as e:
    print(f"Error: {e}")
    # ไฟล์ต้นฉบับยังคงอยู่เดิม
```

---

## 💡 Best Practices

### 1. ใช้ with statement เสมอ
```python
# ✅ ดี: ใช้ with statement
with open('data.txt', 'r', encoding='utf-8') as file:
    content = file.read()

# ❌ ไม่ดี: ลืมปิดไฟล์
file = open('data.txt', 'r', encoding='utf-8')
content = file.read()
# file.close() ลืมปิด!
```

### 2. ระบุ encoding เสมอ
```python
# ✅ ดี: ระบุ encoding
with open('data.txt', 'r', encoding='utf-8') as file:
    content = file.read()

# ❌ ไม่ดี: ใช้ default encoding (อาจแตกต่างกันในแต่ละระบบ)
with open('data.txt', 'r') as file:
    content = file.read()
```

### 3. ใช้ pathlib แทน os.path
```python
# ✅ ดี: ใช้ pathlib
from pathlib import Path
file_path = Path("data") / "input.txt"

# ❌ ไม่ดี: ใช้ os.path
import os
file_path = os.path.join("data", "input.txt")
```

### 4. ตรวจสอบไฟล์ก่อนใช้งาน
```python
# ✅ ดี: ตรวจสอบก่อน
file_path = Path("data.txt")
if file_path.exists():
    content = file_path.read_text(encoding='utf-8')
else:
    print("ไฟล์ไม่มีอยู่")

# ❌ ไม่ดี: ไม่ตรวจสอบ
content = Path("data.txt").read_text(encoding='utf-8')  # Error!
```

---

## 📝 สรุป File Operations

| การดำเนินการ | Method | ตัวอย่าง |
|--------------|--------|---------|
| **อ่านไฟล์** | `read()`, `readline()`, `readlines()` | `file.read()` |
| **เขียนไฟล์** | `write()`, `writelines()` | `file.write("Hello")` |
| **ตรวจสอบ** | `exists()`, `is_file()`, `is_dir()` | `Path("file.txt").exists()` |
| **สร้างโฟลเดอร์** | `mkdir()`, `makedirs()` | `Path("dir").mkdir()` |
| **คัดลอกไฟล์** | `shutil.copy2()` | `shutil.copy2(src, dst)` |
| **ย้ายไฟล์** | `shutil.move()` | `shutil.move(src, dst)` |
| **ลบไฟล์** | `unlink()`, `rmtree()` | `Path("file.txt").unlink()` |

---

## 🎯 แบบฝึกหัด

### แบบฝึกหัด 1: File Processor
```python
# เขียนฟังก์ชันประมวลผลไฟล์ CSV และสร้างสรุป
def process_csv_file(input_file, output_file):
    pass
```

### แบบฝึกหัด 2: Log Analyzer
```python
# เขียนฟังก์ชันวิเคราะห์ log file และหา error
def analyze_log_file(log_file):
    pass
```

### แบบฝึกหัด 3: Backup System
```python
# เขียนคลาสสำหรับ backup ไฟล์อัตโนมัติ
class AutoBackup:
    pass
```

---

*สร้างเมื่อ: 4 มีนาคม 2026*  
*ปรับปรุงล่าสุด: 4 มีนาคม 2026*
