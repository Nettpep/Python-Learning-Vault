# Code Review Checklist

## 📋 ข้อมูลพื้นฐาน

**Reviewer:** [REVIEWER_NAME]  
**Author:** [AUTHOR_NAME]  
**Date:** [REVIEW_DATE]  
**Project:** [PROJECT_NAME]  
**Branch:** [BRANCH_NAME]

---

## ✅ คะแนน Code Review

### 1. ความถูกต้อง (Correctness)
- [ ] โค้ดทำงานตามที่คาดหวัง
- [ ] Logic ถูกต้อง
- [ ] จัดการ edge cases ได้
- [ ] ไม่มี bugs ที่เห็นได้ชัดเจน

### 2. ประสิทธิภาพ (Performance)
- [ ] ไม่มีปัญหา performance ที่ชัดเจน
- [ ] ใช้ algorithm ที่เหมาะสม
- [ ] ไม่มี memory leaks
- [ ] การเข้าถึง database/network มีประสิทธิภาพ

### 3. ความปลอดภัย (Security)
- [ ] ไม่มีช่องโหว่ดความปลอดภัย
- [ ] ตรวจสอบ input จากผู้ใช้
- [ ] ไม่มี SQL injection หรือ XSS
- [ ] การจัดการ sensitive data ปลอดภัย

### 4. ความอ่านง่าย (Readability)
- [ ] ตัวแปรและฟังก์ชันมีชื่อที่ชัดเจน
- [ ] มีคำอธิบายที่เพียงพอดี
- [ ] โครงสร้างชัดเจนและง่ายต่อม
- [ ] ไม่ซับซ้อนซับซ้อนเกินไป

### 5. การบำรุ่งรักษา (Maintainability)
- [ ] โค้ดง่ายแก้ไขและพัฒนา
- [ ] มีการแบ่งโมดูลที่เหมาะสม
- [ ] ไม่มี code duplication
- [ ] มี tests ครบถ้วน

---

## 🔍 รายละเอียดตรวจสอบ

### Python Specific
- [ ] ใช้ Pythonic code (list comprehension, generators, etc.)
- [ ] ใช้ type hints เมื่อเหมาะสม
- [ ] จัดการ exceptions อย่างเหมาะสม
- [ ] ใช้ context managers (with statements)
- [ ] ไม่ใช้ global variables โดยไม่จำเป็น

### Code Structure
- [ ] ฟังก์ชัน/คลาสมีขนาดที่เหมาะสม
- [ ] การแยกความรับผิดชอบดี (Single Responsibility)
- [ ] ไม่มี deep nesting
- [ ] ใช้ design patterns ที่เหมาะสม

### Documentation
- [ ] Docstrings สำหรับฟังก์ชัน/คลาสสำคัญ
- [ ] Comments อธิบายสิ่งที่ซับซ้อน
- [ ] README/Documentation อัปเดต
- [ ] API documentation ถ้าเป็น library

### Testing
- [ ] Unit tests ครบถ้วน
- [ ] Integration tests สำหรับส่วนสำคัญ
- [ ] Edge cases ได้รับการทดสอบ
- [ ] Tests ผ่านทุกกรณี

---

## 🚨 ปัญหาที่พบ

### Critical Issues (ต้องแก้ไขก่อ merge)
- [ ] **[ISSUE_1]** - [คำอธิบายและวิธีแก้ไข]

### Major Issues (ควรพิจารณาแก้ไข)
- [ ] **[ISSUE_2]** - [คำอธิบายและวิธีแก้ไข]

### Minor Issues (แนะนำให้แก้ไข)
- [ ] **[ISSUE_3]** - [คำอธิบายและวิธีแก้ไข]

### Suggestions (ปรับปรุงแต่ไม่จำเป็น)
- [ ] **[SUGGESTION_1]** - [คำอธิบาย]
- [ ] **[SUGGESTION_2]** - [คำอธิบาย]

---

## 💡 ข้อเสนอแนะ

### ข้อเสนอแนะดีๆ
1. [SUGGESTION_1]
2. [SUGGESTION_2]
3. [SUGGESTION_3]

### แนวทางการพัฒนาต่อไป
1. [IMPROVEMENT_1]
2. [IMPROVEMENT_2]

---

## 📊 สรุปเรียง

### คะแนนรวม: [OVERALL_SCORE]/10

| หมวดหมู่ | คะแนน | ความคิดเห็น |
|-----------|-------|-------------|
| ความถูกต้อง | [CORRECTNESS_SCORE]/2 | |
| ประสิทธิภาพ | [PERFORMANCE_SCORE]/2 | |
| ความปลอดภัย | [SECURITY_SCORE]/1 | |
| ความอ่านง่าย | [READABILITY_SCORE]/2 | |
| การบำรุ่งรักษา | [MAINTAINABILITY_SCORE]/2 | |
| Documentation | [DOCS_SCORE]/1 | |

---

## 🎯 การตัดสินใจ

### ✅ Approved
- [ ] ผ่านการ review ทุกประเภท
- [ ] ไม่มี critical issues
- [ ] Major issues ได้รับการแก้ไขแล้ว
- [ ] Code quality อยู่ในมาตรฐานที่ยอมรับ

### ⚠️ Approved with Changes
- [ ] ต้องแก้ไข minor issues ก่อน merge
- [ ] ต้องเพิ่ม tests หรือ documentation
- [ ] ต้องปรับปรุง performance หรือ security

### ❌ Needs Revision
- [ ] มี critical issues ที่ต้องแก้ไข
- [ ] โค้ดไม่ผ่านมาตรฐานคุณภาพ
- [ ] ต้อง refactor ส่วนใหญ่
- [ ] ขาด tests หรือ documentation

---

## 📝 ความเห็นของ Reviewer

### จุดที่โดดเด่น
- [HIGHLIGHT_1]
- [HIGHLIGHT_2]
- [HIGHLIGHT_3]

### ข้อสังเกต
- [CONCERN_1]
- [CONCERN_2]

---

## 🔄 การติดตาม

### Actions Required
- [ ] [ACTION_1] - [ผู้้รับผิดชอบ]
- [ ] [ACTION_2] - [ผู้้รับผิดชอบ]
- [ ] [ACTION_3] - [ผู้้รับผิดชอบ]

### Due Date
[DATE_FOR_CHANGES]

---

## 📎 ลิงก์ที่เกี่ยวข้อง

- [Pull Request]: [PR_LINK]
- [Build Status]: [BUILD_LINK]
- [Test Coverage]: [COVERAGE_LINK]
- [Documentation]: [DOCS_LINK]

---

## 🏷️ Tags

`[TAG_1]` `[TAG_2]` `[TAG_3]`

---

*Review completed: [COMPLETION_DATE]*  
*Next review: [NEXT_REVIEW_DATE]*

---

## 📋 แม่แบบนี้สามารถใช้สำหรับ:

- **Feature Branch Review**: สำหรับ review feature ใหม่
- **Bug Fix Review**: สำหรับ review bug fixes
- **Refactoring Review**: สำหรับ review code refactoring
- **Performance Review**: สำหรับ review performance improvements
- **Security Review**: สำหรับ review security changes

---

## 🔧 เครื่องมือช่วย

- **GitHub**: สำหรับ pull requests และ code reviews
- **SonarQube**: สำหรับ automated code analysis
- **Black**: สำหรับ code formatting
- **Flake8**: สำหรับ linting
- **MyPy**: สำหรับ type checking
- **Coverage.py**: สำหรับ test coverage
