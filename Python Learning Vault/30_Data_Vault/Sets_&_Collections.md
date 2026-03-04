# Sets & Collections - กลุ่มข้อมูลที่ไม่ซ้ำกัน

## 🎯 แนวคิดพื้นฐาน

Set คือ **โครงสร้างข้อมูลที่เก็บสมาชิกที่ไม่ซ้ำกัน** ไม่มีลำดับ (unordered) และมีการค้นหาที่รวดเร็วมาก (O(1)) เหมาะสำหรับการตรวจสอบสมาชิก การหาค่าที่ไม่ซ้ำ และการดำเนินการแบบเซต

```python
# การสร้าง set
numbers = {1, 2, 3, 4, 5}
fruits = {"แอปเปิ้ล", "กล้วย", "ส้ม"}

# ตรวจสอบสมาชิกอย่างรวดเร็ว
print(3 in numbers)  # True
print("มะม่วง" in fruits)  # False
```

---

## 📝 การสร้างและพื้นฐาน Sets

### การสร้าง Set
```python
# วิธีต่างๆ ในการสร้าง
empty_set = set()                    # {} คือ dict ไม่ใช่ set!
numbers = {1, 2, 3, 4, 5}
fruits = {"แอปเปิ้ล", "กล้วย", "ส้ม"}

# จาก list (จะลบตัวซ้ำออก)
numbers_with_duplicates = [1, 2, 2, 3, 3, 4, 5]
unique_numbers = set(numbers_with_duplicates)  # {1, 2, 3, 4, 5}

# จาก string
unique_chars = set("hello")  # {'h', 'e', 'l', 'o'}

# ใช้ set() constructor
set_from_list = set([1, 2, 3, 4, 5])
set_from_string = set("python")

# Set comprehension
squares = {x**2 for x in range(5)}  # {0, 1, 4, 9, 16}
even_numbers = {x for x in range(10) if x % 2 == 0}  # {0, 2, 4, 6, 8}
```

### การเข้าถึงและแก้ไข Set
```python
fruits = {"แอปเปิ้ล", "กล้วย", "ส้ม"}

# ตรวจสอบสมาชิก (เร็วมาก!)
print("กล้วย" in fruits)        # True
print("มะม่วง" in fruits)       # False

# เพิ่มสมาชิก
fruits.add("มะละกอ")            # {"แอปเปิ้ล", "กล้วย", "ส้ม", "มะละกอ"}
fruits.add("กล้วย")             # ไม่เพิ่ม เพราะมีอยู่แล้ว

# เพิ่มหลายสมาชิก
fruits.update(["ฝรั่ง", "มะนาว", "ลิ้นจี่"])

# ลบสมาชิก
fruits.remove("ส้ม")            # ลบและ error ถ้าไม่มี
fruits.discard("มะละกอ")        # ลบและไม่ error ถ้าไม่มี
removed = fruits.pop()           # ลบสมาชิกแบบสุ่มและคืนค่า

# ล้าง set
fruits.clear()                   # set()

# ขนาดของ set
print(len(fruits))               # 0
```

### การวนลูป Set
```python
numbers = {1, 2, 3, 4, 5}

# วนลูปสมาชิก
for num in numbers:
    print(num)

# วนลูปพรับ index
for i, num in enumerate(numbers):
    print(f"{i}: {num}")

# List comprehension กับ set
squared_list = [x**2 for x in numbers]
squared_set = {x**2 for x in numbers}
```

---

## 🎪 การดำเนินการแบบ Set

