# AI Integration Logs - บันทึกการใช้ AI และ Prompt Engineering

## 🤖 แนวคิดพื้นฐาน

AI Integration คือ **การเชื่อมต่อ Python กับ AI Services** เช่น OpenAI, Google AI, หรือ Local AI Models เพื่อสร้างแอปพลิเคชันที่มีความฉลาดและสามารถทำงานอัตโนมัติได้

```python
# การเชื่อมต่อกับ OpenAI API
import openai

response = openai.chat.completions.create(
    model="gpt-3.5-turbo",
    messages=[{"role": "user", "content": "Hello!"}]
)
```

---

## 📝 OpenAI API Integration

### การตั้งค่าและเชื่อมต่อ
```python
import openai
import os
from dotenv import load_dotenv

# โหลด environment variables
load_dotenv()

# ตั้งค่า API key
openai.api_key = os.getenv('OPENAI_API_KEY')

# หรือตั้งค่าโดยตรง (ไม่แนะนำสำหรับ production)
# openai.api_key = "sk-your-api-key-here"

class OpenAIClient:
    """Client สำหรับ OpenAI API"""
    
    def __init__(self, api_key=None, model="gpt-3.5-turbo"):
        self.api_key = api_key or os.getenv('OPENAI_API_KEY')
        self.model = model
        openai.api_key = self.api_key
    
    def chat_completion(self, messages, temperature=0.7, max_tokens=1000):
        """Chat completion"""
        try:
            response = openai.chat.completions.create(
                model=self.model,
                messages=messages,
                temperature=temperature,
                max_tokens=max_tokens
            )
            return response.choices[0].message.content
        except Exception as e:
            return f"Error: {e}"
    
    def simple_chat(self, prompt, system_message=None):
        """Simple chat"""
        messages = []
        
        if system_message:
            messages.append({"role": "system", "content": system_message})
        
        messages.append({"role": "user", "content": prompt})
        
        return self.chat_completion(messages)

# การใช้งาน
client = OpenAIClient()

response = client.simple_chat(
    "สวัสดี! แนะนำตัวเองหน่อย",
    system_message="คุณเป็นผู้ช่วย AI ที่เป็นมิตรและเป็นประโยชน์"
)
print(response)
```

### การจัดการ Conversations
```python
class ConversationManager:
    """จัดการการสนทนากับ AI"""
    
    def __init__(self, system_message=None):
        self.client = OpenAIClient()
        self.messages = []
        
        if system_message:
            self.messages.append({
                "role": "system", 
                "content": system_message
            })
    
    def add_message(self, role, content):
        """เพิ่มข้อความใน conversation"""
        self.messages.append({"role": role, "content": content})
    
    def chat(self, user_input, temperature=0.7):
        """สนทนากับ AI"""
        self.add_message("user", user_input)
        
        response = self.client.chat_completion(
            self.messages, 
            temperature=temperature
        )
        
        self.add_message("assistant", response)
        return response
    
    def get_conversation_history(self):
        """ดูประวัติการสนทนา"""
        return self.messages
    
    def clear_conversation(self):
        """ล้างประวัติการสนทนา"""
        # เก็บเฉพาะ system message
        system_messages = [msg for msg in self.messages if msg["role"] == "system"]
        self.messages = system_messages
    
    def export_conversation(self, filename="conversation.txt"):
        """ส่งออกการสนทนา"""
        with open(filename, 'w', encoding='utf-8') as file:
            for msg in self.messages:
                role = msg["role"].upper()
                content = msg["content"]
                file.write(f"{role}: {content}\n\n")

# การใช้งาน
conv = ConversationManager(
    system_message="คุณเป็นผู้เชี่ยวชาญด้านการเขียนโปรแกรม Python"
)

print(conv.chat("อธิบาย List Comprehension ใน Python"))
print(conv.chat("ให้ตัวอย่างการใช้งาน"))
print(conv.chat("ข้อดีและข้อเสียคืออะไร?"))

conv.export_conversation("python_conversation.txt")
```

---

## 🎪 Prompt Engineering

