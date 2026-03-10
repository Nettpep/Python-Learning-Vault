# เกร็ดความรู้: mbox — หาคนที่ส่งอีเมลมากที่สุด (โจทย์ 9.4)

**หมวด:** 60_Learning_Walkthroughs — File + Dict + Loop  
**ประเภท:** เกร็ดความรู้ / สรุปโจทย์  
**วันที่บันทึก:** 2026-03-11  
**Tags:** `#learning` `#file` `#dict` `#count` `#max-loop` `#mbox` `#startswith` `#split`

**โน้ตหลัก:** [Dictionaries](../30_Data_Vault/Dictionaries.md) · [File_Operations](../50_Automation_Shadows/File_Operations.md)

---

## โจทย์ (9.4)

Write a program to read through the mbox-short.txt and figure out who has sent the greatest number of mail messages. The program looks for 'From ' lines and takes the second word of those lines as the person who sent the mail. The program creates a Python dictionary that maps the sender's mail address to a count of the number of times they appear in the file. After the dictionary is produced, the program reads through the dictionary using a **maximum loop** to find the most prolific committer.

---

## โค้ดโจทย์ / โค้ดตัวอย่าง

```python
fname = input("Enter file name: ")
if len(fname) < 1:
    fname = "mbox-short.txt"

try:
    fh = open(fname)
except:
    print("File cannot be opened:", fname)
    quit()

# สร้าง dict mapping email -> จำนวนครั้งที่ส่ง
counts = {}

for line in fh:
    line = line.rstrip()
    if not line.startswith('From '):
        continue

    words = line.split()
    email = words[1]
    counts[email] = counts.get(email, 0) + 1

# Maximum loop — หา email ที่มี count สูงสุด
max_count = None
max_email = None

for email, count in counts.items():
    if max_count is None or count > max_count:
        max_count = count
        max_email = email

print(max_email, max_count)
```

ผลลัพธ์ (ตัวอย่าง):

```
cwen@iupui.edu 5
```

---

## อธิบายการแก้ไขโจทย์ทีละช่วง

### ขั้นที่ 1: อ่านไฟล์ + กรองบรรทัด From

- รับชื่อไฟล์จากผู้ใช้ ถ้ากด Enter เปล่า → ใช้ `mbox-short.txt`
- เปิดไฟล์ด้วย `open()` ใน `try/except` เพื่อจัดการกรณีไฟล์ไม่มี
- วนอ่านทีละบรรทัดด้วย `for line in fh:`
- ใช้ `line.rstrip()` ลบ whitespace ท้ายบรรทัด
- กรองเฉพาะบรรทัดที่ขึ้นต้นด้วย `'From '` (มีช่องว่าง) ด้วย `startswith('From ')` และ `continue` ข้ามบรรทัดอื่น

### ขั้นที่ 2: ดึง email และนับด้วย Dict

- บรรทัดที่ผ่านเงื่อนไขมีรูปแบบ: `From stephen.marquard@uct.ac.za Sat Jan 5 09:14:16 2008`
- `words = line.split()` → ได้ list เช่น `['From', 'stephen.marquard@uct.ac.za', 'Sat', ...]`
- `email = words[1]` — คำที่สองคือ email address
- **Pattern นับจำนวนซ้ำ:** `counts[email] = counts.get(email, 0) + 1`
  - ถ้า email ยังไม่มีใน dict → `.get(email, 0)` คืน 0 → บรรทัดนี้จะได้ 1
  - ถ้ามีแล้ว → คืนค่าปัจจุบัน → บรรทัดนี้จะเพิ่มขึ้น 1

### ขั้นที่ 3: Maximum loop — หาคนที่ส่งมากที่สุด

- ต้องหาคู่ `(email, count)` ที่ `count` สูงสุด
- ใช้ตัวแปร `max_count` และ `max_email` เก็บค่าที่มากที่สุดที่เคยเจอ
- วน `for email, count in counts.items():` ทีละคู่
- ถ้า `max_count is None` (ยังไม่เคยตั้งค่า) หรือ `count > max_count` → อัปเดต `max_email` และ `max_count`
- สุดท้ายพิมพ์ `max_email` กับ `max_count`

---

## สรุป Key Concepts

- **อ่านไฟล์ + กรองบรรทัด:** `for line in fh` + `startswith('From ')` + `continue`
- **ดึงคำจากบรรทัด:** `split()` → `words[1]` สำหรับ email
- **นับจำนวนซ้ำด้วย Dict:** `counts[key] = counts.get(key, 0) + 1` — pattern มาตรฐาน
- **Maximum loop ใน Dict:** วน `counts.items()` แล้วเปรียบเทียบ value หาค่าสูงสุด เก็บทั้ง key และ value ที่ชนะ
- โจทย์นี้รวม 3 concept: **File I/O** + **Dict frequency count** + **Maximum loop**

---

## ลิงก์ที่เกี่ยวข้อง

- [เกร็ดความรู้_mbox_From_lines](./เกร็ดความรู้_mbox_From_lines.md) — อ่านบรรทัด From พื้นฐาน (ยังไม่ใช้ dict)
- [เกร็ดความรู้_Dict_count_names](./เกร็ดความรู้_Dict_count_names.md) — Pattern นับจำนวนซ้ำด้วย dict
- [For_Loop_หาค่าสูงสุด](./For_Loop_หาค่าสูงสุด.md) — Maximum loop แบบ list
- [File_Operations](../50_Automation_Shadows/File_Operations.md)
- [Dictionaries](../30_Data_Vault/Dictionaries.md)

---

*กลับไป: [README — เกร็ดความรู้](./README.md#-เกร็ดความรู้--สรุปสั้น)*
