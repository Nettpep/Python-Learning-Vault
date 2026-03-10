# Concept Connections — แผนที่ความเชื่อมโยงองค์ความรู้ Python

**จุดประสงค์:** เห็นภาพว่าแต่ละ concept เชื่อมโยงกับอะไรบ้าง และเมื่อเขียนโปรแกรมจริง ต้องนำองค์ความรู้หลายอย่างมาประกอบกันอย่างไร

---

## 🗺️ แผนที่ความเชื่อมโยงแบบภาพรวม

```
                    ┌─────────────────┐
                    │   Variables &   │
                    │     Types       │
                    └────────┬────────┘
                             │
        ┌────────────────────┼────────────────────┐
        ▼                    ▼                    ▼
┌───────────────┐   ┌───────────────┐   ┌───────────────┐
│   Operators   │   │    String     │   │  Data Vault   │
│  + - * / in   │   │  split, strip │   │ List Dict Set │
└───────┬───────┘   └───────┬───────┘   └───────┬───────┘
        │                  │                   │
        └──────────────────┼───────────────────┘
                           ▼
                ┌─────────────────────┐
                │   Logic Flow        │
                │ if/else · for/while │
                │ try/except          │
                └──────────┬──────────┘
                           │
        ┌──────────────────┼──────────────────┐
        ▼                  ▼                  ▼
┌───────────────┐   ┌───────────────┐   ┌───────────────┐
│  Functions    │   │  File I/O     │   │  API / AI      │
│ def · lambda  │   │ open · JSON   │   │ requests       │
│ decorators    │   │ pathlib       │   │ OpenAI         │
└───────────────┘   └───────────────┘   └───────────────┘
        │                  │                   │
        └──────────────────┼───────────────────┘
                           ▼
                ┌─────────────────────┐
                │  Composite Program   │
                │  โปรแกรมรวมหลาย concept │
                └─────────────────────┘
```

---

## 📊 ตารางความเชื่อมโยง — โจทย์/โปรแกรมใช้ concept อะไรบ้าง

| โจทย์ / โปรแกรม | Variables | Operators | String | List/Dict | Loop | if/else | try/except | File | Function |
|-----------------|:---------:|:---------:|:------:|:---------:|:----:|:-------:|:----------:|:----:|:--------:|
| หาค่าสูงสุดใน list | ✓ | ✓ | | ✓ | ✓ | ✓ | | | |
| นับคำซ้ำ (dict) | ✓ | | | ✓ | ✓ | ✓ | | | |
| อ่านไฟล์ mbox ดึง email | ✓ | | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | |
| mbox หาคนส่งมากที่สุด (dict + max loop) | ✓ | | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | |
| เก็บคำไม่ซ้ำจากไฟล์ (romeo) | ✓ | | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | |
| Parse ค่าหลังโคลอน | ✓ | | ✓ | ✓ | | ✓ | | | |
| อ่าน JSON → นับ → เขียน report | ✓ | | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |

---

## 🔗 ความเชื่อมโยงแบบละเอียด — แต่ละ Concept ใช้ร่วมกับอะไร

### String
- **ใช้ร่วมกับ:** Loop (อ่านทีละตัว/ทีละคำ), List (split → list), Dict (key เป็น string), File (อ่านบรรทัดเป็น string)
- **โน้ต:** [String_Manipulation](../10_Universal_Laws/String_Manipulation.md)
- **เกร็ด:** [เกร็ดความรู้_สตริง](../60_Learning_Walkthroughs/เกร็ดความรู้_สตริง_สรุปหลักสำคัญ.md) · [Parsing หลังโคลอน](../60_Learning_Walkthroughs/Parsing_ดึงค่าหลังโคลอน_เป็น_float.md)

### Dict
- **ใช้ร่วมกับ:** Loop (for k,v in dict.items), String (key มักเป็น string), List (values(), keys()), File (เก็บผลลัพธ์ก่อนเขียน)
- **Pattern สำคัญ:** นับจำนวนซ้ำ → `dict.get(key, 0) + 1`
- **โน้ต:** [Dictionaries](../30_Data_Vault/Dictionaries.md)
- **เกร็ด:** [เกร็ดความรู้_Dict_count_names](../60_Learning_Walkthroughs/เกร็ดความรู้_Dict_count_names.md)

### Loop (for / while)
- **ใช้ร่วมกับ:** List, Dict, String, File (for line in fh)
- **Pattern สำคัญ:** Accumulator, หาค่าสูงสุด/ต่ำสุด, กรองด้วย if + continue
- **โน้ต:** [Loops_For_While](../20_Logic_Flow/Loops_For_While.md)
- **เกร็ด:** [For หาค่าสูงสุด](../60_Learning_Walkthroughs/For_Loop_หาค่าสูงสุด.md) · [สะสมค่า](../60_Learning_Walkthroughs/For_Loop_สะสมค่า_Accumulator.md) · [mbox From](../60_Learning_Walkthroughs/เกร็ดความรู้_mbox_From_lines.md)

### File I/O
- **ใช้ร่วมกับ:** String (line.rstrip, line.split), Loop (for line in fh), Dict (เก็บผลก่อนเขียน), try/except (เปิดไฟล์)
- **โน้ต:** [File_Operations](../50_Automation_Shadows/File_Operations.md)
- **เกร็ด:** [สรุปบทเรียน Ch7](../60_Learning_Walkthroughs/สรุปบทเรียน_Ch7_การจัดการไฟล์.md) · [mbox](../60_Learning_Walkthroughs/เกร็ดความรู้_mbox_From_lines.md) · [romeo](../60_Learning_Walkthroughs/เกร็ดความรู้_List_len_max_min_sum.md)

### try/except
- **ใช้ร่วมกับ:** File (open), API (requests), ฟังก์ชันที่อาจ error
- **โน้ต:** [Error_Handling](../20_Logic_Flow/Error_Handling.md)

---

## 🧩 โปรแกรมรวม (Composite) — ประกอบองค์ความรู้หลายอย่าง

โปรแกรมจริงมักใช้หลาย concept พร้อมกัน ดูตัวอย่างที่อธิบายทีละส่วน:

| โปรแกรม | Concept ที่ใช้ | ไฟล์ |
|---------|----------------|------|
| อ่านไฟล์ข้อความ → นับคำแต่ละคำ → สร้าง report → เขียนไฟล์ | File + String (split) + Dict (นับ) + Loop + File (เขียน) | [Composite_อ่านไฟล์นับคำสร้างรายงาน](../65_Composite_Examples/Composite_อ่านไฟล์นับคำสร้างรายงาน.md) |
| Parse log file → สรุปสถิติ → เขียน JSON | File + String (parsing) + Dict + JSON | [Composite_Parse_Log_สรุปสถิติ](../65_Composite_Examples/Composite_Parse_Log_สรุปสถิติ.md) |

---

## 📌 วิธีใช้แผนที่นี้

1. **ก่อนเขียนโปรแกรม** — ดูตาราง "โจทย์ใช้ concept อะไรบ้าง" เพื่อรู้ว่าต้องทบทวนโน้ตไหน
2. **หลังเรียน concept ใหม่** — ดู "ใช้ร่วมกับอะไร" เพื่อเห็นว่าจะเอาไปใช้ที่ไหน
3. **อยากเห็นภาพรวม** — ดูโปรแกรมรวม (Composite) เพื่อเห็นการประกอบองค์ความรู้

---

*กลับไป: [Index_MOC](./Index_MOC.md) · [Learning_Path](./Learning_Path.md)*
