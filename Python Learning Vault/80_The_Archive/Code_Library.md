# Code Library - คลังเก็บ Scripts ที่เขียนเสร็จแล้ว

## 📚 แนวคิดพื้นฐาน

Code Library คือ **คลังเก็บโค้ดที่พร้อมใช้งาน** ที่เราได้พัฒนาขึ้นและทดสอบแล้ว ช่วยให้สามารถนำกลับมาใช้ใหม่ได้อย่างรวดเร็ว

```python
# โครงสร้างพื้นฐานของ library
def utility_function():
    """ฟังก์ชันที่พร้อมใช้งาน"""
    pass

class UsefulClass:
    """คลาสที่พร้อมใช้งาน"""
    pass
```

---

## 🛠️ Utility Functions

### String Utilities
```python
def clean_text(text):
    """ทำความสะอาดข้อความ"""
    import re
    
    # ลบ whitespace พิเศษ
    text = re.sub(r'\s+', ' ', text.strip())
    
    # ลบตัวอักษรพิเศษยกเว้นภาษาไทยและพื้นฐาน
    text = re.sub(r'[^\w\s\u0E00-\u0E7F]', '', text)
    
    return text.lower()

def format_thai_currency(amount):
    """จัดรูปแบบจำนวนเงินเป็นภาษาไทย"""
    try:
        amount = float(amount)
        formatted = f"{amount:,.2f}"
        return f"฿{formatted}"
    except (ValueError, TypeError):
        return "฿0.00"

def extract_emails(text):
    """ดึง email addresses จากข้อความ"""
    import re
    pattern = r'\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Z|a-z]{2,}\b'
    return re.findall(pattern, text)

def generate_slug(text):
    """สร้าง slug จากข้อความ"""
    import re
    
    # แปลงเป็นพิมพ์เล็กและแทน space ด้วย dash
    slug = text.lower()
    slug = re.sub(r'[^\w\s-]', '', slug)
    slug = re.sub(r'[\s_-]+', '-', slug)
    slug = slug.strip('-')
    
    return slug

# ทดสอบ
if __name__ == "__main__":
    print(clean_text("  Hello,   World!  สวัสดีครับ  "))
    print(format_thai_currency(12345.67))
    print(extract_emails("Contact us at info@example.com or support@test.org"))
    print(generate_slug("Python Programming Tutorial"))
```

### Date/Time Utilities
```python
from datetime import datetime, timedelta
import calendar

def get_thai_month_name(month_number):
    """ดึงชื่อเดือนภาษาไทย"""
    thai_months = [
        "มกราคม", "กุมภาพันธ์", "มีนาคม", "เมษายน",
        "พฤษภาคม", "มิถุนายน", "กรกฎาคม", "สิงหาคม",
        "กันยายน", "ตุลาคม", "พฤศจิกายน", "ธันวาคม"
    ]
    
    if 1 <= month_number <= 12:
        return thai_months[month_number - 1]
    return "ไม่ทราบ"

def format_thai_date(date_obj, format_type="full"):
    """จัดรูปแบบวันที่ภาษาไทย"""
    if format_type == "full":
        thai_year = date_obj.year + 543
        month_name = get_thai_month_name(date_obj.month)
        return f"{date_obj.day} {month_name} {thai_year}"
    elif format_type == "short":
        return date_obj.strftime("%d/%m/%Y")
    else:
        return date_obj.strftime("%Y-%m-%d")

def get_business_days(start_date, end_date):
    """คำนวณีวันทำการ (จันทร์-ศุกร์)"""
    business_days = []
    current = start_date
    
    while current <= end_date:
        if current.weekday() < 5:  # 0-4 = จันทร์-ศุกร์
            business_days.append(current)
        current += timedelta(days=1)
    
    return business_days

def calculate_age(birth_date):
    """คำนวณอายุจากวันเกิด"""
    today = datetime.now()
    age = today.year - birth_date.year
    
    # ตรวจสอบว่าผ่านวันเกิดปีนี้หรือยัง
    if today.month < birth_date.month or \
       (today.month == birth_date.month and today.day < birth_date.day):
        age -= 1
    
    return age

def get_week_range(date_obj):
    """ดึงช่วงวันของสัปดาห์"""
    start_of_week = date_obj - timedelta(days=date_obj.weekday())
    end_of_week = start_of_week + timedelta(days=6)
    
    return {
        "start": start_of_week,
        "end": end_of_week,
        "week_number": date_obj.isocalendar()[1]
    }

# ทดสอบ
if __name__ == "__main__":
    today = datetime.now()
    print(get_thai_month_name(today.month))
    print(format_thai_date(today))
    
    birth = datetime(1990, 5, 15)
    print(f"อายุ: {calculate_age(birth)} ปี")
    
    week_info = get_week_range(today)
    print(f"สัปดาห์ที่ {week_info['week_number']}: {week_info['start']} ถึง {week_info['end']}")
```

