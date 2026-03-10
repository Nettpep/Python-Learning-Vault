# Logging & Debugging — บันทึกและแก้จุดบกพร่อง

**หมวด:** 20_Logic_Flow — เครื่องมือระดับโปร  
**ระดับ:** กลาง–สูง (Intermediate–Advanced)  
**Tags:** `#logging` `#debug` `#print` `#breakpoint`

---

## 🎯 สรุปสั้น ๆ

- **print()** — ใช้ debug เร็ว ๆ แต่ไม่เหมาะกับโปรแกรมจริง
- **logging** — บันทึกระดับ (DEBUG, INFO, WARNING, ERROR) ควบคุมได้
- **breakpoint()** — หยุดรันที่จุดที่กำหนด ใช้ debugger ตรวจสอบตัวแปร

---

## logging พื้นฐาน

```python
import logging

logging.basicConfig(
    level=logging.DEBUG,
    format="%(asctime)s - %(levelname)s - %(message)s"
)
logger = logging.getLogger(__name__)

logger.debug("ค่า x = %s", x)
logger.info("ประมวลผลเสร็จ")
logger.warning("ไฟล์ไม่พบ ใช้ค่า default")
logger.error("เกิดข้อผิดพลาด: %s", str(e))
```

---

## breakpoint() และ pdb

```python
def process(data):
    result = []
    for item in data:
        breakpoint()  # หยุดตรงนี้ ตรวจสอบ item, result
        result.append(item.upper())
    return result
```

เมื่อหยุด: ใช้ `p` ดูค่า (เช่น `p item`), `n` ข้ามบรรทัดถัดไป, `c` ทำงานต่อ

---

## 📌 สถานะ

โน้ตนี้เป็น **โครงเบื้องต้น** — จะขยายเมื่อมี use case จริง

**ลิงก์ที่เกี่ยวข้อง:** [Error_Handling](./Error_Handling.md) · [Quick_Snippets](../00_Central_Core/Quick_Snippets.md) (section Debug)

---

*กลับไป: [Index_MOC](../00_Central_Core/Index_MOC.md)*