### การดำเนินการพื้นฐาน
```python
set1 = {1, 2, 3, 4, 5}
set2 = {4, 5, 6, 7, 8}

# Union (รวม) - สมาชิกที่อยู่ใน set ใด set หนึ่ง
union_set = set1 | set2           # {1, 2, 3, 4, 5, 6, 7, 8}
union_set = set1.union(set2)     # อีกวิธี

# Intersection (ตัดกัน) - สมาชิกที่อยู่ในทั้งสอง set
intersection_set = set1 & set2    # {4, 5}
intersection_set = set1.intersection(set2)

# Difference (ต่าง) - สมาชิกที่อยู่ใน set1 แต่ไม่อยู่ใน set2
difference_set = set1 - set2      # {1, 2, 3}
difference_set = set1.difference(set2)

# Symmetric Difference (ต่างสมมาตร) - สมาชิกที่อยู่ใน set ใด set หนึ่งเท่านั้น
sym_diff_set = set1 ^ set2        # {1, 2, 3, 6, 7, 8}
sym_diff_set = set1.symmetric_difference(set2)
```

### การเปรียบเทียบ Set
```python
set1 = {1, 2, 3, 4, 5}
set2 = {1, 2, 3}
set3 = {1, 2, 3, 4, 5}

# Subset (set2 เป็นส่วนย่อยของ set1)
print(set2.issubset(set1))       # True
print(set2 <= set1)              # True

# Superset (set1 เป็น superset ของ set2)
print(set1.issuperset(set2))     # True
print(set1 >= set2)              # True

# Disjoint (ไม่มีสมาชิกร่วมกัน)
set4 = {6, 7, 8}
print(set1.isdisjoint(set4))     # True

# เท่ากัน
print(set1 == set3)              # True
print(set1 != set2)              # True
```

### การดำเนินการแบบ Update
```python
set1 = {1, 2, 3}
set2 = {3, 4, 5}

# Update union (เปลี่ยน set1)
set1.update(set2)                 # {1, 2, 3, 4, 5}
set1 |= set2                      # อีกวิธี

# Update intersection
set1.intersection_update(set2)    # {3}
set1 &= set2

# Update difference
set1.difference_update(set2)      # {1, 2}
set1 -= set2

# Update symmetric difference
set1.symmetric_difference_update(set2)  # {1, 2, 4, 5}
set1 ^= set2
```

---

## 🛠️ การใช้งานจริง

### การหาค่าที่ไม่ซ้ำกัน
```python
def find_unique_items(data):
    """หาค่าที่ไม่ซ้ำกันจาก list"""
    return list(set(data))

def count_duplicates(data):
    """นับจำนวนข้อมูลซ้ำ"""
    unique = set(data)
    return len(data) - len(unique)

def remove_duplicates_keep_order(data):
    """ลบตัวซ้ำแต่รักษาลำดับ"""
    seen = set()
    result = []
    for item in data:
        if item not in seen:
            seen.add(item)
            result.append(item)
    return result

# ตัวอย่างการใช้งาน
numbers = [1, 2, 2, 3, 3, 4, 5, 5, 5]
unique_numbers = find_unique_items(numbers)  # [1, 2, 3, 4, 5]
duplicate_count = count_duplicates(numbers)  # 4
ordered_unique = remove_duplicates_keep_order(numbers)  # [1, 2, 3, 4, 5]
```

### การตรวจสอบ Permission
```python
class PermissionManager:
    """จัดการสิทธิ์ผู้ใช้"""
    
    def __init__(self):
        self.permissions = {
            "admin": {"read", "write", "delete", "manage"},
            "editor": {"read", "write"},
            "viewer": {"read"},
            "guest": set()
        }
    
    def has_permission(self, user_role, permission):
        """ตรวจสอบว่ามีสิทธิ์หรือไม่"""
        return permission in self.permissions.get(user_role, set())
    
    def add_permission(self, user_role, permission):
        """เพิ่มสิทธิ์"""
        if user_role not in self.permissions:
            self.permissions[user_role] = set()
        self.permissions[user_role].add(permission)
    
    def remove_permission(self, user_role, permission):
        """ลบสิทธิ์"""
        if user_role in self.permissions:
            self.permissions[user_role].discard(permission)
    
    def get_all_permissions(self, user_role):
        """ดึงสิทธิ์ทั้งหมด"""
        return self.permissions.get(user_role, set())
    
    def can_access(self, user_role, required_permissions):
        """ตรวจสอบว่ามีสิทธิ์ครบถ้วนหรือไม่"""
        user_perms = self.permissions.get(user_role, set())
        return required_permissions.issubset(user_perms)

# การใช้งาน
perm_manager = PermissionManager()
print(perm_manager.has_permission("admin", "delete"))  # True
print(perm_manager.has_permission("viewer", "write"))  # False
print(perm_manager.can_access("editor", {"read", "write"}))  # True
```

