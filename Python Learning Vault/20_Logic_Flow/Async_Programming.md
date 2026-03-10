# Async Programming — การเขียนโปรแกรมแบบไม่บล็อก

**หมวด:** 20_Logic_Flow — เครื่องมือระดับโปร  
**ระดับ:** สูง (Advanced)  
**Tags:** `#async` `#await` `#asyncio` `#concurrency`

---

## 🎯 สรุปสั้น ๆ

- **async / await** — เขียนโค้ดที่รอ I/O (เช่น เรียก API, อ่านไฟล์) โดยไม่บล็อก thread
- **asyncio** — โมดูลมาตรฐานสำหรับรัน coroutine หลายตัวพร้อมกัน
- **ใช้เมื่อ:** ต้องเรียก API หลายครั้ง, ดาวน์โหลดหลายไฟล์, รอ network มาก

---

## ตัวอย่างพื้นฐาน

```python
import asyncio

async def fetch_url(url):
    # จำลองการรอ I/O
    await asyncio.sleep(1)
    return f"Data from {url}"

async def main():
    tasks = [fetch_url(f"https://example.com/{i}") for i in range(3)]
    results = await asyncio.gather(*tasks)
    print(results)

asyncio.run(main())
```

---

## async vs sync

| | sync (ปกติ) | async |
|--|-------------|-------|
| รอ I/O | บล็อกทั้งโปรแกรม | สลับไปทำงานอื่น |
| ใช้เมื่อ | โปรแกรมเล็ก, I/O น้อย | เรียก API/ไฟล์หลายจุดพร้อมกัน |

---

## 📌 สถานะ

โน้ตนี้เป็น **โครงเบื้องต้น** — จะขยายเมื่อมีโปรเจกต์ที่ใช้ asyncio

**ลิงก์ที่เกี่ยวข้อง:** [API_Connections](../50_Automation_Shadows/API_Connections.md) · [Function_Definitions](../40_Architect_Tools/Function_Definitions.md) (generators)

---

*กลับไป: [Index_MOC](../00_Central_Core/Index_MOC.md) · [Learning_Path](../00_Central_Core/Learning_Path.md)*