### File Utilities
```python
import os
import shutil
from pathlib import Path
import hashlib
import json

def get_file_size(file_path, unit="KB"):
    """ดูขนาดไฟล์"""
    path = Path(file_path)
    if not path.exists():
        return "File not found"
    
    size_bytes = path.stat().st_size
    
    units = {
        "B": size_bytes,
        "KB": size_bytes / 1024,
        "MB": size_bytes / (1024 * 1024),
        "GB": size_bytes / (1024 * 1024 * 1024)
    }
    
    return f"{units.get(unit, size_bytes):.2f} {unit}"

def calculate_file_hash(file_path, algorithm="md5"):
    """คำนวณ hash ของไฟล์"""
    path = Path(file_path)
    if not path.exists():
        return None
    
    hash_func = hashlib.new(algorithm)
    
    with open(path, 'rb') as file:
        for chunk in iter(lambda: file.read(4096), b""):
            hash_func.update(chunk)
    
    return hash_func.hexdigest()

def find_duplicate_files(directory):
    """หาไฟล์ซ้ำในโฟลเดอร์"""
    file_hashes = {}
    duplicates = []
    
    for file_path in Path(directory).rglob("*"):
        if file_path.is_file():
            file_hash = calculate_file_hash(file_path)
            
            if file_hash in file_hashes:
                duplicates.append({
                    "original": file_hashes[file_hash],
                    "duplicate": str(file_path),
                    "hash": file_hash
                })
            else:
                file_hashes[file_hash] = str(file_path)
    
    return duplicates

def organize_files_by_type(source_dir, target_dir):
    """จัดระเบียบไฟล์ตามประเภท"""
    file_types = {
        "images": [".jpg", ".jpeg", ".png", ".gif", ".bmp"],
        "documents": [".pdf", ".doc", ".docx", ".txt", ".rtf"],
        "videos": [".mp4", ".avi", ".mov", ".wmv", ".flv"],
        "audio": [".mp3", ".wav", ".flac", ".aac", ".ogg"],
        "archives": [".zip", ".rar", ".7z", ".tar", ".gz"]
    }
    
    source_path = Path(source_dir)
    target_path = Path(target_dir)
    target_path.mkdir(exist_ok=True)
    
    moved_files = []
    
    for file_path in source_path.glob("*"):
        if file_path.is_file():
            file_ext = file_path.suffix.lower()
            
            # หาประเภทของไฟล์
            target_category = "others"
            for category, extensions in file_types.items():
                if file_ext in extensions:
                    target_category = category
                    break
            
            # สร้างโฟลเดอร์ปลายทาง
            category_dir = target_path / target_category
            category_dir.mkdir(exist_ok=True)
            
            # ย้ายไฟล์
            target_file = category_dir / file_path.name
            shutil.move(str(file_path), str(target_file))
            moved_files.append(str(target_file))
    
    return moved_files

def backup_files(source_dir, backup_dir, keep_days=30):
    """backup ไฟล์พร้อมการลบไฟล์เก่า"""
    import time
    
    source_path = Path(source_dir)
    backup_path = Path(backup_dir)
    backup_path.mkdir(exist_ok=True)
    
    # สร้างโฟลเดอร์ backup วันที่
    timestamp = time.strftime("%Y%m%d_%H%M%S")
    today_backup = backup_path / timestamp
    today_backup.mkdir(exist_ok=True)
    
    # คัดลอกไฟล์
    copied_files = []
    for file_path in source_path.glob("*"):
        if file_path.is_file():
            dest_path = today_backup / file_path.name
            shutil.copy2(file_path, dest_path)
            copied_files.append(str(dest_path))
    
    # ลบ backup เก่า
    cutoff_time = time.time() - (keep_days * 24 * 60 * 60)
    
    for backup_folder in backup_path.glob("*"):
        if backup_folder.is_dir():
            folder_time = backup_folder.stat().st_mtime
            if folder_time < cutoff_time:
                shutil.rmtree(backup_folder)
    
    return copied_files

# ทดสอบ
if __name__ == "__main__":
    print(get_file_size(__file__))
    print(f"File hash: {calculate_file_hash(__file__)}")
    
    # สร้างไฟล์ทดสอบ
    test_dir = Path("test_files")
    test_dir.mkdir(exist_ok=True)
    
    (test_dir / "test.txt").write_text("Hello World")
    (test_dir / "test_copy.txt").write_text("Hello World")
    
    duplicates = find_duplicate_files(test_dir)
    print(f"Duplicates: {duplicates}")
```

