# Classes & Objects - การสร้างต้นแบบและอัตลักษณ์

## 🏗️ แนวคิดพื้นฐาน

Class คือ **แม่แบบ (template)** สำหรับสร้าง objects ส่วน Object คือ **ตัวตน (instance)** ที่สร้างจาก class คล้ายกับแม่พิมพ์ (class) และขนม (object)

```python
# การสร้าง class พื้นฐาน
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age
    
    def greet(self):
        return f"สวัสดีฉันชื่อ {self.name}"

# การสร้าง object
person1 = Person("สมชาย", 25)
person2 = Person("มานี", 30)

print(person1.greet())  # สวัสดีฉันชื่อ สมชาย
print(person2.name)     # มานี
```

---

## 📝 การสร้าง Classes พื้นฐาน

### Class และ Instance
```python
class Car:
    """คลาสรถยนต์"""
    
    # Class attribute (ใช้ร่วมกันทุก instance)
    wheel_count = 4
    
    def __init__(self, brand, model, year):
        """Constructor - สร้าง object"""
        # Instance attributes (แต่ละ object มีของตัวเอง)
        self.brand = brand
        self.model = model
        self.year = year
        self.mileage = 0
    
    def start_engine(self):
        """เริ่มต้นเครื่องยนต์"""
        return f"เครื่องยนต์ {self.brand} {self.model} สตาร์ทแล้ว"
    
    def drive(self, distance):
        """ขับรถ"""
        self.mileage += distance
        return f"ขับรถไป {distance} กิโลเมตร"
    
    def get_info(self):
        """ดึงข้อมูลรถ"""
        return f"{self.brand} {self.model} ({self.year}) - วิ่ง {self.mileage} กม."

# การสร้าง objects
car1 = Car("Toyota", "Camry", 2022)
car2 = Car("Honda", "Civic", 2023)

# การเข้าถึง attributes
print(car1.brand)        # Toyota
print(car2.year)         # 2023
print(Car.wheel_count)   # 4 (class attribute)

# การเรียก methods
print(car1.start_engine())  # เครื่องยนต์ Toyota Camry สตาร์ทแล้ว
car1.drive(100)
print(car1.get_info())      # Toyota Camry (2022) - วิ่ง 100 กม.
```

### Methods ประเภทต่างๆ
```python
class BankAccount:
    """บัญชีธนาคาร"""
    
    bank_name = "Python Bank"  # Class attribute
    
    def __init__(self, account_number, owner_name, balance=0):
        self.account_number = account_number
        self.owner_name = owner_name
        self.balance = balance
    
    # Instance method (ต้องมี self)
    def deposit(self, amount):
        """ฝากเงิน"""
        if amount > 0:
            self.balance += amount
            return f"ฝาก {amount} บาท สำเร็จ"
        return "จำนวนเงินต้องมากกว่า 0"
    
    def withdraw(self, amount):
        """ถอนเงิน"""
        if amount <= self.balance:
            self.balance -= amount
            return f"ถอน {amount} บาท สำเร็จ"
        return "ยอดเงินไม่พอ"
    
    def get_balance(self):
        """ดูยอดเงิน"""
        return f"ยอดเงิน: {self.balance} บาท"
    
    # Class method (ต้องมี cls)
    @classmethod
    def get_bank_info(cls):
        """ดูข้อมูลธนาคาร"""
        return f"ธนาคาร: {cls.bank_name}"
    
    @classmethod
    def create_account(cls, owner_name, initial_deposit=0):
        """สร้างบัญชีใหม่"""
        import random
        account_number = f"ACC{random.randint(1000, 9999)}"
        return cls(account_number, owner_name, initial_deposit)
    
    # Static method (ไม่ต้องมี self หรือ cls)
    @staticmethod
    def validate_account_number(account_number):
        """ตรวจสอบเลขที่บัญชี"""
        return account_number.startswith("ACC") and len(account_number) == 7

# การใช้งาน
account1 = BankAccount("ACC1234", "สมชาย", 10000)
print(account1.deposit(5000))        # ฝาก 5000 บาท สำเร็จ
print(account1.get_balance())        # ยอดเงิน: 15000 บาท

# Class method
print(BankAccount.get_bank_info())   # ธนาคาร: Python Bank
account2 = BankAccount.create_account("วิชัย", 2000)

# Static method
print(BankAccount.validate_account_number("ACC1234"))  # True
print(BankAccount.validate_account_number("XYZ1234"))  # False
```

