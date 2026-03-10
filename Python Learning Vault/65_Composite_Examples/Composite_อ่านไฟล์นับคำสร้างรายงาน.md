# โปรแกรมรวม: อ่านไฟล์ → นับคำ → สร้าง Report → เขียนไฟล์

**หมวด:** 65_Composite_Examples — โปรแกรมประกอบองค์ความรู้  
**ระดับ:** กลาง (Intermediate)  
**วันที่บันทึก:** 2026-03-10  
**Tags:** `#composite` `#file` `#dict` `#string` `#loop` `#learning`

**Concept ที่ใช้:** File I/O · String (split) · Dictionary (นับจำนวน) · Loop · try/except

---

## 🎯 สิ่งที่โปรแกรมนี้ทำ

1. อ่านไฟล์ข้อความ (เช่น romeo.txt)
2. แยกคำออกจากทุกบรรทัด
3. นับว่าคำแต่ละคำปรากฏกี่ครั้ง
4. สร้างรายงานสรุป (คำที่เจอบ่อยที่สุด)
5. เขียนรายงานลงไฟล์ใหม่

---

## 📝 โค้ดเต็ม

```python
# 1. รับชื่อไฟล์และเปิด
fname = input("Enter file name: ")
if len(fname) < 1:
    fname = "romeo.txt"

try:
    fh = open(fname, encoding="utf-8")
except FileNotFoundError:
    print("File cannot be opened:", fname)
    quit()

# 2. นับคำด้วย dict (pattern จาก เกร็ดความรู้_Dict_count_names)
counts = {}
for line in fh:
    line = line.rstrip()
    words = line.split()
    for word in words:
        counts[word] = counts.get(word, 0) + 1

fh.close()

# 3. เรียงตามจำนวน (มากไปน้อย) — ใช้ sorted + lambda
sorted_words = sorted(counts.items(), key=lambda x: x[1], reverse=True)

# 4. สร้างรายงานเป็น string
report_lines = ["=== Word Count Report ===\n"]
for word, count in sorted_words[:10]:  # Top 10
    report_lines.append(f"{word}: {count}\n")

report_text = "".join(report_lines)

# 5. เขียนไฟล์ report
output_name = fname.replace(".txt", "_report.txt")
with open(output_name, "w", encoding="utf-8") as out:
    out.write(report_text)

print(f"Report saved to {output_name}")
```

---

## 🧩 แผนภาพ: Concept ไหนมาจากโน้ตไหน

| บรรทัด / ส่วน | Concept | โน้ต / เกร็ดความรู้ |
|---------------|---------|---------------------|
| `open`, `try/except`, `close` | เปิดไฟล์ + จัดการ error | [File_Operations](../50_Automation_Shadows/File_Operations.md) · [เกร็ดความรู้ mbox](../60_Learning_Walkthroughs/เกร็ดความรู้_mbox_From_lines.md) |
| `line.rstrip()`, `line.split()` | String methods | [String_Manipulation](../10_Universal_Laws/String_Manipulation.md) · [เกร็ดความรู้ romeo](../60_Learning_Walkthroughs/เกร็ดความรู้_List_len_max_min_sum.md) |
| `counts.get(word, 0) + 1` | Dict นับจำนวนซ้ำ | [เกร็ดความรู้_Dict_count_names](../60_Learning_Walkthroughs/เกร็ดความรู้_Dict_count_names.md) |
| `for line in fh`, `for word in words` | Loop อ่านไฟล์ + วนคำ | [Loops_For_While](../20_Logic_Flow/Loops_For_While.md) |
| `sorted(..., key=lambda ...)` | เรียงลำดับ + lambda | [Function_Definitions](../40_Architect_Tools/Function_Definitions.md) · [Dictionaries](../30_Data_Vault/Dictionaries.md) |
| `with open(..., "w")` | เขียนไฟล์ | [File_Operations](../50_Automation_Shadows/File_Operations.md) |

---

## 💡 อธิบายสั้น ๆ — ทำไมต้องประกอบหลาย concept

- **อ่านไฟล์** → ได้ string ของแต่ละบรรทัด
- **String split** → แปลงบรรทัดเป็น list ของคำ
- **Dict + get** → นับจำนวนซ้ำได้ง่าย ไม่ต้องเขียน if-else
- **sorted + lambda** → เรียง dict ตาม value (จำนวน)
- **เขียนไฟล์** → บันทึกผลลัพธ์ให้ใช้ต่อได้

โปรแกรมจริงแทบทุกตัวใช้ pattern แบบนี้: **อ่านข้อมูล → ประมวลผล (ใช้หลาย concept) → เขียนผลลัพธ์**

---

## 🔗 ลิงก์ที่เกี่ยวข้อง

- [Concept_Connections](../00_Central_Core/Concept_Connections.md) — แผนที่ความเชื่อมโยง
- [Learning_Path](../00_Central_Core/Learning_Path.md) — ขั้นที่ 10 โปรแกรมรวม
- [เกร็ดความรู้_Dict_count_names](../60_Learning_Walkthroughs/เกร็ดความรู้_Dict_count_names.md)
- [เกร็ดความรู้_List_len_max_min_sum](../60_Learning_Walkthroughs/เกร็ดความรู้_List_len_max_min_sum.md)
- [File_Operations](../50_Automation_Shadows/File_Operations.md)

---

*กลับไป: [README — Composite Examples](./README.md)*
