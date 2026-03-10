# โปรแกรมรวม: Parse Log File → สรุปสถิติ → เขียน JSON

**หมวด:** 65_Composite_Examples — โปรแกรมประกอบองค์ความรู้  
**ระดับ:** กลาง–สูง (Intermediate–Advanced)  
**วันที่บันทึก:** 2026-03-10  
**Tags:** `#composite` `#file` `#parsing` `#dict` `#json` `#learning`

**Concept ที่ใช้:** File I/O · String (parsing, split, startswith) · Dictionary · JSON · Loop · Function

---

## 🎯 สิ่งที่โปรแกรมนี้ทำ

1. อ่านไฟล์ log (รูปแบบคล้าย mbox หรือ log ทั่วไป)
2. Parse บรรทัดที่สนใจ (เช่น ขึ้นต้นด้วย "From " หรือ "ERROR")
3. สรุปสถิติ (จำนวน email แต่ละคน, จำนวน error ต่อวัน ฯลฯ)
4. เขียนผลลัพธ์เป็น JSON

---

## 📝 โค้ดเต็ม

```python
import json
from pathlib import Path

def parse_mbox_emails(filepath):
    """อ่านไฟล์ mbox แล้วคืน dict ของ {email: จำนวนครั้ง}"""
    counts = {}
    with open(filepath, encoding="utf-8") as fh:
        for line in fh:
            line = line.rstrip()
            if not line.startswith("From "):
                continue
            words = line.split()
            if len(words) >= 2:
                email = words[1]
                counts[email] = counts.get(email, 0) + 1
    return counts

def main():
    fname = input("Enter file name: ")
    if len(fname) < 1:
        fname = "mbox-short.txt"

    try:
        stats = parse_mbox_emails(fname)
    except FileNotFoundError:
        print("File cannot be opened:", fname)
        return

    # เรียงตามจำนวนมากไปน้อย
    sorted_stats = dict(
        sorted(stats.items(), key=lambda x: x[1], reverse=True)[:5]
    )

    # เขียน JSON
    output_path = Path(fname).stem + "_stats.json"
    with open(output_path, "w", encoding="utf-8") as f:
        json.dump(sorted_stats, f, indent=2, ensure_ascii=False)

    print(f"Stats saved to {output_path}")
    print(json.dumps(sorted_stats, indent=2, ensure_ascii=False))

if __name__ == "__main__":
    main()
```

---

## 🧩 แผนภาพ: Concept ไหนมาจากโน้ตไหน

| ส่วน | Concept | โน้ต / เกร็ดความรู้ |
|------|---------|---------------------|
| `with open`, `for line in fh` | อ่านไฟล์ทีละบรรทัด | [File_Operations](../50_Automation_Shadows/File_Operations.md) |
| `line.rstrip()`, `startswith("From ")`, `split()` | Parse บรรทัด | [เกร็ดความรู้ mbox](../60_Learning_Walkthroughs/เกร็ดความรู้_mbox_From_lines.md) |
| `counts.get(email, 0) + 1` | นับจำนวนซ้ำ | [เกร็ดความรู้_Dict_count_names](../60_Learning_Walkthroughs/เกร็ดความรู้_Dict_count_names.md) |
| `def parse_mbox_emails(...)` | แยก logic เป็นฟังก์ชัน | [Function_Definitions](../40_Architect_Tools/Function_Definitions.md) |
| `json.dump()` | เขียน JSON | [File_Operations](../50_Automation_Shadows/File_Operations.md) (section JSON) |
| `Path(fname).stem` | pathlib | [File_Operations](../50_Automation_Shadows/File_Operations.md) |

---

## 💡 ทำไมต้องแยกฟังก์ชัน

- โปรแกรมใหญ่ขึ้น → แยก logic เป็นฟังก์ชันให้อ่านง่าย ทดสอบง่าย
- `parse_mbox_emails` รับ path คืน dict — ใช้ซ้ำได้กับไฟล์อื่น
- `main()` ดูแล flow หลัก: รับ input → เรียก parse → เขียนผล

---

## 🔗 ลิงก์ที่เกี่ยวข้อง

- [Concept_Connections](../00_Central_Core/Concept_Connections.md)
- [Learning_Path](../00_Central_Core/Learning_Path.md)
- [เกร็ดความรู้_mbox_From_lines](../60_Learning_Walkthroughs/เกร็ดความรู้_mbox_From_lines.md)
- [File_Operations](../50_Automation_Shadows/File_Operations.md)

---

*กลับไป: [README — Composite Examples](./README.md)*