---

## 🎪 Inheritance (การสืบทอด)

### การสืบทอดพื้นฐาน
```python
# Parent class (Superclass)
class Animal:
    """สัตว์"""
    
    def __init__(self, name, age):
        self.name = name
        self.age = age
    
    def eat(self):
        """กินอาหาร"""
        return f"{self.name} กำลังกินอาหาร"
    
    def sleep(self):
        """นอน"""
        return f"{self.name} กำลังนอน"
    
    def make_sound(self):
        """ส่งเสียง (base implementation)"""
        return "ส่งเสียง..."

# Child class (Subclass)
class Dog(Animal):
    """สุนัข"""
    
    def __init__(self, name, age, breed):
        # เรียก constructor ของ parent
        super().__init__(name, age)
        self.breed = breed
    
    # Override method
    def make_sound(self):
        """ส่งเสียงหอบหอบ"""
        return f"{self.name} หอบหอบ!"
    
    # New method
    def wag_tail(self):
        """เขย่าหาง"""
        return f"{self.name} กำลังเขย่าหาง"
    
    def fetch(self, item):
        """เอาของมาให้"""
        return f"{self.name} วิ่งไปเอา {item}"

class Cat(Animal):
    """แมว"""
    
    def __init__(self, name, age, color):
        super().__init__(name, age)
        self.color = color
    
    def make_sound(self):
        """ส่งเสียงเหมียว"""
        return f"{self.name} เหมียว!"
    
    def scratch(self):
        """ข่วน"""
        return f"{self.name} กำลังข่วน"

# การใช้งาน
dog = Dog("หมู", 3, "Golden Retriever")
cat = Cat("น้ำ", 2, "ส้ม")

print(dog.eat())           # หมู กำลังกินอาหาร (จาก Animal)
print(dog.make_sound())    # หมู หอบหอบ! (override)
print(dog.wag_tail())      # หมู กำลังเขย่าหาง (Dog method)

print(cat.sleep())         # น้ำ กำลังนอน (จาก Animal)
print(cat.make_sound())    # น้ำ เหมียว! (override)
```

### Multiple Inheritance
```python
class Flyable:
    """สามารถบินได้"""
    
    def fly(self):
        """บิน"""
        return "กำลังบิน"
    
    def take_off(self):
        """起飞"""
        return "起飞!"

class Swimmable:
    """สามารถว่ายน้ำได้"""
    
    def swim(self):
        """ว่ายน้ำ"""
        return "กำลังว่ายน้ำ"
    
    def dive(self):
        """ดำน้ำ"""
        return "ดำน้ำลึก!"

class Duck(Animal, Flyable, Swimmable):
    """เป็ด - สืบทอดหลายคลาส"""
    
    def __init__(self, name, age):
        super().__init__(name, age)
    
    def make_sound(self):
        """ส่งเสียงก๊าบ"""
        return f"{self.name} ก๊าบก๊าบ!"
    
    def do_all(self):
        """ทำกิจกรรมทั้งหมด"""
        actions = [
            self.eat(),
            self.make_sound(),
            self.fly(),
            self.swim()
        ]
        return actions

# การใช้งาน
duck = Duck("เป็ดเป็ด", 1)
print(duck.fly())        # กำลังบิน (จาก Flyable)
print(duck.swim())       # กำลังว่ายน้ำ (จาก Swimmable)
print(duck.eat())        # เป็ดเป็ด กำลังกินอาหาร (จาก Animal)

for action in duck.do_all():
    print(action)
```