### การเขียน Prompts ที่มีประสิทธิภาพ
```python
class PromptTemplates:
    """Templates สำหรับการเขียน prompts"""
    
    @staticmethod
    def code_review_prompt(code, language="Python"):
        """Prompt สำหรับ review code"""
        return f"""
        กรุณา review โค้ด {language} ต่อไปนี้:
        
        ```{language}
        {code}
        ```
        
        กรุณาวิเคราะห์และแนะนำในส่วนต่อไปนี้:
        1. คุณภาพโค้ด (Code Quality)
        2. ประสิทธิภาพ (Performance)
        3. ความปลอดภัย (Security)
        4. การอ่านง่าย (Readability)
        5. ข้อแนะนำในการปรับปรุง
        
        ตอบเป็นภาษาไทย และจัดรูปแบบให้ชัดเจน
        """
    
    @staticmethod
    def explanation_prompt(topic, level="beginner"):
        """Prompt สำหรับอธิบาย concept"""
        return f"""
        กรุณาอธิบายเรื่อง "{topic}" สำหรับผู้เริ่มต้นระดับ {level}
        
        กรุณาจัดรูปแบบคำตอบดังนี้:
        1. คำอธิบายแบบง่ายๆ (Easy explanation)
        2. ตัวอย่างโค้ด (Code examples)
        3. กรณีการใช้งาน (Use cases)
        4. ข้อควรระวัง (Important notes)
        5. แหล่งเรียนรู้เพิ่มเติม (Additional resources)
        
        ตอบเป็นภาษาไทยที่เข้าใจง่าย
        """
    
    @staticmethod
    def debugging_prompt(code, error_message):
        """Prompt สำหรับ debugging"""
        return f"""
        ฉันมีปัญหากับโค้ด Python ต่อไปนี้:
        
        ```python
        {code}
        ```
        
        และได้รับ error ว่า:
        "{error_message}"
        
        กรุณา:
        1. อธิบายสาเหตุของ error
        2. แนะนำวิธีแก้ไข
        3. ให้โค้ดที่แก้ไขแล้ว
        4. อธิบายว่าทำไมการแก้ไขนี้ถึงได้ผล
        
        ตอบเป็นภาษาไทยและมีประโยชน์
        """
    
    @staticmethod
    def translation_prompt(text, target_language, context=""):
        """Prompt สำหรับการแปลภาษา"""
        return f"""
        กรุณาแปลข้อความต่อไปนี้เป็นภาษา{target_language}:
        
        "{text}"
        
        {f'บริบท: {context}' if context else ''}
        
        กรุณา:
        1. รักษาความหมายเดิม
        2. ใช้ภาษาที่เป็นธรรมชาติ
        3. รักษาโทนและสไตล์ของข้อความ
        4. ถ้ามีคำศัพท์เฉพาะทาง ให้คงไว้และอธิบายในวงเล็บ
        """

# การใช้งาน
client = OpenAIClient()

# Review code
code = """
def calculate_sum(numbers):
    total = 0
    for num in numbers:
        total += num
    return total
"""

prompt = PromptTemplates.code_review_prompt(code)
review = client.chat_completion([{"role": "user", "content": prompt}])
print(review)
```

