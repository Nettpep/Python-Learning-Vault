# CLI Tools — สร้างโปรแกรม Command Line

**หมวด:** 50_Automation_Shadows — เครื่องมืออัตโนมัติ  
**ระดับ:** กลาง–สูง (Intermediate–Advanced)  
**Tags:** `#cli` `#argparse` `#click` `#command-line`

---

## 🎯 สรุปสั้น ๆ

- **argparse** — โมดูลมาตรฐาน รับ argument จาก command line
- **click** — library ยอดนิยม เขียนสั้นกว่า argparse
- **ใช้เมื่อ:** สร้าง script ให้รันจาก terminal พร้อม options (เช่น `python script.py --input file.txt --verbose`)

---

## argparse พื้นฐาน

```python
import argparse

parser = argparse.ArgumentParser(description="Process some files.")
parser.add_argument("input", help="Input file path")
parser.add_argument("-o", "--output", help="Output file path")
parser.add_argument("-v", "--verbose", action="store_true", help="Verbose mode")

args = parser.parse_args()
print(f"Input: {args.input}, Output: {args.output}, Verbose: {args.verbose}")
```

รัน: `python script.py data.txt -o result.txt -v`

---

## click พื้นฐาน

```python
import click

@click.command()
@click.argument("input", type=click.Path(exists=True))
@click.option("-o", "--output", help="Output file path")
@click.option("-v", "--verbose", is_flag=True, help="Verbose mode")
def main(input, output, verbose):
    click.echo(f"Input: {input}, Output: {output}, Verbose: {verbose}")

if __name__ == "__main__":
    main()
```

---

## 📌 สถานะ

โน้ตนี้เป็น **โครงเบื้องต้น** — จะขยายเมื่อมี script ที่ใช้ CLI

**ลิงก์ที่เกี่ยวข้อง:** [File_Operations](./File_Operations.md) · [Function_Definitions](../40_Architect_Tools/Function_Definitions.md)

---

*กลับไป: [Index_MOC](../00_Central_Core/Index_MOC.md) · [Learning_Path](../00_Central_Core/Learning_Path.md)*