---

## 📊 Data Processing Scripts

### CSV Processor
```python
import csv
import json
from pathlib import Path
from datetime import datetime

def read_csv_with_validation(file_path, required_columns=None):
    """อ่าน CSV พร้อม validation"""
    results = []
    errors = []
    
    try:
        with open(file_path, 'r', encoding='utf-8') as file:
            reader = csv.DictReader(file)
            
            # ตรวจสอบ required columns
            if required_columns:
                missing_cols = set(required_columns) - set(reader.fieldnames)
                if missing_cols:
                    raise ValueError(f"Missing columns: {missing_cols}")
            
            for row_num, row in enumerate(reader, 1):
                try:
                    # ตรวจสอบว่ามีข้อมูลครบถ้วนหรือไม่
                    if any(value.strip() for value in row.values()):
                        results.append(row)
                except Exception as e:
                    errors.append(f"Row {row_num}: {e}")
    
    except Exception as e:
        errors.append(f"File error: {e}")
    
    return {"data": results, "errors": errors}

def csv_to_json(csv_file, json_file, key_column=None):
    """แปลง CSV เป็น JSON"""
    result = read_csv_with_validation(csv_file)
    
    if result["errors"]:
        print(f"Errors found: {result['errors']}")
    
    if key_column and key_column in result["data"][0]:
        # จัดรูปแบบเป็น dictionary ตาม key_column
        json_data = {row[key_column]: row for row in result["data"]}
    else:
        # จัดรูปแบบเป็น list
        json_data = result["data"]
    
    with open(json_file, 'w', encoding='utf-8') as file:
        json.dump(json_data, file, indent=2, ensure_ascii=False)
    
    return json_data

def analyze_csv_data(csv_file):
    """วิเคราะห์ข้อมูล CSV"""
    result = read_csv_with_validation(csv_file)
    data = result["data"]
    
    if not data:
        return {"error": "No data to analyze"}
    
    analysis = {
        "total_rows": len(data),
        "columns": list(data[0].keys()),
        "column_stats": {}
    }
    
    # วิเคราะห์แต่ละ column
    for column in data[0].keys():
        values = [row[column] for row in data if row[column].strip()]
        
        stats = {
            "non_empty_count": len(values),
            "empty_count": len(data) - len(values),
            "unique_values": len(set(values))
        }
        
        # ตรวจสอบว่าเป็นตัวเลขหรือไม่
        numeric_values = []
        for value in values:
            try:
                numeric_values.append(float(value))
            except ValueError:
                pass
        
        if numeric_values:
            stats.update({
                "is_numeric": True,
                "min": min(numeric_values),
                "max": max(numeric_values),
                "avg": sum(numeric_values) / len(numeric_values)
            })
        else:
            stats["is_numeric"] = False
        
        analysis["column_stats"][column] = stats
    
    return analysis

def filter_csv_data(csv_file, filters, output_file):
    """กรองข้อมูล CSV"""
    result = read_csv_with_validation(csv_file)
    data = result["data"]
    
    filtered_data = []
    
    for row in data:
        include_row = True
        
        for column, filter_config in filters.items():
            if column not in row:
                include_row = False
                break
            
            value = row[column]
            
            if filter_config["type"] == "equals":
                if value != filter_config["value"]:
                    include_row = False
                    break
            elif filter_config["type"] == "contains":
                if filter_config["value"] not in value:
                    include_row = False
                    break
            elif filter_config["type"] == "greater_than":
                try:
                    if float(value) <= float(filter_config["value"]):
                        include_row = False
                        break
                except ValueError:
                    include_row = False
                    break
            elif filter_config["type"] == "less_than":
                try:
                    if float(value) >= float(filter_config["value"]):
                        include_row = False
                        break
                except ValueError:
                    include_row = False
                    break
        
        if include_row:
            filtered_data.append(row)
    
    # เขียนผลลัพธ์
    if filtered_data:
        with open(output_file, 'w', newline='', encoding='utf-8') as file:
            writer = csv.DictWriter(file, fieldnames=filtered_data[0].keys())
            writer.writeheader()
            writer.writerows(filtered_data)
    
    return filtered_data

# ทดสอบ
if __name__ == "__main__":
    # สร้างข้อมูลทดสอบ
    test_data = [
        {"name": "สมชาย", "age": "25", "city": "กรุงเทพฯ"},
        {"name": "วิชัย", "age": "30", "city": "เชียงใหม่"},
        {"name": "มานี", "age": "28", "city": "ภูเก็ต"}
    ]
    
    test_file = Path("test_data.csv")
    with open(test_file, 'w', newline='', encoding='utf-8') as file:
        writer = csv.DictWriter(file, fieldnames=["name", "age", "city"])
        writer.writeheader()
        writer.writerows(test_data)
    
    # ทดสอบฟังก์ชัน
    analysis = analyze_csv_data(test_file)
    print(f"Analysis: {json.dumps(analysis, indent=2, ensure_ascii=False)}")
    
    # กรองข้อมูล
    filters = {
        "age": {"type": "greater_than", "value": "25"},
        "city": {"type": "contains", "value": "เชียง"}
    }
    
    filtered = filter_csv_data(test_file, filters, "filtered_data.csv")
    print(f"Filtered {len(filtered)} rows")
```

