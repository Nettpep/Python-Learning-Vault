# เกร็ดความรู้: Dict — เรียงตาม value ด้วย tuple (score, name)

**หมวด:** 30_Data_Vault — Dict  
**ประเภท:** เกร็ดความรู้ / สรุปสั้น  
**วันที่บันทึก:** 2026-03-11  
**Tags:** `#learning` `#dict` `#sorted` `#tuple` `#sort-by-value`

**โน้ตหลัก:** [Dictionaries](../30_Data_Vault/Dictionaries.md)

---

## โค้ดตัวอย่าง

```python
scores = {"alice": 5, "bob": 2, "carol": 8}

pairs = []
for name, score in scores.items():
    pair = (score, name)
    pairs.append(pair)

result = sorted(pairs)
print(result)
```

ผลลัพธ์:

```python
[(2, 'bob'), (5, 'alice'), (8, 'carol')]
```

---

## อธิบายสั้น ๆ

- **ปัญหา:** `sorted(scores.items())` จะเรียงตาม **key** (ชื่อ) ไม่ใช่ value (คะแนน)
- **เทคนิค:** สร้าง tuple เป็น `(score, name)` — ใส่ **value ก่อน** เพื่อให้ `sorted()` เรียงตามตัวเลข
- **`sorted()` กับ tuple:** เรียงตาม element แรกก่อน ถ้าเท่ากันค่อยดูตัวถัดไป
- ถ้าใช้ `(name, score)` จะได้เรียงตามชื่อ (a-z) แทน

---

## สรุป Key Concepts

- **Pattern เรียง dict ตาม value:** แปลงเป็น list ของ `(value, key)` แล้ว `sorted()`
- **ทำไมต้องสลับ:** เพราะ `sorted()` ใช้ element แรกของ tuple เป็นเกณฑ์เรียง
- **เรียงจากมากไปน้อย:** ใช้ `sorted(pairs, reverse=True)`

---

## รูปแบบย่อ (list comprehension)

```python
pairs = [(score, name) for name, score in scores.items()]
result = sorted(pairs)
```

---

## ลิงก์ที่เกี่ยวข้อง

- [เกร็ดความรู้_Dict_count_names](./เกร็ดความรู้_Dict_count_names.md) — นับจำนวนซ้ำด้วย dict
- [เกร็ดความรู้_mbox_หาคนส่งมากที่สุด](./เกร็ดความรู้_mbox_หาคนส่งมากที่สุด.md) — หาค่าสูงสุดใน dict (maximum loop)
- [Dictionaries](../30_Data_Vault/Dictionaries.md)

---

*กลับไป: [README — เกร็ดความรู้](./README.md#-เกร็ดความรู้--สรุปสั้น)*
