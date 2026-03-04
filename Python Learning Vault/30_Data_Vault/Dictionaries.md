# Dictionaries - โครงสร้างความหมายและ Key-Value

## 🔑 แนวคิดพื้นฐาน

Dictionary คือ **โครงสร้างข้อมูลแบบ Key-Value** ที่เก็บข้อมูลคู่ของ key และ value ไว้ด้วยกัน คล้ายกับพจนานุกรมที่ค้นหาคำ (key) เพื่อหาความหมาย (value)

```python
# โครงสร้างพื้นฐาน
person = {
    "name": "สมชาย",
    "age": 25,
    "city": "กรุงเทพฯ"
}

# เข้าถึงข้อมูล
print(person["name"])     # สมชาย
print(person.get("age"))  # 25
```

---

## 📝 การสร้างและพื้นฐาน Dictionary

### การสร้าง Dictionary
```python
# วิธีต่างๆ ในการสร้าง
empty_dict = {}
person = {"name": "สมชาย", "age": 25}
mixed = {"name": "วิชัย", "age": 30, "scores": [85, 90, 78]}

# ใช้ dict() constructor
person1 = dict(name="มานี", age=28, city="เชียงใหม่")
person2 = dict([("name", "สมชาย"), ("age", 25)])

# จาก list ของ tuples
data = [("a", 1), ("b", 2), ("c", 3)]
my_dict = dict(data)

# Dictionary comprehension
squares = {x: x**2 for x in range(5)}  # {0: 0, 1: 1, 2: 4, 3: 9, 4: 16}
```

### การเข้าถึงข้อมูล
```python
person = {
    "name": "สมชาย",
    "age": 25,
    "city": "กรุงเทพฯ",
    "hobbies": ["อ่านหนังสือ", "เล่นกีฬา"]
}

# การเข้าถึงแบบตรงๆ
print(person["name"])        # สมชาย
print(person["hobbies"][0])  # อ่านหนังสือ

# การเข้าถึงแบบปลอดภัย
print(person.get("age"))     # 25
print(person.get("salary"))  # None
print(person.get("salary", 0))  # 0 (default value)

# การตรวจสอบว่ามี key หรือไม่
if "name" in person:
    print("มีข้อมูลชื่อ")

if "salary" not in person:
    print("ไม่มีข้อมูลเงินเดือน")
```

### การแก้ไขและเพิ่มข้อมูล
```python
person = {"name": "สมชาย", "age": 25}

# แก้ไขค่าที่มีอยู่
person["age"] = 26

# เพิ่ม key-value ใหม่
person["city"] = "กรุงเทพฯ"
person["salary"] = 50000

# ใช้ update() เพิ่มหลายค่าพร้อมกัน
person.update({
    "department": "IT",
    "position": "Developer",
    "experience": 3
})

# ใช้ setdefault() (เพิ่มถ้าไม่มี)
person.setdefault("email", "somchai@email.com")
person.setdefault("name", "วิชัย")  # ไม่เปลี่ยนเพราะมีอยู่แล้ว
```

### การลบข้อมูล
```python
person = {
    "name": "สมชาย",
    "age": 25,
    "city": "กรุงเทพฯ",
    "salary": 50000
}

# ลบตาม key
del person["salary"]

# ลบและคืนค่า
removed_age = person.pop("age")      # 25
removed_city = person.pop("city", "N/A")  # กรุงเทพฯ

# ลบ key สุดท้าย
last_item = person.popitem()        # ("name", "สมชาย")

# ลบทั้งหมด
person.clear()                       # {}
```

---

## 🎪 การดำเนินการขั้นสูง

### การวนลูป Dictionary
```python
person = {
    "name": "สมชาย",
    "age": 25,
    "city": "กรุงเทพฯ",
    "salary": 50000
}

# วนลูป keys (default)
for key in person:
    print(f"{key}: {person[key]}")

# วนลูป keys อย่างชัดเจน
for key in person.keys():
    print(f"Key: {key}")

# วนลูป values
for value in person.values():
    print(f"Value: {value}")

# วนลูป key-value (แนะนำ)
for key, value in person.items():
    print(f"{key}: {value}")

# วนลูปพร้อม index
for i, (key, value) in enumerate(person.items()):
    print(f"{i+1}. {key}: {value}")
```

