% เกร็ดความรู้: อ่านบรรทัด From จากไฟล์ mbox

**หมวด:** 60_Learning_Walkthroughs — Files / Text Processing  
**ประเภท:** เกร็ดความรู้ / สรุปโจทย์  
**วันที่บันทึก:** 2026-03-10  
**Tags:** `#learning` `#file` `#string` `#startswith` `#split` `#loop`

---

## โค้ดโจทย์

```python
fname = input("Enter file name: ")
if len(fname) < 1:
    fname = "mbox-short.txt"

try:
    fh = open(fname)
except:
    print("File cannot be opened:", fname)
    quit()

count = 0

for line in fh:
    line = line.rstrip()

    if not line.startswith('From '):
        continue

    words = line.split()
    print(words[1])
    count = count + 1

print("There were", count, "lines in the file with From as the first word")
```

---

## อธิบายการทำงานแบบสั้น ๆ ทีละช่วง

- **รับชื่อไฟล์ + ตั้งค่า default**
  - `fname = input("Enter file name: ")` รับชื่อไฟล์จากผู้ใช้  
  - `if len(fname) < 1:` ถ้าผู้ใช้กด Enter เปล่า ๆ (ความยาวชื่อไฟล์ < 1)  
    - ให้ตั้งชื่อไฟล์เป็น `"mbox-short.txt"` อัตโนมัติ (ไม่ต้องพิมพ์เองทุกครั้ง)

- **เปิดไฟล์พร้อมจัดการกรณี error**
  - `fh = open(fname)` พยายามเปิดไฟล์  
  - ถ้าเปิดไม่ได้ ให้พิมพ์ `"File cannot be opened: ..."` แล้ว `quit()` หยุดโปรแกรม

- **เตรียมตัวนับจำนวนบรรทัดที่สนใจ**
  - `count = 0` ตัวนับจำนวนบรรทัดที่ขึ้นต้นด้วย `From `

- **วนอ่านไฟล์ทีละบรรทัด**
  - `for line in fh:` อ่านไฟล์ทีละบรรทัดเก็บในตัวแปร `line`
  - `line = line.rstrip()` ลบ whitespace (เช่น `\n`, space ท้ายบรรทัด) ออก เพื่อให้เช็ค string ได้ตรง ๆ

- **กรองเฉพาะบรรทัดที่เริ่มด้วย `From `**
  - `if not line.startswith('From '): continue`  
    - ถ้าบรรทัด **ไม่** เริ่มด้วย `'From '` ให้ข้ามไปบรรทัดถัดไปทันที  
    - ใช้ `'From '` (มีช่องว่าง) เพื่อกันสับสนกับ `From:` ที่เป็น header แบบอื่น

- **แยกคำและดึง email ออกมา**
  - `words = line.split()` แยกบรรทัดที่ผ่านเงื่อนไขแล้วออกเป็น list ของคำ  
    - ตัวอย่างบรรทัด:  
      `From stephen.marquard@uct.ac.za Sat Jan  5 09:14:16 2008`  
      จะกลายเป็น  
      `['From', 'stephen.marquard@uct.ac.za', 'Sat', 'Jan', '5', '09:14:16', '2008']`
  - `print(words[1])` พิมพ์คำตำแหน่งที่ 2 (index 1) ซึ่งคือ **Email Address**

- **เพิ่มตัวนับบรรทัดที่ match เงื่อนไข**
  - `count = count + 1` ทุกครั้งที่เจอบรรทัด `From ...` ให้เพิ่มตัวนับ 1

- **พิมพ์สรุปท้ายโปรแกรม**
  - `print("There were", count, "lines in the file with From as the first word")`  
    สรุปว่าพบกี่บรรทัดที่ขึ้นต้นด้วยคำว่า `From` (มีช่องว่าง)

---

## สรุป Key Concepts จากโจทย์นี้

- การตั้งค่า **default file name** เมื่อผู้ใช้กด Enter เปล่า ๆ (`len(fname) < 1`)
- การเปิดไฟล์ด้วย `open()` ร่วมกับ `try/except` เพื่อจัดการ error
- การวนอ่านไฟล์ทีละบรรทัดด้วย `for line in fh:`
- ใช้ `rstrip()` ลบ whitespace ท้ายบรรทัดก่อนตรวจสอบ / แยกคำ
- การกรองบรรทัดด้วย `startswith('From ')` และใช้ `continue` ข้ามบรรทัดที่ไม่ต้องการ
- การใช้ `split()` เพื่อแยก string เป็น list ของคำ แล้วเข้าถึงด้วย index (`words[1]`)
- การใช้ตัวแปรตัวนับ (`count`) เพื่อเก็บจำนวนบรรทัดที่เข้าเงื่อนไข