### Advanced Prompt Techniques
```python
class AdvancedPrompts:
    """เทคนิคการเขียน prompts ขั้นสูง"""
    
    @staticmethod
    def chain_of_thought_prompt(problem):
        """Chain of Thought prompting"""
        return f"""
        แก้โจทย์ต่อไปนี้โดยใช้ความคิดทีละขั้น (step by step):
        
        {problem}
        
        กรุณา:
        1. วิเคราะห์ปัญหา
        2. วางแผนการแก้ไข
        3. ดำเนินการทีละขั้น
        4. ตรวจสอบคำตอบ
        5. สรุปผลลัพธ์
        
        อธิบายความคิดในแต่ละขั้นตอน
        """
    
    @staticmethod
    def few_shot_prompt(task, examples):
        """Few-shot prompting"""
        example_text = "\n\n".join([
            f"ตัวอย่าง {i+1}:\n{ex['input']}\nคำตอบ: {ex['output']}"
            for i, ex in enumerate(examples)
        ])
        
        return f"""
        ดูตัวอย่างต่อไปนี้แล้วตอบคำถามสุดท้าย:
        
        {example_text}
        
        คำถาม: {task}
        คำตอบ:
        """
    
    @staticmethod
    def role_playing_prompt(scenario, role):
        """Role-playing prompting"""
        return f"""
        จินตนาการว่าคุณเป็น {role}
        
        สถานการณ์: {scenario}
        
        กรุณาตอบในบทบาทของ{role} โดย:
        1. ใช้ภาษาที่เหมาะสมกับบทบาท
        2. แสดงความเชี่ยวชาญในด้านนั้น
        3. ให้คำแนะนำที่เป็นประโยชน์
        4. มีปฏิสัมพันธ์กับสถานการณ์
        """

# การใช้งาน
client = OpenAIClient()

# Chain of Thought
math_problem = "ถ้ามีแอปเปิ้ล 10 ลูก และส้ม 5 ลูก จำนวนผลไม้ทั้งหมดคือเท่าไหร่?"
cot_prompt = AdvancedPrompts.chain_of_thought_prompt(math_problem)
cot_response = client.chat_completion([{"role": "user", "content": cot_prompt}])

# Few-shot
examples = [
    {"input": "2 + 2", "output": "4"},
    {"input": "5 + 3", "output": "8"},
    {"input": "7 + 1", "output": "8"}
]
few_shot_prompt = AdvancedPrompts.few_shot_prompt("4 + 6", examples)
few_shot_response = client.chat_completion([{"role": "user", "content": few_shot_prompt}])
```

---

## 🛠️ การใช้งานจริง

### Code Assistant
```python
class CodeAssistant:
    """ผู้ช่วยด้านการเขียนโค้ด"""
    
    def __init__(self):
        self.client = OpenAIClient()
        self.conversation = ConversationManager(
            system_message="คุณเป็นผู้เชี่ยวชาญด้านการเขียนโปรแกรม Python ที่มีประสบการณ์มากกว่า 10 ปี"
        )
    
    def generate_code(self, description, language="Python"):
        """สร้างโค้ดจากคำอธิบาย"""
        prompt = f"""
        กรุณาเขียนโค้ด {language} ตามคำอธิบายต่อไปนี้:
        
        {description}
        
        กรุณา:
        1. เขียนโค้ดที่สมบูรณ์และทำงานได้
        2. เพิ่มคำอธิบายในโค้ด (comments)
        3. ให้ตัวอย่างการใช้งาน
        4. อธิบายว่าโค้ดทำงานอย่างไร
        """
        
        return self.conversation.chat(prompt)
    
    def debug_code(self, code, error):
        """ช่วย debug โค้ด"""
        prompt = PromptTemplates.debugging_prompt(code, error)
        return self.conversation.chat(prompt)
    
    def optimize_code(self, code):
        """ปรับปรุงโค้ดให้มีประสิทธิภาพดีขึ้น"""
        prompt = f"""
        กรุณาปรับปรุงโค้ด Python ต่อไปนี้ให้มีประสิทธิภาพดีขึ้น:
        
        ```python
        {code}
        ```
        
        กรุณา:
        1. รักษาฟังก์ชันการทำงานเดิม
        2. ปรับปรุงประสิทธิภาพ
        3. ลดความซับซ้อน
        4. เพิ่มความปลอดภัย
        5. อธิบายการเปลี่ยนแปลง
        """
        
        return self.conversation.chat(prompt)
    
    def explain_code(self, code):
        """อธิบายโค้ด"""
        prompt = f"""
        กรุณาอธิบายโค้ด Python ต่อไปนี้:
        
        ```python
        {code}
        ```
        
        อธิบาย:
        1. วัตถุประสงค์ของโค้ด
        2. การทำงานของแต่ละส่วน
        3. ตรรกะการทำงาน
        4. จุดที่น่าสนใจ
        """
        
        return self.conversation.chat(prompt)

# การใช้งาน
assistant = CodeAssistant()

# สร้างฟังก์ชันคำนวณ factorial
description = "สร้างฟังก์ชันคำนวณ factorial แบบ recursive และ iterative"
factorial_code = assistant.generate_code(description)
print(factorial_code)

# อธิบายโค้ด
code_explanation = assistant.explain_code(factorial_code)
print(code_explanation)
```