### Dictionary Comprehension
```python
# พื้นฐาน
squares = {x: x**2 for x in range(5)}  # {0: 0, 1: 1, 2: 4, 3: 9, 4: 16}

# มีเงื่อนไข
even_squares = {x: x**2 for x in range(10) if x % 2 == 0}

# แปลงข้อมูลจาก dictionary อื่น
person = {"name": "สมชาย", "age": 25, "city": "กรุงเทพฯ"}
upper_person = {k.upper(): v for k, v in person.items()}

# กรองและแปลงพร้อมกัน
scores = {"math": 85, "science": 90, "english": 78}
high_scores = {subject: score for subject, score in scores.items() if score >= 80}

# ใช้กับฟังก์ชัน
words = ["hello", "world", "python"]
word_lengths = {word: len(word) for word in words}
```

### การจัดการ Nested Dictionary
```python
# Dictionary ซ้อน Dictionary
company = {
    "name": "Tech Company",
    "employees": {
        "engineering": {
            "frontend": {"count": 5, "lead": "Alice"},
            "backend": {"count": 3, "lead": "Bob"}
        },
        "marketing": {
            "digital": {"count": 2, "lead": "Carol"},
            "content": {"count": 1, "lead": "David"}
        }
    },
    "revenue": {
        "2023": 1000000,
        "2024": 1500000
    }
}

# เข้าถึงข้อมูลซ้อน
frontend_lead = company["employees"]["engineering"]["frontend"]["lead"]
print(f"Frontend Lead: {frontend_lead}")

# การเพิ่มข้อมูลซ้อน
company["employees"]["engineering"]["qa"] = {"count": 2, "lead": "Eve"}

# การวนลูป nested
for dept, teams in company["employees"].items():
    print(f"Department: {dept}")
    for team, info in teams.items():
        print(f"  {team}: {info['count']} people, lead: {info['lead']}")
```

---

## 🛠️ การใช้งานจริง

### การจัดการข้อมูลผู้ใช้
```python
class UserManager:
    """จัดการข้อมูลผู้ใช้"""
    
    def __init__(self):
        self.users = {}
        self.next_id = 1
    
    def add_user(self, name, email, age):
        """เพิ่มผู้ใช้ใหม่"""
        user_id = f"user_{self.next_id}"
        self.users[user_id] = {
            "name": name,
            "email": email,
            "age": age,
            "created_at": datetime.datetime.now(),
            "active": True
        }
        self.next_id += 1
        return user_id
    
    def get_user(self, user_id):
        """ดึงข้อมูลผู้ใช้"""
        return self.users.get(user_id)
    
    def update_user(self, user_id, **kwargs):
        """อัปเดตข้อมูลผู้ใช้"""
        if user_id in self.users:
            self.users[user_id].update(kwargs)
            return True
        return False
    
    def deactivate_user(self, user_id):
        """ปิดใช้งานผู้ใช้"""
        if user_id in self.users:
            self.users[user_id]["active"] = False
            return True
        return False
    
    def get_active_users(self):
        """ดึงผู้ใช้ที่ใช้งานอยู่"""
        return {uid: info for uid, info in self.users.items() 
                if info["active"]}

# การใช้งาน
manager = UserManager()
user1 = manager.add_user("สมชาย", "somchai@email.com", 25)
user2 = manager.add_user("วิชัย", "wichai@email.com", 30)

manager.update_user(user1, age=26)
active_users = manager.get_active_users()
```

