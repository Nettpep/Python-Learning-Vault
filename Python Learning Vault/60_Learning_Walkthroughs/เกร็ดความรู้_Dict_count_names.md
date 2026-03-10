% เกร็ดความรู้: นับจำนวนซ้ำด้วย Dictionary

**หมวด:** 60_Learning_Walkthroughs — Dict / Counting  
**ประเภท:** เกร็ดความรู้ / สรุปโจทย์  
**วันที่บันทึก:** 2026-03-10  
**Tags:** `#learning` `#dict` `#count` `#loop` `#if-else`

---

## โค้ดโจทย์

```python
names = ["Anna", "Bob", "Anna", "Cara", "Bob", "Anna"]

counts = {}

for person in names:
    if person not in counts:
        counts[person] = 1
    else:
        counts[person] = counts[person] + 1

print(counts)
```

ผลลัพธ์:

```python
{'Anna': 3, 'Bob': 2, 'Cara': 1}
```

---

## อธิบายสั้น ๆ

- `names` คือ list ของชื่อที่มีซ้ำกันหลายครั้ง  
- `counts = {}` สร้าง dictionary ว่าง สำหรับเก็บผลนับจำนวนครั้งของแต่ละชื่อ  
- วนลูป `for person in names:`  
  - ถ้า `person` **ยังไม่อยู่ใน** `counts`  
    - สร้าง key ใหม่ให้เริ่มนับที่ `1` → `counts[person] = 1`  
  - ถ้า `person` **มีอยู่แล้ว** ใน `counts`  
    - เพิ่มค่าปัจจุบันขึ้นอีก 1 → `counts[person] = counts[person] + 1`
- สุดท้าย `print(counts)` จะได้ dict ที่บอกว่าแต่ละชื่อเจอกี่ครั้ง

---

## สรุป Key Concepts

- ใช้ **dictionary** เก็บ mapping จาก `ชื่อ` → `จำนวนครั้งที่พบ`  
- โครง pattern นับจำนวนที่พบบ่อยมาก:

  ```python
  counts = {}
  for item in some_list:
      if item not in counts:
          counts[item] = 1
      else:
          counts[item] += 1
  ```

- นี่คือพื้นฐานของการทำ **frequency count** / **histogram** ใน Python

---

## Pattern แบบย่อด้วย `.get(key, default)`

แทน if-else ด้วยบรรทัดเดียว — ใช้ `.get(word, 0)` คืน 0 ถ้า key ยังไม่มี:

```python
line = "red blue red green blue red"
counts = dict()
words = line.split()

for word in words:
    counts[word] = counts.get(word, 0) + 1

print(counts)  # {'red': 3, 'blue': 2, 'green': 1}
```

**Key concept:** `dict.get(key, default)` — ถ้ามี key คืน value; ถ้าไม่มี คืน default (ไม่ error)