### Text Analyzer
```python
import re
from collections import Counter
import json

def analyze_text(text):
    """วิเคราะห์ข้อความ"""
    analysis = {
        "character_count": len(text),
        "word_count": len(text.split()),
        "sentence_count": len(re.findall(r'[.!?]+', text)),
        "paragraph_count": len(text.split('\n\n')),
        "avg_word_length": 0,
        "most_common_words": [],
        "language": "unknown"
    }
    
    # คำนวณความยาวคำเฉลี่ย
    words = text.split()
    if words:
        total_length = sum(len(word.strip('.,!?;:')) for word in words)
        analysis["avg_word_length"] = total_length / len(words)
    
    # หาคำที่พบบ่อย
    word_counter = Counter(word.lower().strip('.,!?;:') for word in words)
    analysis["most_common_words"] = word_counter.most_common(10)
    
    # ตรวจสอบภาษา (simple detection)
    thai_chars = len(re.findall(r'[\u0E00-\u0E7F]', text))
    english_chars = len(re.findall(r'[a-zA-Z]', text))
    
    if thai_chars > english_chars:
        analysis["language"] = "thai"
    elif english_chars > 0:
        analysis["language"] = "english"
    
    return analysis

def extract_keywords(text, min_length=3, top_n=10):
    """ดึง keywords จากข้อความ"""
    # ทำความสะอาดข้อความ
    text = re.sub(r'[^\w\s]', ' ', text.lower())
    
    # แยกคำ
    words = text.split()
    
    # กรองคำสั้นๆ
    words = [word for word in words if len(word) >= min_length]
    
    # นับความถี่คำ
    word_counter = Counter(words)
    
    return word_counter.most_common(top_n)

def sentiment_analysis_simple(text):
    """วิเคราะห์ความรู้สึก (simple version)"""
    # Positive words (Thai + English)
    positive_words = [
        'ดี', 'สวย', 'ยอดเยี่ยม', 'เยี่ยม', 'ชอบ', 'รัก', 'สุขใจ', 'มีความสุข',
        'good', 'great', 'excellent', 'love', 'happy', 'wonderful', 'amazing'
    ]
    
    # Negative words
    negative_words = [
        'แย่', 'ไม่ดี', 'เสียใจ', 'เสีย', 'ชั่ว', 'เกลียด', 'โกรธ', 'เศร้า',
        'bad', 'terrible', 'awful', 'hate', 'sad', 'angry', 'horrible'
    ]
    
    text_lower = text.lower()
    
    positive_count = sum(1 for word in positive_words if word in text_lower)
    negative_count = sum(1 for word in negative_words if word in text_lower)
    
    if positive_count > negative_count:
        return "positive"
    elif negative_count > positive_count:
        return "negative"
    else:
        return "neutral"

def generate_summary(text, max_sentences=3):
    """สร้างสรุปคำ (simple extractive summarization)"""
    sentences = re.split(r'[.!?]+', text)
    sentences = [s.strip() for s in sentences if s.strip()]
    
    if not sentences:
        return ""
    
    # คำนวณคะแนนความสำคัญของแต่ละประโยค (simple approach)
    sentence_scores = []
    
    for sentence in sentences:
        score = 0
        
        # คำนวณความยาวประโยค
        score += len(sentence.split())
        
        # ตรวจสอบคำสำคัญ
        keywords = extract_keywords(sentence, min_length=4, top_n=5)
        score += len(keywords) * 2
        
        sentence_scores.append((sentence, score))
    
    # เรียงลำดับตามคะแนน
    sentence_scores.sort(key=lambda x: x[1], reverse=True)
    
    # เลือกประโยคที่ดีที่สุด
    summary_sentences = [s[0] for s in sentence_scores[:max_sentences]]
    
    return '. '.join(summary_sentences) + '.'

# ทดสอบ
if __name__ == "__main__":
    sample_text = """
    Python เป็นภาษาโปรแกรมที่ยอดเยี่ยมมาก! ฉันชอบใช้ Python เพราะมันมีความสุขใจเมื่อเขียนโค้ดด้วย Python.
    ภาษานี้เรียนรู้ง่ายและมี community ที่ดีมาก. แต่บางคนอาจจะรู้สึกว่ามันยากไปหน่อย.
    โดยรวมแล้ว Python เป็นภาษาที่น่าจะเรียนรู้มากที่สุดในปัจจุบัน.
    """
    
    analysis = analyze_text(sample_text)
    print(f"Text Analysis: {json.dumps(analysis, indent=2, ensure_ascii=False)}")
    
    keywords = extract_keywords(sample_text)
    print(f"Keywords: {keywords}")
    
    sentiment = sentiment_analysis_simple(sample_text)
    print(f"Sentiment: {sentiment}")
    
    summary = generate_summary(sample_text)
    print(f"Summary: {summary}")
```

