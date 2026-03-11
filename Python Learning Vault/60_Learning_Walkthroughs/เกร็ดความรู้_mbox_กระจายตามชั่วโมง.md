# เกร็ดความรู้: mbox — กระจายจำนวนอีเมลตามชั่วโมง (โจทย์ 10.2)

**หมวด:** 60_Learning_Walkthroughs — File + Dict + String Parsing  
**ประเภท:** เกร็ดความรู้ / สรุปโจทย์  
**วันที่บันทึก:** 2026-03-11  
**Tags:** `#learning` `#file` `#dict` `#split` `#mbox` `#parsing` `#colon`

**โน้ตหลัก:** [Dictionaries](../30_Data_Vault/Dictionaries.md) · [File_Operations](../50_Automation_Shadows/File_Operations.md)

---

## โจทย์ (10.2)

Write a program to read through the mbox-short.txt and figure out the **distribution by hour of the day** for each of the messages. You can pull the hour out from the 'From ' line by finding the time and then splitting the string a second time using a colon. Once you have accumulated the counts for each hour, print out the counts, **sorted by hour**.

ตัวอย่างบรรทัด:
```
From stephen.marquard@uct.ac.za Sat Jan  5 09:14:16 2008
```

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

counts = {}

for line in fh:
    line = line.rstrip()
    if not line.startswith('From '):
        continue

    words = line.split()
    time_str = words[5]           # เช่น '09:14:16'
    hour = time_str.split(':')[0] # ดึง '09'

    counts[hour] = counts.get(hour, 0) + 1

# เรียงตาม hour (key) แล้วพิมพ์
for hour in sorted(counts.keys()):
    print(hour, counts[hour])
```

ผลลัพธ์ (ตัวอย่าง):

```
04 3
06 1
07 1
09 2
10 3
11 6
14 1
15 2
16 4
17 2
18 1
19 1
```

---

## อธิบายการแก้ไขโจทย์ทีละช่วง

### ขั้นที่ 1: อ่านไฟล์ + กรองบรรทัด From

- เหมือนโจทย์ mbox อื่น ๆ — รับชื่อไฟล์, เปิดด้วย try/except, วน `for line in fh:`
- กรองเฉพาะบรรทัด `startswith('From ')` ด้วย `continue`

### ขั้นที่ 2: ดึงชั่วโมงจาก time — split สองครั้ง

- บรรทัด: `From stephen.marquard@uct.ac.za Sat Jan  5 09:14:16 2008`
- **ครั้งที่ 1 — split ด้วยช่องว่าง:** `words = line.split()`  
  → `['From', 'stephen.marquard@uct.ac.za', 'Sat', 'Jan', '5', '09:14:16', '2008']`
- `words[5]` = `'09:14:16'` — นี่คือ time
- **ครั้งที่ 2 — split ด้วยโคลอน:** `time_str.split(':')`  
  → `['09', '14', '16']`
- `split(':')[0]` = `'09'` — ชั่วโมง

### ขั้นที่ 3: นับด้วย Dict + เรียงตาม hour

- **Pattern นับ:** `counts[hour] = counts.get(hour, 0) + 1`
- **เรียงตาม key (hour):** `sorted(counts.keys())` — hour เป็น string แบบ `'04'`, `'09'`, `'18'` การเรียงแบบ string จะได้ลำดับถูกต้อง (04 < 09 < 18) เพราะมี leading zero

---

## สรุป Key Concepts

- **Parsing แบบซ้อน:** `split()` ครั้งแรกแยกคำ → ครั้งที่สองแยก time ด้วย `split(':')`
- **ดึงชั่วโมงจาก time:** `words[5]` คือ time → `words[5].split(':')[0]` คือ hour
- **นับจำนวนซ้ำ:** `counts[key] = counts.get(key, 0) + 1`
- **เรียง dict ตาม key:** `sorted(counts.keys())` — ใช้ได้เมื่อ key เป็น string ที่เรียงตามลำดับได้ (เช่น hour มี leading zero)

---

## ลิงก์ที่เกี่ยวข้อง

- [เกร็ดความรู้_mbox_From_lines](./เกร็ดความรู้_mbox_From_lines.md) — โครงสร้างบรรทัด From
- [เกร็ดความรู้_mbox_หาคนส่งมากที่สุด](./เกร็ดความรู้_mbox_หาคนส่งมากที่สุด.md) — mbox + dict นับ (โจทย์ 9.4)
- [เกร็ดความรู้_Dict_count_names](./เกร็ดความรู้_Dict_count_names.md) — Pattern นับด้วย dict
- [Parsing_ดึงค่าหลังโคลอน_เป็น_float](./Parsing_ดึงค่าหลังโคลอน_เป็น_float.md) — Parsing ด้วย split/colon
- [File_Operations](../50_Automation_Shadows/File_Operations.md)

---

*กลับไป: [README — เกร็ดความรู้](./README.md#-เกร็ดความรู้--สรุปสั้น)*
