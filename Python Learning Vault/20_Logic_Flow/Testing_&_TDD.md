# Testing & TDD — การทดสอบและพัฒนาแบบ Test-Driven

**หมวด:** 20_Logic_Flow — เครื่องมือระดับโปร  
**ระดับ:** สูง (Advanced)  
**Tags:** `#testing` `#pytest` `#unittest` `#tdd`

---

## 🎯 สรุปสั้น ๆ

- **unittest** — โมดูลมาตรฐานของ Python สำหรับเขียน test
- **pytest** — เครื่องมือยอดนิยม เขียน test สั้นกว่า ใช้ได้กับ unittest
- **TDD (Test-Driven Development)** — เขียน test ก่อน แล้วค่อยเขียนโค้ดให้ผ่าน

---

## unittest พื้นฐาน

```python
import unittest

def add(a, b):
    return a + b

class TestAdd(unittest.TestCase):
    def test_add_positive(self):
        self.assertEqual(add(2, 3), 5)

    def test_add_negative(self):
        self.assertEqual(add(-1, -1), -2)

if __name__ == "__main__":
    unittest.main()
```

---

## pytest พื้นฐาน

```python
# test_sample.py
def add(a, b):
    return a + b

def test_add_positive():
    assert add(2, 3) == 5

def test_add_negative():
    assert add(-1, -1) == -2
```

รัน: `pytest test_sample.py -v`

---

## ขั้นตอน TDD แบบสั้น

1. **Red** — เขียน test ที่ fail (ฟังก์ชันยังไม่มีหรือยังผิด)
2. **Green** — เขียนโค้ดน้อยที่สุดให้ test ผ่าน
3. **Refactor** — ปรับปรุงโค้ดให้อ่านง่าย โดย test ยังผ่าน

---

## 📌 สถานะ

โน้ตนี้เป็น **โครงเบื้องต้น** — จะขยายเนื้อหาเมื่อมีโจทย์หรือโปรเจกต์ที่ใช้ testing

**ลิงก์ที่เกี่ยวข้อง:** [Error_Handling](./Error_Handling.md) · [Function_Definitions](../40_Architect_Tools/Function_Definitions.md)

---

*กลับไป: [Index_MOC](../00_Central_Core/Index_MOC.md) · [Learning_Path](../00_Central_Core/Learning_Path.md)*