---

## 🛠️ Encapsulation และ Access Modifiers

### Public, Protected, Private
```python
class Employee:
    """พนักงาน"""
    
    def __init__(self, employee_id, name, salary):
        # Public attribute
        self.employee_id = employee_id
        self.name = name
        
        # Protected attribute (convention: _)
        self._salary = salary
        
        # Private attribute (name mangling: __)
        self.__bonus = 0
    
    # Public method
    def get_info(self):
        """ดูข้อมูลพนักงาน"""
        return f"ID: {self.employee_id}, ชื่อ: {self.name}"
    
    # Protected method
    def _calculate_tax(self):
        """คำนวณภาษี (protected)"""
        return self._salary * 0.07
    
    # Private method
    def __calculate_total_income(self):
        """คำนวณรายได้ทั้งหมด (private)"""
        return self._salary + self.__bonus
    
    # Public interface สำหรับเข้าถึง private data
    def set_bonus(self, bonus):
        """ตั้งโบนัส"""
        if bonus > 0:
            self.__bonus = bonus
    
    def get_total_income(self):
        """ดูรายได้ทั้งหมด"""
        return self.__calculate_total_income()
    
    def get_salary(self):
        """ดูเงินเดือน"""
        return self._salary
    
    def set_salary(self, new_salary):
        """ตั้งเงินเดือน"""
        if new_salary > 0:
            self._salary = new_salary

# การใช้งาน
emp = Employee("E001", "สมชาย", 50000)

# Public - เข้าถึงได้
print(emp.get_info())          # ID: E001, ชื่อ: สมชาย

# Protected - ควรไม่เข้าถึงโดยตรง (แต่ Python ไม่ห้าม)
print(emp._salary)            # 50000 (ทำได้แต่ไม่แนะนำ)

# Private - ไม่สามารถเข้าถึงโดยตรง
# print(emp.__bonus)         # Error!
# print(emp.__calculate_total_income())  # Error!

# ใช้ public interface แทน
emp.set_bonus(5000)
print(emp.get_total_income())  # 55000

# Name mangling ทำให้เข้าถึงได้แต่ยากขึ้น
print(emp._Employee__bonus)    # 50000 (ทางลัด ไม่แนะนำ)
```

### Properties (Getters/Setters)
```python
class Temperature:
    """อุณหภูมิ"""
    
    def __init__(self, celsius=0):
        self._celsius = celsius
    
    @property
    def celsius(self):
        """อุณหภูมิเซลเซียส"""
        return self._celsius
    
    @celsius.setter
    def celsius(self, value):
        """ตั้งอุณหภูมิเซลเซียส"""
        if value < -273.15:
            raise ValueError("อุณหภูมิต่ำสุดคือ -273.15°C")
        self._celsius = value
    
    @property
    def fahrenheit(self):
        """อุณหภูมิฟาเรนไฮต์"""
        return (self._celsius * 9/5) + 32
    
    @fahrenheit.setter
    def fahrenheit(self, value):
        """ตั้งอุณหภูมิฟาเรนไฮต์"""
        self.celsius = (value - 32) * 5/9
    
    @property
    def kelvin(self):
        """อุณหภูมิเคลวิน"""
        return self._celsius + 273.15
    
    def __str__(self):
        return f"อุณหภูมิ: {self._celsius:.1f}°C / {self.fahrenheit:.1f}°F / {self.kelvin:.1f}K"

# การใช้งาน
temp = Temperature(25)

# ใช้ property เหมือน attribute
print(temp.celsius)       # 25
print(temp.fahrenheit)    # 77.0
print(temp.kelvin)        # 298.15

# ตั้งค่าผ่าน property
temp.celsius = 30
print(temp.fahrenheit)    # 86.0

temp.fahrenheit = 100
print(temp.celsius)       # 37.78

# Validation ใน setter
try:
    temp.celsius = -300   # Error!
except ValueError as e:
    print(f"Error: {e}")

print(temp)              # อุณหภูมิ: 37.8°C / 100.0°F / 310.9K
```

