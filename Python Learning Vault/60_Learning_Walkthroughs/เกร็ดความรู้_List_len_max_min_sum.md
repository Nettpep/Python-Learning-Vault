# เกร็ดความรู้: List — len, max, min, sum และ Slicing

**หมวด:** 30_Data_Vault — Lists  
**ประเภท:** เกร็ดความรู้ / สรุปทบทวน  
**วันที่บันทึก:** 2026-03-10  
**Tags:** `#learning` `#list` `#built-in` `#len` `#max` `#min` `#sum` `#slicing` `#slice` `#append`

**โน้ตหลัก:** [Lists_&_Tuples.md](../30_Data_Vault/Lists_&_Tuples.md) — Indexing/Slicing · Built-in Functions

---

## สร้าง List ว่างแล้วค่อยเพิ่มสมาชิก — `list()` + `.append()`

ใช้ **`list()`** สร้าง list ว่าง แล้วใช้ **`.append(ค่า)`** เพิ่มสมาชิกทีละตัว (เพิ่มต่อท้าย)

```python
items = list()
items.append("pen")
items.append("42")
items.append("notebook")

print(items)   # ['pen', '42', 'notebook']
```

เทียบกับเขียนแบบสั้น: `items = []` แล้ว `items.append(...)` ก็ได้ผลเหมือนกัน — `list()` กับ `[]` คือ list ว่างทั้งคู่

---

## สรุปสั้นๆ

สำหรับ **List ของตัวเลข** (หรือ sequence ที่เปรียบเทียบได้) Python มีฟังก์ชัน built-in สี่ตัวที่ใช้บ่อยมาก ไม่ต้องเขียน loop เอง:

| ฟังก์ชัน | ความหมาย | ตัวอย่างผลลัพธ์ |
|----------|----------|------------------|
| **`len(numbers)`** | จำนวนสมาชิกใน list | 4 |
| **`max(numbers)`** | ค่าที่มากที่สุด | 9 |
| **`min(numbers)`** | ค่าที่น้อยที่สุด | 2 |
| **`sum(numbers)`** | ผลรวมทั้งหมด | 22 |

---

## ตัวอย่างจากโจทย์

```python
numbers = [4, 7, 2, 9]

print(len(numbers))   # 4  — มี 4 ตัว
print(max(numbers))   # 9  — มากสุดคือ 9
print(min(numbers))   # 2  — น้อยสุดคือ 2
print(sum(numbers))   # 22 — 4+7+2+9 = 22
```

---

## List Slicing — ตัดช่วง `[start:end]`

ใช้ **slice** `list[start:end]` เพื่อได้ list ชุดใหม่ที่เก็บเฉพาะช่วงจาก index `start` ถึง**ก่อน** `end` (รวม start ไม่รวม end)

```python
numbers = [5, 8, 13, 21, 34]
part = numbers[1:4]   # index 1, 2, 3 (ไม่รวม 4)
print(part)           # [8, 13, 21]
```

| รูปแบบ | ความหมาย | ตัวอย่างจาก `numbers` |
|--------|-----------|------------------------|
| `[1:4]` | ตั้งแต่ index 1 ถึงก่อน 4 | `[8, 13, 21]` |
| `[:3]`  | ตั้งแต่ต้นถึงก่อน index 3 | `[5, 8, 13]` |
| `[2:]`  | ตั้งแต่ index 2 ถึงจบ | `[13, 21, 34]` |

---

## ใช้กับอย่างอื่นได้ด้วย

- **`len()`** ใช้กับ string, tuple, dict, set ได้ (คืนจำนวนสมาชิก/ความยาว)
- **`max()` / `min()`** ใช้กับ sequence ที่เปรียบเทียบกันได้ (ตัวเลข, string ฯลฯ)
- **`sum()`** ใช้กับ iterable ของตัวเลข; ถ้าต้องการค่าเริ่มต้นใช้ `sum(iterable, start=0)`
- **Slicing** ใช้กับ string, tuple ได้เหมือนกัน (ได้ string/tuple ชุดใหม่)

---

## โค้ดอ่านไฟล์เก็บคำไม่ซ้ำจาก `romeo.txt`

**แนวคิด:**  
- รับชื่อไฟล์จากผู้ใช้ → พยายามเปิดไฟล์ (ถ้าเปิดไม่ได้ให้แจ้งเตือนแล้วหยุดโปรแกรม)  
- อ่านไฟล์ทีละบรรทัด → ใช้ `.split()` แตกบรรทัดเป็นคำ  
- เก็บคำลงใน list เฉพาะคำที่ยังไม่เคยมี (กันซ้ำด้วย `if word not in lst`)  
- เรียงลำดับคำด้วย `.sort()` แล้ว `print()` ออกมา

**โค้ดตัวอย่าง:**

```python
fname = input("Enter file name: ")
try:
    fh = open(fname)
except:
    print("File cannot be opened:", fname)
    quit()

words_list = []

for line in fh:
    words = line.split()
    for word in words:
        if word not in words_list:
            words_list.append(word)

words_list.sort()
print(words_list)
```

**สรุปสิ่งที่เรียนรู้จากโค้ดนี้**
- `input()` รับชื่อไฟล์จากผู้ใช้  
- `open()` + `try/except` ใช้เปิดไฟล์พร้อมจัดการกรณีเปิดไม่ได้  
- วนอ่านไฟล์ทีละบรรทัด: `for line in fh:`  
- แตกบรรทัดเป็นคำ: `line.split()`  
- ใช้ list ว่าง + `.append()` เก็บค่าทีละตัว  
- ใช้ `if word not in words_list:` ป้องกันคำซ้ำ  
- ใช้ `.sort()` เรียงคำตามตัวอักษร ก่อนพิมพ์ผลลัพธ์

---

*กลับไป: [README — เกร็ดความรู้](./README.md#-เกร็ดความรู้--สรุปสั้น)*