---

## 🎨 Web Scraping Scripts

### Simple Web Scraper
```python
import requests
from bs4 import BeautifulSoup
import json
import time
from urllib.parse import urljoin, urlparse

def scrape_page(url, headers=None):
    """ดึงข้อมูลจากเว็บเพจ"""
    if headers is None:
        headers = {
            'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36'
        }
    
    try:
        response = requests.get(url, headers=headers, timeout=10)
        response.raise_for_status()
        
        soup = BeautifulSoup(response.content, 'html.parser')
        
        return {
            "url": url,
            "title": soup.title.string if soup.title else "",
            "content": soup.get_text(strip=True),
            "links": [a.get('href') for a in soup.find_all('a', href=True)],
            "images": [img.get('src') for img in soup.find_all('img', src=True)]
        }
    
    except Exception as e:
        return {"error": str(e), "url": url}

def extract_text_content(soup, selector=None):
    """ดึงข้อความจาก elements"""
    if selector:
        elements = soup.select(selector)
    else:
        elements = soup.find_all(['p', 'h1', 'h2', 'h3', 'h4', 'h5', 'h6'])
    
    return [elem.get_text(strip=True) for elem in elements]

def scrape_news_headlines(url, news_selector):
    """ดึงข่าวหัวข่าว"""
    result = scrape_page(url)
    
    if "error" in result:
        return result
    
    try:
        response = requests.get(url, headers={
            'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36'
        })
        response.raise_for_status()
        
        soup = BeautifulSoup(response.content, 'html.parser')
        headlines = soup.select(news_selector)
        
        news_data = []
        for headline in headlines:
            news_data.append({
                "title": headline.get_text(strip=True),
                "link": headline.get('href'),
                "source": urlparse(url).netloc
            })
        
        return {"headlines": news_data, "source": url}
    
    except Exception as e:
        return {"error": str(e), "url": url}

def download_images(url, save_dir="images"):
    """ดาวน์โหลดรูปภาพจากเว็บเพจ"""
    from pathlib import Path
    
    save_path = Path(save_dir)
    save_path.mkdir(exist_ok=True)
    
    result = scrape_page(url)
    
    if "error" in result:
        return result
    
    downloaded_images = []
    
    for img_src in result["images"]:
        if not img_src:
            continue
        
        # แปลง relative URL เป็น absolute
        if img_src.startswith('//'):
            img_url = f"https:{img_src}"
        elif img_src.startswith('/'):
            img_url = urljoin(url, img_src)
        elif not img_src.startswith(('http://', 'https://')):
            img_url = urljoin(url, img_src)
        else:
            img_url = img_src
        
        try:
            # ดาวน์โหลดรูปภาพ
            img_response = requests.get(img_url, timeout=10)
            img_response.raise_for_status()
            
            # สร้างชื่อไฟล์
            img_filename = f"image_{len(downloaded_images) + 1}.jpg"
            img_path = save_path / img_filename
            
            # บันทึกรูปภาพ
            with open(img_path, 'wb') as f:
                f.write(img_response.content)
            
            downloaded_images.append({
                "filename": img_filename,
                "url": img_url,
                "size": len(img_response.content)
            })
            
            # รอสักครู่เพื่อไม่โหลดเร็วเกินไป
            time.sleep(0.5)
            
        except Exception as e:
            print(f"Failed to download {img_url}: {e}")
    
    return {"downloaded_images": downloaded_images, "total": len(downloaded_images)}

# ทดสอบ
if __name__ == "__main__":
    # ทดสอบ scraping (ใช้เว็บทดสอบ)
    test_url = "https://httpbin.org/html"
    
    page_data = scrape_page(test_url)
    print(f"Page title: {page_data.get('title')}")
    print(f"Links found: {len(page_data.get('links', []))}")
    
    # ทดสอบดาวน์โหลดรูปภาพ
    # image_result = download_images(test_url)
    # print(f"Downloaded {image_result['total']} images")
```