---

## 🎨 Magic Methods (Dunder Methods)

### พื้นฐาน Magic Methods
```python
class Book:
    """หนังสือ"""
    
    def __init__(self, title, author, pages, price):
        self.title = title
        self.author = author
        self.pages = pages
        self.price = price
    
    def __str__(self):
        """String representation (print)"""
        return f"'{self.title}' โดย {self.author}"
    
    def __repr__(self):
        """Official representation"""
        return f"Book('{self.title}', '{self.author}', {self.pages}, {self.price})"
    
    def __len__(self):
        """จำนวนหน้า"""
        return self.pages
    
    def __bool__(self):
        """Truth value"""
        return self.pages > 0
    
    def __eq__(self, other):
        """เท่ากับ"""
        if isinstance(other, Book):
            return (self.title == other.title and 
                   self.author == other.author)
        return False
    
    def __lt__(self, other):
        """น้อยกว่า (compare by price)"""
        if isinstance(other, Book):
            return self.price < other.price
        return NotImplemented
    
    def __add__(self, other):
        """บวก (combine books)"""
        if isinstance(other, Book):
            combined_title = f"{self.title} + {other.title}"
            combined_author = f"{self.author}, {other.author}"
            total_pages = self.pages + other.pages
            total_price = self.price + other.price
            return Book(combined_title, combined_author, total_pages, total_price)
        return NotImplemented
    
    def __getitem__(self, key):
        """Access like dictionary"""
        if key == "title":
            return self.title
        elif key == "author":
            return self.author
        elif key == "pages":
            return self.pages
        elif key == "price":
            return self.price
        else:
            raise KeyError(f"Key '{key}' not found")
    
    def __call__(self, discount=0):
        """Make object callable"""
        final_price = self.price * (1 - discount)
        return f"ราคาหลังหักส่วนลด {discount*100}%: {final_price:.2f} บาท"

# การใช้งาน
book1 = Book("Python Programming", "John Doe", 300, 450)
book2 = Book("Data Science", "Jane Smith", 250, 380)

# String representation
print(book1)                 # 'Python Programming' โดย John Doe (__str__)
print(repr(book1))           # Book('Python Programming', 'John Doe', 300, 450) (__repr__)

# Built-in functions
print(len(book1))            # 300 (__len__)
print(bool(book1))           # True (__bool__)

# Comparison
print(book1 == book2)        # False (__eq__)
print(book1 < book2)         # False (__lt__) - 450 > 380

# Arithmetic
combined = book1 + book2
print(combined.title)        # Python Programming + Data Science (__add__)

# Dictionary-like access
print(book1["title"])        # Python Programming (__getitem__)

# Callable
print(book1(0.1))            # ราคาหลังหักส่วนลด 10.0%: 405.00 บาท (__call__)
```