### การวิเคราะห์ข้อมูล
```python
def analyze_text(text):
    """วิเคราะห์ข้อความ"""
    words = text.lower().split()
    unique_words = set(words)
    
    return {
        "total_words": len(words),
        "unique_words": len(unique_words),
        "word_frequency": {word: words.count(word) for word in unique_words},
        "long_words": {word for word in unique_words if len(word) > 5}
    }

def find_common_elements(lists):
    """หาสมาชิกที่มีในทุก list"""
    if not lists:
        return set()
    
    common = set(lists[0])
    for lst in lists[1:]:
        common.intersection_update(lst)
    
    return common

def find_any_elements(lists):
    """หาสมาชิกที่มีใน list ใด list หนึ่ง"""
    all_elements = set()
    for lst in lists:
        all_elements.update(lst)
    return all_elements

# ตัวอย่างการใช้งาน
text = "Hello world hello Python world programming"
analysis = analyze_text(text)

lists = [
    [1, 2, 3, 4, 5],
    [3, 4, 5, 6, 7],
    [5, 6, 7, 8, 9]
]

common = find_common_elements(lists)  # {5}
any_elements = find_any_elements(lists)  # {1, 2, 3, 4, 5, 6, 7, 8, 9}
```

---

## 🎯 การเพิ่มประสิทธิภาพ

### การเลือกใช้ Set อย่างเหมาะสม
```python
# ✅ ใช้ set เมื่อต้องการตรวจสอบสมาชิกเร็วๆ
valid_colors = {"red", "green", "blue"}
if user_color in valid_colors:  # O(1)
    print("Valid color")

# ❌ ไม่ดี: ใช้ list ตรวจสอบสมาชิก
valid_colors_list = ["red", "green", "blue"]
if user_color in valid_colors_list:  # O(n)
    print("Valid color")

# ✅ ใช้ set เมื่อต้องการค่าที่ไม่ซ้ำกัน
unique_ids = set(user_ids)

# ✅ ใช้ set เมื่อต้องการดำเนินการแบบเซต
common_tags = set1 & set2
```

### การเปรียบเทียบประสิทธิภาพ
```python
import time

def test_performance():
    large_list = list(range(1000000))
    large_set = set(range(1000000))
    
    # ทดสอบการค้นหา
    search_item = 999999
    
    # List search
    start = time.time()
    found_list = search_item in large_list
    list_time = time.time() - start
    
    # Set search
    start = time.time()
    found_set = search_item in large_set
    set_time = time.time() - start
    
    print(f"List search time: {list_time:.6f}s")
    print(f"Set search time: {set_time:.6f}s")
    print(f"Set is {list_time/set_time:.1f}x faster")

test_performance()
```

### การใช้ Frozen Set
```python
# Frozen set - immutable set
frozen_set = frozenset([1, 2, 3, 4, 5])

# ใช้เป็น key ใน dictionary
permissions = {
    frozenset({"read", "write"}): "editor",
    frozenset({"read"}): "viewer",
    frozenset({"read", "write", "delete"}): "admin"
}

# ใช้ใน set ของ set (ไม่สามารถใช้ set ซ้อน set ได้)
set_of_sets = {
    frozenset({1, 2}),
    frozenset({2, 3}),
    frozenset({3, 4})
}
```

---

## 💡 Best Practices