---

## 🎯 Automation Scripts

### File Backup Automation
```python
import os
import shutil
import time
import json
from pathlib import Path
from datetime import datetime

class AutomatedBackup:
    """ระบบ backup อัตโนมัติ"""
    
    def __init__(self, config_file="backup_config.json"):
        self.config_file = Path(config_file)
        self.config = self.load_config()
    
    def load_config(self):
        """โหลดค่าตั้งค่า"""
        default_config = {
            "source_directories": [],
            "backup_directory": "backups",
            "exclude_patterns": ["*.tmp", "*.log", "__pycache__"],
            "max_backups": 7,
            "compression": True,
            "schedule": "daily"
        }
        
        if self.config_file.exists():
            try:
                with open(self.config_file, 'r', encoding='utf-8') as f:
                    return {**default_config, **json.load(f)}
            except Exception:
                return default_config
        else:
            self.save_config(default_config)
            return default_config
    
    def save_config(self, config=None):
        """บันทึกค่าตั้งค่า"""
        config_to_save = config or self.config
        
        with open(self.config_file, 'w', encoding='utf-8') as f:
            json.dump(config_to_save, f, indent=2, ensure_ascii=False)
    
    def should_exclude(self, file_path):
        """ตรวจสอบว่าควร exclude ไฟล์หรือไม่"""
        import fnmatch
        
        file_path_str = str(file_path)
        
        for pattern in self.config["exclude_patterns"]:
            if fnmatch.fnmatch(file_path_str, pattern):
                return True
        
        return False
    
    def create_backup(self):
        """สร้าง backup"""
        timestamp = datetime.now().strftime("%Y%m%d_%H%M%S")
        backup_dir = Path(self.config["backup_directory"]) / timestamp
        backup_dir.mkdir(parents=True, exist_ok=True)
        
        backup_info = {
            "timestamp": timestamp,
            "backup_directory": str(backup_dir),
            "source_directories": self.config["source_directories"],
            "files_backed_up": [],
            "total_size": 0,
            "start_time": datetime.now().isoformat(),
            "errors": []
        }
        
        for source_dir in self.config["source_directories"]:
            source_path = Path(source_dir)
            
            if not source_path.exists():
                backup_info["errors"].append(f"Source directory not found: {source_dir}")
                continue
            
            # คัดลอกไฟล์
            for file_path in source_path.rglob("*"):
                if file_path.is_file() and not self.should_exclude(file_path):
                    try:
                        # สร้าง path ใน backup
                        relative_path = file_path.relative_to(source_path)
                        backup_file_path = backup_dir / relative_path
                        backup_file_path.parent.mkdir(parents=True, exist_ok=True)
                        
                        # คัดลอกไฟล์
                        shutil.copy2(file_path, backup_file_path)
                        
                        file_size = file_path.stat().st_size
                        backup_info["files_backed_up"].append({
                            "source": str(file_path),
                            "backup": str(backup_file_path),
                            "size": file_size
                        })
                        backup_info["total_size"] += file_size
                        
                    except Exception as e:
                        backup_info["errors"].append(f"Failed to backup {file_path}: {e}")
        
        backup_info["end_time"] = datetime.now().isoformat()
        
        # บันทึกข้อมูล backup
        info_file = backup_dir / "backup_info.json"
        with open(info_file, 'w', encoding='utf-8') as f:
            json.dump(backup_info, f, indent=2, ensure_ascii=False)
        
        # ลบ backup เก่า
        self.cleanup_old_backups()
        
        return backup_info
    
    def cleanup_old_backups(self):
        """ลบ backup เก่า"""
        backup_root = Path(self.config["backup_directory"])
        
        if not backup_root.exists():
            return
        
        # ดู backup directories ทั้งหมด
        backup_dirs = [d for d in backup_root.iterdir() if d.is_dir()]
        
        # เรียงตามเวลา (ชื่อ = timestamp)
        backup_dirs.sort(reverse=True)
        
        # ลบของเก่า
        if len(backup_dirs) > self.config["max_backups"]:
            for old_backup in backup_dirs[self.config["max_backups"]:]:
                try:
                    shutil.rmtree(old_backup)
                    print(f"Deleted old backup: {old_backup}")
                except Exception as e:
                    print(f"Failed to delete {old_backup}: {e}")
    
    def add_source_directory(self, directory):
        """เพิ่ม source directory"""
        if directory not in self.config["source_directories"]:
            self.config["source_directories"].append(directory)
            self.save_config()
    
    def remove_source_directory(self, directory):
        """ลบ source directory"""
        if directory in self.config["source_directories"]:
            self.config["source_directories"].remove(directory)
            self.save_config()

# ทดสอบ
if __name__ == "__main__":
    backup = AutomatedBackup()
    
    # เพิ่ม source directory (ปรับตามความต้องการ)
    # backup.add_source_directory("documents")
    # backup.add_source_directory("projects")
    
    print(f"Current config: {json.dumps(backup.config, indent=2, ensure_ascii=False)}")
    
    # สร้าง backup
    # backup_info = backup.create_backup()
    # print(f"Backup completed: {backup_info['files_backed_up']} files, {backup_info['total_size']} bytes")
```