### Content Generator
```python
class ContentGenerator:
    """สร้างเนื้อหาต่างๆ ด้วย AI"""
    
    def __init__(self):
        self.client = OpenAIClient()
    
    def generate_blog_post(self, topic, tone="informative"):
        """สร้างบทความบล็อก"""
        prompt = f"""
        เขียนบทความบล็อกเกี่ยวกับ "{topic}" 
        โดยใช้โทนเสียง {tone}
        
        โครงสร้างบทความ:
        1. หัวข้อที่น่าดึงดูด
        2. บทนำ (สั้นๆ)
        3. เนื้อหาหลัก (3-4 ย่อหน้า)
        4. บทสรุป
        5. คำถามที่พบบ่อย
        
        ความยาวประมาณ 500-800 คำ
        เขียนเป็นภาษาไทยที่อ่านง่ายเข้าใจ
        """
        
        return self.client.simple_chat(prompt)
    
    def generate_social_media_post(self, topic, platform="facebook"):
        """สร้างโพสต์โซเชียลมีเดีย"""
        platform_configs = {
            "facebook": {"max_length": 500, "hashtags": True},
            "twitter": {"max_length": 280, "hashtags": True},
            "instagram": {"max_length": 300, "hashtags": True, "visual": True},
            "linkedin": {"max_length": 600, "hashtags": False, "professional": True}
        }
        
        config = platform_configs.get(platform, platform_configs["facebook"])
        
        prompt = f"""
        เขียนโพสต์สำหรับ {platform} เกี่ยวกับ "{topic}"
        
        ข้อกำหนด:
        - ความยาวไม่เกิน {config['max_length']} ตัวอักษร
        - โทนเสียง: {'มืออาชีพ' if config.get('professional') else 'เป็นกันเอง'}
        - {'เพิ่ม hashtags ที่เกี่ยวข้อง' if config['hashtags'] else 'ไม่ต้องมี hashtags'}
        - {'เหมาะสำหรับโพสต์ที่มีรูปภาพ' if config.get('visual') else ''}
        
        เขียนเป็นภาษาไทยที่น่าสนใจและดึงดูด
        """
        
        return self.client.simple_chat(prompt)
    
    def generate_email(self, purpose, recipient_type, tone="formal"):
        """สร้างอีเมล"""
        prompt = f"""
        เขียนอีเมล {purpose} สำหรับ {recipient_type}
        โดยใช้โทนเสียง {tone}
        
        โครงสร้างอีเมล:
        1. Subject line (หัวข้อ)
        2. Greeting (คำทักทาย)
        3. Body (เนื้อหา)
        4. Closing (คำลงาม)
        5. Signature (ลายเซ็น)
        
        เขียนเป็นภาษาไทยที่เป็นทางการและสุภาพ
        """
        
        return self.client.simple_chat(prompt)

# การใช้งาน
generator = ContentGenerator()

# สร้างบทความ
blog_post = generator.generate_blog_post("การเรียนรู้ Python สำหรับผู้เริ่มต้น")
print(blog_post)

# สร้างโพสต์ Facebook
fb_post = generator.generate_social_media_post("เคล็ดลับการเขียนโค้ด Python", "facebook")
print(fb_post)
```