### Magic Methods ขั้นสูง
```python
class ShoppingCart:
    """ตะกร้าสินค้า"""
    
    def __init__(self):
        self._items = {}
    
    def __len__(self):
        """จำนวนสินค้า"""
        return sum(self._items.values())
    
    def __contains__(self, item):
        """ตรวจสอบว่ามีสินค้าหรือไม่"""
        return item in self._items
    
    def __iter__(self):
        """Iterator สำหรับวนลูป"""
        return iter(self._items.items())
    
    def __getitem__(self, item):
        """ดูจำนวนสินค้า"""
        return self._items.get(item, 0)
    
    def __setitem__(self, item, quantity):
        """ตั้งจำนวนสินค้า"""
        if quantity <= 0:
            del self._items[item]
        else:
            self._items[item] = quantity
    
    def __delitem__(self, item):
        """ลบสินค้า"""
        if item in self._items:
            del self._items[item]
    
    def add_item(self, item, quantity=1):
        """เพิ่มสินค้า"""
        self[item] = self[item] + quantity
    
    def remove_item(self, item, quantity=1):
        """ลบสินค้า"""
        current = self[item]
        if current <= quantity:
            del self[item]
        else:
            self[item] = current - quantity
    
    def __str__(self):
        """แสดงรายการสินค้า"""
        if not self._items:
            return "ตะกร้าว่าง"
        
        items = []
        for item, quantity in self._items.items():
            items.append(f"{item}: {quantity}")
        
        return "ตะกร้าสินค้า:\n" + "\n".join(items)

# การใช้งาน
cart = ShoppingCart()

# เพิ่มสินค้า
cart.add_item("น้ำดื่ม", 2)
cart.add_item "ขนมปัง", 1)
cart["ผลไม้"] = 5

# ตรวจสอบ
print(len(cart))              # 8 (__len__)
print("น้ำดื่ม" in cart)      # True (__contains__)
print(cart["น้ำดื่ม"])        # 2 (__getitem__)

# วนลูป
for item, quantity in cart:   # (__iter__)
    print(f"{item}: {quantity}")

# แก้ไข
cart["น้ำดื่ม"] = 3          # (__setitem__)
del cart["ขนมปัง"]           # (__delitem__)

print(cart)                   # (__str__)
```

---

## 🎯 การใช้งานจริง

### การสร้าง Game Character
```python
import random

class Character:
    """ตัวละครเกม"""
    
    def __init__(self, name, character_class):
        self.name = name
        self.character_class = character_class
        self.level = 1
        self.hp = 100
        self.max_hp = 100
        self.mp = 50
        self.max_mp = 50
        self.attack = 10
        self.defense = 5
        self.inventory = []
        self.skills = []
    
    def level_up(self):
        """อัปเลเวล"""
        self.level += 1
        self.max_hp += 20
        self.max_mp += 10
        self.attack += 5
        self.defense += 3
        self.hp = self.max_hp
        self.mp = self.max_mp
        return f"{self.name} อัปเลเวลเป็น {self.level}!"
    
    def take_damage(self, damage):
        """รับความเสียหาย"""
        actual_damage = max(0, damage - self.defense)
        self.hp -= actual_damage
        if self.hp <= 0:
            self.hp = 0
            return f"{self.name} โดนความเสียหาย {actual_damage} และตายแล้ว!"
        return f"{self.name} โดนความเสียหาย {actual_damage} เหลือ HP {self.hp}"
    
    def heal(self, amount):
        """ฟื้น HP"""
        old_hp = self.hp
        self.hp = min(self.max_hp, self.hp + amount)
        healed = self.hp - old_hp
        return f"{self.name} ฟื้น HP {healed} เหลือ {self.hp}/{self.max_hp}"
    
    def attack_enemy(self, enemy):
        """โจมตีศัตรู"""
        damage = self.attack + random.randint(-2, 2)
        return enemy.take_damage(damage)
    
    def add_item(self, item):
        """เพิ่มไอเทม"""
        self.inventory.append(item)
        return f"{self.name} ได้รับ {item}"
    
    def use_item(self, item_name):
        """ใช้ไอเทม"""
        for i, item in enumerate(self.inventory):
            if item.name == item_name:
                effect = item.use(self)
                self.inventory.pop(i)
                return effect
        return f"ไม่มีไอเทม {item_name}"
    
    def __str__(self):
        """แสดงข้อมูลตัวละคร"""
        return (f"{self.name} (Lv.{self.level} {self.character_class})\n"
                f"HP: {self.hp}/{self.max_hp} | MP: {self.mp}/{self.max_mp}\n"
                f"ATK: {self.attack} | DEF: {self.defense}")

class Item:
    """ไอเทม"""
    
    def __init__(self, name, item_type, effect_value):
        self.name = name
        self.item_type = item_type
        self.effect_value = effect_value
    
    def use(self, character):
        """ใช้ไอเทม"""
        if self.item_type == "potion":
            return character.heal(self.effect_value)
        elif self.item_type == "ether":
            old_mp = character.mp
            character.mp = min(character.max_mp, character.mp + self.effect_value)
            restored = character.mp - old_mp
            return f"{character.name} ฟื้น MP {restored}"
        else:
            return f"ใช้ {self.name}"

# การใช้งาน
hero = Character("ฮีโร่", "Warrior")
monster = Character("มอนสเตอร์", "Beast")

# เพิ่มไอเทม
potion = Item("Potion", "potion", 30)
hero.add_item(potion)

# การต่อสู้
print(hero)
print(monster)
print(hero.attack_enemy(monster))
print(monster.attack_enemy(hero))
print(hero.use_item("Potion"))
print(hero)
```