---

## 📝 สรุป Code Library Structure

| Category | Scripts | การใช้งาน |
|----------|---------|-----------|
| **Utilities** | String, Date, File | ฟังก์ชันที่ใช้บ่อย |
| **Data Processing** | CSV, Text Analyzer | ประมวลผลข้อมูล |
| **Web Scraping** | Simple Scraper | ดึงข้อมูลจากเว็บ |
| **Automation** | Backup System | ทำงานอัตโนมัติ |

---

## 🎯 การใช้งาน Code Library

### 1. การนำเข้ามาใช้
```python
# นำเข้าฟังก์ชันที่ต้องการ
from code_library import string_utils, date_utils, file_utils

# ใช้งาน
clean_text = string_utils.clean_text("  Hello, World!  ")
thai_date = date_utils.format_thai_date(datetime.now())
file_size = file_utils.get_file_size("document.pdf")
```

### 2. การสร้าง Module
```python
# สร้าง my_utils.py
def helper_function():
    pass

# ใช้งานในไฟลอื่น
from my_utils import helper_function
```

### 3. การปรับปรุง
- เพิ่ม error handling
- เพิ่ม type hints
- เพิ่ม unit tests
- เพิ่ม documentation

---

*สร้างเมื่อ: 4 มีนาคม 2026*  
*ปรับปรุงล่าสุด: 4 มีนาคม 2026*