### Learning Assistant
```python
class LearningAssistant:
    """ผู้ช่วยการเรียนรู้"""
    
    def __init__(self):
        self.client = OpenAIClient()
        self.conversation = ConversationManager(
            system_message="คุณเป็นครูสอนพิเศษด้านการเขียนโปรแกรมที่มีความอดทนและสอนได้ดี"
        )
    
    def explain_concept(self, concept, level="beginner"):
        """อธิบาย concept"""
        prompt = PromptTemplates.explanation_prompt(concept, level)
        return self.conversation.chat(prompt)
    
    def create_exercise(self, topic, difficulty="easy"):
        """สร้างแบบฝึกหัด"""
        difficulty_levels = {
            "easy": "ง่าย (สำหรับผู้เริ่มต้น)",
            "medium": "ปานกลาง (สำหรับผู้มีพื้นฐาน)",
            "hard": "ยาก (สำหรับผู้มีประสบการณ์)"
        }
        
        prompt = f"""
        สร้างแบบฝึกหัดปัญหาการเขียนโปรแกรม Python เกี่ยวกับ "{topic}"
        ระดับความยาก: {difficulty_levels.get(difficulty, difficulty)}
        
        กรุณาสร้าง:
        1. โจทย์ปัญหา (ชัดเจนและเข้าใจง่าย)
        2. ตัวอย่าง input/output
        3. เคล็ดลับในการแก้ไข
        4. คำตอบพร้อมคำอธิบาย
        5. จุดประสงค์การเรียนรู้จากแบบฝึกหัด
        
        จัดรูปแบบให้เหมาะสำหรับการเรียนรู้
        """
        
        return self.conversation.chat(prompt)
    
    def review_answer(self, question, student_answer, expected_answer):
        """ตรวจสอบคำตอบของนักเรียน"""
        prompt = f"""
        โจทย์: {question}
        
        คำตอบของนักเรียน:
        {student_answer}
        
        คำตอบที่คาดหวัง:
        {expected_answer}
        
        กรุณาประเมินคำตอบของนักเรียน:
        1. ถูกต้องหรือไม่
        2. ส่วนไหนถูก/ผิด
        3. ข้อดีของคำตอบ
        4. ข้อเสนอแนะในการปรับปรุง
        5. คะแนน (ถ้าจะให้คะแนน)
        
        ให้ข้อเสนอแนะอย่างสร้างสรรค์และเป็นประโยชน์
        """
        
        return self.conversation.chat(prompt)
    
    def create_study_plan(self, topic, duration_weeks, current_level="beginner"):
        """สร้างแผนการเรียน"""
        prompt = f"""
        สร้างแผนการเรียน Python เกี่ยวกับ "{topic}"
        ระยะเวลา: {duration_weeks} สัปดาห์
        ระดับปัจจุบัน: {current_level}
        
        แผนการเรียนควรมี:
        1. วัตถุประสงค์การเรียน
        2. แบ่งเป็นสัปดาห์ๆ (สัปดาห์ละกี่หัวข้อ)
        3. หัวข้อที่ต้องเรียนแต่ละสัปดาห์
        4. กิจกรรม/แบบฝึกหัดแต่ละสัปดาห์
        5. โปรเจคต์สุดท้าย
        6. แหล่งเรียนรู้เพิ่มเติม
        7. วิธีการวัดผล
        
        ทำให้แผนการเรียนเป็นไปอย่างเป็นระบบและทำตามได้จริง
        """
        
        return self.conversation.chat(prompt)

# การใช้งาน
tutor = LearningAssistant()

# อธิบาย concept
explanation = tutor.explain_concept("Decorators in Python", "beginner")
print(explanation)

# สร้างแบบฝึกหัด
exercise = tutor.create_exercise("List Comprehension", "medium")
print(exercise)

# สร้างแผนการเรียน
study_plan = tutor.create_study_plan("Python Fundamentals", 8, "beginner")
print(study_plan)
```

---

## 🎯 การจัดการ Costs และ Optimization