### การสร้าง Database Model
```python
import datetime
from typing import Dict, Any, Optional

class Model:
    """Base Model สำหรับ database"""
    
    def __init__(self, **kwargs):
        self.id = None
        self.created_at = datetime.datetime.now()
        self.updated_at = datetime.datetime.now()
        
        # กำหนด attributes จาก kwargs
        for key, value in kwargs.items():
            if hasattr(self, key):
                setattr(self, key, value)
    
    def to_dict(self) -> Dict[str, Any]:
        """แปลงเป็น dictionary"""
        result = {}
        for key, value in self.__dict__.items():
            if isinstance(value, datetime.datetime):
                result[key] = value.isoformat()
            else:
                result[key] = value
        return result
    
    @classmethod
    def from_dict(cls, data: Dict[str, Any]):
        """สร้าง object จาก dictionary"""
        instance = cls()
        for key, value in data.items():
            if hasattr(instance, key):
                if key.endswith('_at') and isinstance(value, str):
                    # แปลง string เป็น datetime
                    try:
                        value = datetime.datetime.fromisoformat(value)
                    except ValueError:
                        pass
                setattr(instance, key, value)
        return instance
    
    def save(self):
        """บันทึก (simulate)"""
        self.updated_at = datetime.datetime.now()
        print(f"Saved {self.__class__.__name__}: {self.to_dict()}")
        return self
    
    def __repr__(self):
        return f"{self.__class__.__name__}(id={self.id})"

class User(Model):
    """โมเดลผู้ใช้"""
    
    def __init__(self, username: str = "", email: str = "", **kwargs):
        super().__init__(**kwargs)
        self.username = username
        self.email = email
        self.is_active = True
    
    def validate(self) -> bool:
        """ตรวจสอบความถูกต้อง"""
        return (len(self.username) >= 3 and 
                "@" in self.email and
                len(self.email) > 5)
    
    def deactivate(self):
        """ปิดใช้งานผู้ใช้"""
        self.is_active = False
        return self.save()

class Post(Model):
    """โมเดลโพสต์"""
    
    def __init__(self, title: str = "", content: str = "", 
                 author_id: Optional[int] = None, **kwargs):
        super().__init__(**kwargs)
        self.title = title
        self.content = content
        self.author_id = author_id
        self.published = False
    
    def publish(self):
        """เผยแพร่โพสต์"""
        self.published = True
        return self.save()
    
    def unpublish(self):
        """ยกเลิกเผยแพร่"""
        self.published = False
        return self.save()

# การใช้งาน
user = User(username="somchai", email="somchai@example.com")
if user.validate():
    user.save()

post = Post(title="สวัสดี Python", content="นี่คือเนื้อหา...", author_id=1)
post.publish()

# แปลงเป็น dictionary และกลับ
user_dict = user.to_dict()
user_from_dict = User.from_dict(user_dict)
```

---

## 💡 Best Practices