### 1. เลือกโครงสร้างที่เหมาะสม
```python
# ✅ ใช้ set เมื่อต้องการค่าที่ไม่ซ้ำและค้นหาเร็ว
unique_users = set(user_ids)

# ✅ ใช้ list เมื่อต้องการลำดับและซ้ำได้
user_actions = ["login", "click", "logout", "login"]

# ✅ ใช้ dict เมื่อต้องการ key-value
user_profiles = {"user1": {"name": "John"}, "user2": {"name": "Jane"}}
```

### 2. ใช้ Set Operations อย่างมีประสิทธิภาพ
```python
# ✅ ดี: ใช้ set operations
common_tags = user_tags & system_tags
missing_tags = required_tags - user_tags

# ❌ ไม่ดี: ใช้ loop
common_tags = [tag for tag in user_tags if tag in system_tags]
missing_tags = [tag for tag in required_tags if tag not in user_tags]
```

### 3. ระวังการแปลงข้อมูล
```python
# ✅ ดี: แปลงครั้งเดียว
unique_items = set(large_list)

# ❌ ไม่ดี: แปลงหลายครั้ง
for item in large_list:
    if item not in seen_set:  # แปลงซ้ำ
        seen_set.add(item)
```

### 4. ใช้ Type Hints
```python
from typing import Set, List, Dict, FrozenSet

def process_tags(tags: List[str]) -> Set[str]:
    """ประมวลผล tags"""
    return set(tags)

def analyze_permissions(permissions: FrozenSet[str]) -> bool:
    """วิเคราะห์สิทธิ์"""
    return "admin" in permissions

def group_items(items: List[int]) -> Dict[int, Set[int]]:
    """จัดกลุ่มข้อมูล"""
    groups = {}
    for item in items:
        category = item % 10
        if category not in groups:
            groups[category] = set()
        groups[category].add(item)
    return groups
```

---

## 📝 สรุป Set Methods

| Method | คำอธิบาย | ตัวอย่าง |
|--------|-----------|-----------|
| `set.add(item)` | เพิ่มสมาชิก | `my_set.add(5)` |
| `set.remove(item)` | ลบสมาชิก (error ถ้าไม่มี) | `my_set.remove(5)` |
| `set.discard(item)` | ลบสมาชิก (ไม่ error) | `my_set.discard(5)` |
| `set.pop()` | ลบสมาชิกแบบสุ่ม | `item = my_set.pop()` |
| `set.clear()` | ลบทั้งหมด | `my_set.clear()` |
| `set1 & set2` | Intersection | `set1.intersection(set2)` |
| `set1 \| set2` | Union | `set1.union(set2)` |
| `set1 - set2` | Difference | `set1.difference(set2)` |
| `set1 ^ set2` | Symmetric Difference | `set1.symmetric_difference(set2)` |
| `set1.issubset(set2)` | Subset | `set1 <= set2` |
| `set1.issuperset(set2)` | Superset | `set1 >= set2` |
| `set1.isdisjoint(set2)` | Disjoint | `set1.isdisjoint(set2)` |

---

## 🎯 แบบฝึกหัด

### แบบฝึกหัด 1: หาค่าที่ไม่ซ้ำ
```python
# เขียนฟังก์ชันหาค่าที่ปรากฏเพียงครั้งเดียว
def find_unique_once(items):
    pass
```

### แบบฝึกหัด 2: ตรวจสอบ Anagram
```python
# เขียนฟังก์ชันตรวจสอบว่าเป็น anagram หรือไม่
def is_anagram(word1, word2):
    pass
```

### แบบฝึกหัด 3: จัดการ Tags
```python
# เขียนฟังก์ชันจัดการ tags ของบทความ
def manage_tags(article_tags, user_tags, action):
    pass
```

---

*สร้างเมื่อ: 4 มีนาคม 2026*  
*ปรับปรุงล่าสุด: 4 มีนาคม 2026*