### Token Counting
```python
import tiktoken

class TokenManager:
    """จัดการ tokens สำหรับควบคุม costs"""
    
    def __init__(self, model="gpt-3.5-turbo"):
        self.model = model
        self.encoding = tiktoken.encoding_for_model(model)
    
    def count_tokens(self, text):
        """นับจำนวน tokens"""
        return len(self.encoding.encode(text))
    
    def estimate_cost(self, messages, model="gpt-3.5-turbo"):
        """ประมาณการค่าใช้จ่าย"""
        # Prices per 1K tokens (as of 2024)
        prices = {
            "gpt-3.5-turbo": {"input": 0.0015, "output": 0.002},
            "gpt-4": {"input": 0.03, "output": 0.06},
            "gpt-4-turbo": {"input": 0.01, "output": 0.03}
        }
        
        model_prices = prices.get(model, prices["gpt-3.5-turbo"])
        
        input_tokens = 0
        output_tokens = 1000  # ประมาณการ
        
        for message in messages:
            input_tokens += self.count_tokens(message["content"])
        
        input_cost = (input_tokens / 1000) * model_prices["input"]
        output_cost = (output_tokens / 1000) * model_prices["output"]
        
        return {
            "input_tokens": input_tokens,
            "estimated_output_tokens": output_tokens,
            "input_cost": input_cost,
            "estimated_output_cost": output_cost,
            "total_estimated_cost": input_cost + output_cost
        }
    
    def truncate_text(self, text, max_tokens):
        """ตัดข้อความให้ไม่เกิน tokens"""
        tokens = self.encoding.encode(text)
        if len(tokens) <= max_tokens:
            return text
        
        truncated_tokens = tokens[:max_tokens]
        return self.encoding.decode(truncated_tokens)

# การใช้งาน
token_manager = TokenManager()

messages = [
    {"role": "system", "content": "You are a helpful assistant."},
    {"role": "user", "content": "Explain quantum computing in simple terms."}
]

cost_info = token_manager.estimate_cost(messages)
print(f"Estimated cost: ${cost_info['total_estimated_cost']:.4f}")
```

### Response Caching
```python
import hashlib
import json
import time
from pathlib import Path

class AIResponseCache:
    """Cache สำหรับ AI responses"""
    
    def __init__(self, cache_dir="ai_cache", ttl=3600):
        self.cache_dir = Path(cache_dir)
        self.cache_dir.mkdir(exist_ok=True)
        self.ttl = ttl
    
    def _get_cache_key(self, prompt, model="gpt-3.5-turbo"):
        """สร้าง cache key"""
        key_data = f"{model}:{prompt}"
        return hashlib.md5(key_data.encode()).hexdigest()
    
    def get(self, prompt, model="gpt-3.5-turbo"):
        """ดึง response จาก cache"""
        cache_key = self._get_cache_key(prompt, model)
        cache_file = self.cache_dir / f"{cache_key}.json"
        
        if not cache_file.exists():
            return None
        
        try:
            with open(cache_file, 'r', encoding='utf-8') as f:
                cache_data = json.load(f)
            
            # ตรวจสอบว่าหมดอายุหรือไม่
            if time.time() - cache_data['timestamp'] > self.ttl:
                cache_file.unlink()
                return None
            
            return cache_data['response']
            
        except Exception:
            return None
    
    def set(self, prompt, response, model="gpt-3.5-turbo"):
        """บันทึก response ลง cache"""
        cache_key = self._get_cache_key(prompt, model)
        cache_file = self.cache_dir / f"{cache_key}.json"
        
        cache_data = {
            'timestamp': time.time(),
            'prompt': prompt,
            'model': model,
            'response': response
        }
        
        try:
            with open(cache_file, 'w', encoding='utf-8') as f:
                json.dump(cache_data, f, indent=2)
        except Exception as e:
            print(f"Cache write error: {e}")

class CostOptimizedAI:
    """AI Client ที่เพิ่มประสิทธิภาพ costs"""
    
    def __init__(self, api_key, cache_ttl=3600):
        self.client = OpenAIClient(api_key)
        self.cache = AIResponseCache(ttl=cache_ttl)
        self.token_manager = TokenManager()
    
    def chat_with_cache(self, prompt, model="gpt-3.5-turbo"):
        """Chat พร้อม cache"""
        # พยายามดูจาก cache ก่อน
        cached_response = self.cache.get(prompt, model)
        if cached_response:
            print("📦 From cache")
            return cached_response
        
        # ตรวจสอบ tokens และตัดถ้าจำเป็น
        max_tokens = 4000  # สำหรับ gpt-3.5-turbo
        if self.token_manager.count_tokens(prompt) > max_tokens:
            prompt = self.token_manager.truncate_text(prompt, max_tokens)
            print("✂️ Prompt truncated")
        
        # Request จาก API
        print("🌐 From API")
        response = self.client.simple_chat(prompt)
        
        # บันทึกลง cache
        self.cache.set(prompt, response, model)
        
        return response

# การใช้งาน
optimized_ai = CostOptimizedAI("your-api-key")

# Request ครั้งแรก (จาก API)
response1 = optimized_ai.chat_with_cache("What is Python?")

# Request ครั้งที่สอง (จาก cache)
response2 = optimized_ai.chat_with_cache("What is Python?")
```