### 1. ตั้งชื่อ Class และ Attributes
```python
# ✅ ดี: ใช้ PascalCase สำหรับ class
class UserAccount:
    pass

class DatabaseConnection:
    pass

# ✅ ดี: ใช้ snake_case สำหรับ attributes/methods
class Person:
    def __init__(self, first_name, last_name):
        self.first_name = first_name
        self.last_name = last_name
    
    def get_full_name(self):
        return f"{self.first_name} {self.last_name}"

# ❌ ไม่ดี: ชื่อไม่ชัดเจน
class uc:
    pass

class data:
    def __init__(self, n, a):
        self.n = n
        self.a = a
```

### 2. ใช้ Composition มากกว่า Inheritance
```python
# ✅ ดี: ใช้ composition
class Engine:
    def start(self):
        return "Engine started"

class Car:
    def __init__(self):
        self.engine = Engine()  # มี engine
    
    def start(self):
        return self.engine.start()

# ❌ ไม่ดี: inheritance ลึกเกินไป
class Car(Engine):  # Car ไม่ใช่ Engine
    pass
```

### 3. ใช้ Type Hints
```python
from typing import List, Dict, Optional, Union

class Product:
    """สินค้า"""
    
    def __init__(self, 
                 name: str, 
                 price: float, 
                 category: str,
                 tags: Optional[List[str]] = None):
        self.name = name
        self.price = price
        self.category = category
        self.tags = tags or []
    
    def add_tag(self, tag: str) -> None:
        """เพิ่มแท็ก"""
        if tag not in self.tags:
            self.tags.append(tag)
    
    def get_info(self) -> Dict[str, Union[str, float, List[str]]]:
        """ดูข้อมูลสินค้า"""
        return {
            "name": self.name,
            "price": self.price,
            "category": self.category,
            "tags": self.tags
        }
```

### 4. เขียน Docstring ให้ครบถ้วน
```python
class Calculator:
    """เครื่องคิดเลข
    
    เครื่องคิดเลขพื้นฐานสำหรับการคำนวณทางคณิตศาสตร์
    
    Attributes:
        memory (float): ค่าที่จดไว้ในหน่วยความจำ
        history (list): ประวัติการคำนวณ
    """
    
    def __init__(self):
        """เริ่มต้นเครื่องคิดเลข"""
        self.memory = 0
        self.history = []
    
    def add(self, a: float, b: float) -> float:
        """บวกเลขสองตัว
        
        Args:
            a (float): ตัวเลขแรก
            b (float): ตัวเลขที่สอง
        
        Returns:
            float: ผลรวมของ a และ b
        
        Example:
            >>> calc = Calculator()
            >>> calc.add(2, 3)
            5.0
        """
        result = a + b
        self.history.append(f"{a} + {b} = {result}")
        return result
```

---

## 📝 สรุป OOP Concepts

| Concept | คำอธิบาย | ตัวอย่าง |
|---------|-----------|-----------|
| **Encapsulation** | ซ่อนข้อมูลภายใน | `self._private_attr` |
| **Inheritance** | สืบทอดคุณสมบัติ | `class Dog(Animal):` |
| **Polymorphism** | หลายรูปแบบ พฤติกรรมเดียว | `duck.make_sound()` |
| **Abstraction** | ซ่อนความซับซ้อน | `@abstractmethod` |
| **Composition** | ประกอบด้วย objects | `self.engine = Engine()` |

---

## 🎯 แบบฝึกหัด

### แบบฝึกหัด 1: Bank System
```python
# เขียนคลาส Bank, Account, Transaction
class Bank:
    pass

class Account:
    pass

class Transaction:
    pass
```

### แบบฝึกหัด 2: Game System
```python
# เขียนคลาส Player, Enemy, Weapon, Game
class Player:
    pass

class Enemy:
    pass

class Weapon:
    pass
```

### แบบฝึกหัด 3: E-commerce
```python
# เขียนคลาส Product, Cart, Order, Customer
class Product:
    pass

class Cart:
    pass

class Order:
    pass
```

---

*สร้างเมื่อ: 4 มีนาคม 2026*  
*ปรับปรุงล่าสุด: 4 มีนาคม 2026*