### การจัดการ Configuration
```python
class ConfigManager:
    """จัดการการตั้งค่า"""
    
    def __init__(self, config_file=None):
        self.config = {
            "database": {
                "host": "localhost",
                "port": 5432,
                "name": "myapp",
                "user": "admin",
                "password": "secret"
            },
            "api": {
                "base_url": "https://api.example.com",
                "timeout": 30,
                "retry_count": 3
            },
            "logging": {
                "level": "INFO",
                "file": "app.log",
                "max_size": "10MB"
            }
        }
        
        if config_file:
            self.load_from_file(config_file)
    
    def get(self, key_path, default=None):
        """ดึงค่า config ตาม path"""
        keys = key_path.split(".")
        value = self.config
        
        try:
            for key in keys:
                value = value[key]
            return value
        except (KeyError, TypeError):
            return default
    
    def set(self, key_path, value):
        """ตั้งค่า config ตาม path"""
        keys = key_path.split(".")
        config = self.config
        
        for key in keys[:-1]:
            if key not in config:
                config[key] = {}
            config = config[key]
        
        config[keys[-1]] = value
    
    def load_from_file(self, filename):
        """โหลด config จากไฟล์ JSON"""
        try:
            with open(filename, 'r', encoding='utf-8') as f:
                file_config = json.load(f)
            self._deep_update(self.config, file_config)
        except FileNotFoundError:
            print(f"Config file {filename} not found")
        except json.JSONDecodeError as e:
            print(f"Invalid JSON in config file: {e}")
    
    def _deep_update(self, base_dict, update_dict):
        """อัปเดต dictionary แบบ deep"""
        for key, value in update_dict.items():
            if key in base_dict and isinstance(base_dict[key], dict) and isinstance(value, dict):
                self._deep_update(base_dict[key], value)
            else:
                base_dict[key] = value

# การใช้งาน
config = ConfigManager()
db_host = config.get("database.host", "localhost")
config.set("api.timeout", 60)
```

### การแปลงข้อมูล
```python
def transform_data(data_dict):
    """แปลงข้อมูลจาก dictionary"""
    transformed = {}
    
    # แปลง keys เป็น uppercase
    for key, value in data_dict.items():
        new_key = key.upper().replace(" ", "_")
        
        # แปลงค่าตามประเภท
        if isinstance(value, str):
            new_value = value.strip().upper()
        elif isinstance(value, (int, float)):
            new_value = value
        elif isinstance(value, list):
            new_value = [str(item).upper() for item in value]
        elif isinstance(value, dict):
            new_value = transform_data(value)  # recursion
        else:
            new_value = str(value)
        
        transformed[new_key] = new_value
    
    return transformed

# ตัวอย่างข้อมูล
raw_data = {
    "user name": "สมชาย",
    "age": 25,
    "hobbies": ["อ่านหนังสือ", "เล่นกีฬา"],
    "contact": {
        "email": "somchai@email.com",
        "phone": "081-234-5678"
    }
}

clean_data = transform_data(raw_data)
```

---

## 🎯 การเพิ่มประสิทธิภาพ

### การเลือกโครงสร้างข้อมูล
```python
# ✅ ใช้ dict สำหรับข้อมูลที่มีความหมาย
user_profile = {
    "id": 123,
    "name": "สมชาย",
    "email": "somchai@email.com",
    "preferences": {
        "theme": "dark",
        "notifications": True
    }
}

# ❌ ไม่ดี: ใช้ tuple สำหรับข้อมูลซับซ้อน
# user_data = (123, "สมชาย", "somchai@email.com", "dark", True)
# ไม่รู้ว่าแต่ละตำแหน่งคืออะไร
```

### การใช้ defaultdict
```python
from collections import defaultdict

# กรณีที่ต้องตรวจสอบ key ก่อน
# ❌ ไม่ดี
word_count = {}
for word in ["hello", "world", "hello", "python"]:
    if word in word_count:
        word_count[word] += 1
    else:
        word_count[word] = 1

# ✅ ดีกว่า: ใช้ defaultdict
word_count = defaultdict(int)
for word in ["hello", "world", "hello", "python"]:
    word_count[word] += 1

# กรณี list
# ❌ ไม่ดี
grouped_data = {}
for item in [("A", 1), ("B", 2), ("A", 3)]:
    key, value = item
    if key not in grouped_data:
        grouped_data[key] = []
    grouped_data[key].append(value)

# ✅ ดีกว่า: ใช้ defaultdict
grouped_data = defaultdict(list)
for key, value in [("A", 1), ("B", 2), ("A", 3)]:
    grouped_data[key].append(value)
```

### การใช้ OrderedDict
```python
from collections import OrderedDict

# Python 3.7+ dict เรียงลำดับอยู่แล้ว แต่ OrderedDict มีประโยชน์บางกรณี
# เมื่อต้องการควบคุมลำดับอย่างชัดเจน
config = OrderedDict([
    ("database", {"host": "localhost"}),
    ("api", {"url": "https://api.example.com"}),
    ("logging", {"level": "INFO"})
])

# ย้าย key ไปข้างหน้า/หลัง
config.move_to_end("database", last=False)  # ไปข้างหน้าสุด
config.move_to_end("logging")               # ไปข้างหลังสุด
```