---

## 💡 Best Practices

### 1. การเขียน Prompts ที่ดี
```python
# ✅ ดี: ชัดเจน มีโครงสร้าง
prompt = """
แปลข้อความต่อไปนี้เป็นภาษาอังกฤษ:
"สวัสดี ยินดีต้อนรับสู่ประเทศไทย"

ความต้องการ:
- รักษาความเป็นทางการ
- ใช้ภาษาที่เป็นธรรมชาติ
- แปลความหมายให้ถูกต้อง
"""

# ❌ ไม่ดี: ไม่ชัดเจน
prompt = "แปลสวัสดี ยินดีต้อนรับสู่ประเทศไทย เป็นอังกฤษ"
```

### 2. การจัดการ API Keys
```python
# ✅ ดี: ใช้ environment variables
import os
from dotenv import load_dotenv

load_dotenv()
api_key = os.getenv('OPENAI_API_KEY')

# ❌ ไม่ดี: hardcode ในโค้ด
api_key = "sk-1234567890abcdef"
```

### 3. การควบคุม Costs
```python
# ✅ ดี: ตรวจสอบ tokens ก่อน
if token_manager.count_tokens(prompt) > max_tokens:
    prompt = token_manager.truncate_text(prompt, max_tokens)

# ❌ ไม่ดี: ไม่ตรวจสอบ
response = client.chat_completion(messages)  # อาจแพง
```

### 4. การจัดการ Errors
```python
# ✅ ดี: จัดการ errors ตามประเภท
try:
    response = client.chat_completion(messages)
except openai.RateLimitError:
    print("Rate limit exceeded")
except openai.AuthenticationError:
    print("Invalid API key")
except openai.APIError as e:
    print(f"API error: {e}")

# ❌ ไม่ดี: จัดการรวมๆ
try:
    response = client.chat_completion(messages)
except Exception as e:
    print(f"Error: {e}")
```

---

## 📝 สรุป AI Integration

| Component | การใช้งาน | ตัวอย่าง |
|-----------|-----------|-----------|
| **OpenAI Client** | เชื่อมต่อกับ OpenAI API | `OpenAIClient()` |
| **Conversation Manager** | จัดการการสนทนา | `ConversationManager()` |
| **Prompt Templates** | Templates สำหรับ prompts | `PromptTemplates` |
| **Token Manager** | จัดการ tokens และ costs | `TokenManager()` |
| **Response Cache** | Cache responses | `AIResponseCache()` |

---

## 🎯 แบบฝึกหัด

### แบบฝึกหัด 1: Code Review Bot
```python
# เขียน bot สำหรับ review code อัตโนมัติ
class CodeReviewBot:
    pass
```

### แบบฝึกหัด 2: Learning Assistant
```python
# เขียนผู้ช่วยสอน Python แบบ interactive
class PythonTutor:
    pass
```

### แบบฝึกหัด 3: Content Generator
```python
# เขียนเครื่องมือสร้างเนื้อหาอัตโนมัติ
class ContentBot:
    pass
```

---

*สร้างเมื่อ: 4 มีนาคม 2026*  
*ปรับปรุงล่าสุด: 4 มีนาคม 2026*