---

## 💡 Best Practices

### 1. ตั้งชื่อ Key ที่ชัดเจน
```python
# ✅ ดี: ชื่อชัดเจน สอดคล้องกัน
user_data = {
    "first_name": "สมชาย",
    "last_name": "ใจดี",
    "email_address": "somchai@email.com",
    "phone_number": "081-234-5678"
}

# ❌ ไม่ดี: ชื่อไม่สอดคล้องกัน
user_data = {
    "name": "สมชาย",
    "email": "somchai@email.com",
    "phone": "081-234-5678",
    "addr": "กรุงเทพฯ"
}
```

### 2. ใช้ Type Hints
```python
from typing import Dict, List, Any, Optional

def process_user_data(user: Dict[str, Any]) -> Dict[str, str]:
    """ประมวลผลข้อมูลผู้ใช้"""
    processed = {}
    
    for key, value in user.items():
        if isinstance(value, str):
            processed[key] = value.upper()
        else:
            processed[key] = str(value)
    
    return processed

def get_user_config() -> Dict[str, Dict[str, Any]]:
    """ดึงค่า config"""
    return {
        "database": {"host": "localhost"},
        "api": {"timeout": 30}
    }
```

### 3. จัดการ Key ที่ไม่มีอย่างปลอดภัย
```python
# ✅ ดี: ใช้ get() พร้อม default
age = user.get("age", 0)
city = user.get("city", "Unknown")

# ✅ ดี: ใช้ setdefault()
user.setdefault("preferences", {})
user["preferences"]["theme"] = "dark"

# ✅ ดี: ใช้ defaultdict สำหรับ grouping
from collections import defaultdict
grouped = defaultdict(list)
grouped["category"].append(item)
```

### 4. หลีกเลี่ยงการแก้ไขระหว่าง Loop
```python
# ❌ อันตราย: แก้ไข dict ระหว่างวนลูป
data = {"a": 1, "b": 2, "c": 3}
for key in data:
    if key == "b":
        del data[key]  # RuntimeError!

# ✅ ดี: สร้าง list ของ keys ที่จะลบ
keys_to_remove = [key for key, value in data.items() if value > 1]
for key in keys_to_remove:
    del data[key]

# ✅ ดีกว่า: สร้าง dict ใหม่
filtered = {key: value for key, value in data.items() if value <= 1}
```

---

## 📝 สรุป Dictionary Methods

| Method | คำอธิบาย | ตัวอย่าง |
|--------|-----------|-----------|
| `dict.get(key, default)` | ดึงค่าอย่างปลอดภัย | `person.get("age", 0)` |
| `dict.keys()` | ดึง keys ทั้งหมด | `person.keys()` |
| `dict.values()` | ดึง values ทั้งหมด | `person.values()` |
| `dict.items()` | ดึง key-value ทั้งหมด | `person.items()` |
| `dict.update(other)` | อัปเดต dict | `person.update({"age": 26})` |
| `dict.pop(key, default)` | ลบและคืนค่า | `person.pop("age")` |
| `dict.popitem()` | ลด key สุดท้าย | `person.popitem()` |
| `dict.setdefault(key, default)` | เพิ่มถ้าไม่มี | `person.setdefault("city", "BKK")` |
| `dict.clear()` | ลบทั้งหมด | `person.clear()` |

---

## 🎯 แบบฝึกหัด

### แบบฝึกหัด 1: นับคำ
```python
# เขียนฟังก์ชันนับคำในข้อความ
def word_count(text):
    pass
```

### แบบฝึกหัด 2: จัดกลุ่มข้อมูล
```python
# เขียนฟังก์ชันจัดกลุ่มข้อมูลตามหมวดหมู่
def group_by_category(items, key_func):
    pass
```

### แบบฝึกหัด 3: แปลงข้อมูล
```python
# เขียนฟังก์ชันแปลง CSV เป็น dict
def csv_to_dict(csv_data):
    pass
```

---

*สร้างเมื่อ: 4 มีนาคม 2026*  
*ปรับปรุงล่าสุด: 4 มีนาคม 2026*
