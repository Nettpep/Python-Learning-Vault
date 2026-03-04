# API Connections - การเชื่อมต่อกับระบบอื่น

## 🌐 แนวคิดพื้นฐาน

API (Application Programming Interface) คือ **ช่องทางการสื่อสาร** ระหว่างโปรแกรมต่างๆ ทำให้ Python สามารถเชื่อมต่อกับเว็บไซต์ บริการออนไลน์ และระบบภายนอกได้

```python
# การเชื่อมต่อ API พื้นฐาน
import requests

response = requests.get('https://api.example.com/data')
data = response.json()
```

---

## 📝 HTTP Methods พื้นฐาน

### HTTP Methods หลักๆ
```python
import requests
import json

# GET - ดึงข้อมูล
def get_data(url, params=None):
    """GET request"""
    try:
        response = requests.get(url, params=params)
        response.raise_for_status()  # ตรวจสอบ HTTP error
        return response.json()
    except requests.exceptions.RequestException as e:
        print(f"GET Error: {e}")
        return None

# POST - สร้างข้อมูล
def post_data(url, data):
    """POST request"""
    try:
        headers = {'Content-Type': 'application/json'}
        response = requests.post(url, json=data, headers=headers)
        response.raise_for_status()
        return response.json()
    except requests.exceptions.RequestException as e:
        print(f"POST Error: {e}")
        return None

# PUT - อัปเดตข้อมูล
def put_data(url, data):
    """PUT request"""
    try:
        headers = {'Content-Type': 'application/json'}
        response = requests.put(url, json=data, headers=headers)
        response.raise_for_status()
        return response.json()
    except requests.exceptions.RequestException as e:
        print(f"PUT Error: {e}")
        return None

# DELETE - ลบข้อมูล
def delete_data(url):
    """DELETE request"""
    try:
        response = requests.delete(url)
        response.raise_for_status()
        return response.status_code == 200
    except requests.exceptions.RequestException as e:
        print(f"DELETE Error: {e}")
        return False

# การใช้งาน
base_url = "https://jsonplaceholder.typicode.com"

# GET ข้อมูล
users = get_data(f"{base_url}/users")
print(f"Found {len(users)} users")

# POST สร้างข้อมูล
new_post = {
    "title": "My Post",
    "body": "This is my post content",
    "userId": 1
}
created_post = post_data(f"{base_url}/posts", new_post)
print(f"Created post with ID: {created_post.get('id')}")
```

---

## 🎪 การจัดการ Headers และ Authentication

### Headers และ Parameters
```python
import requests

# การตั้งค่า Headers
def make_request_with_headers():
    """Request พร้อม headers"""
    url = "https://httpbin.org/get"
    
    headers = {
        'User-Agent': 'My Python App/1.0',
        'Accept': 'application/json',
        'Accept-Language': 'th-TH,th;q=0.9,en;q=0.8',
        'Custom-Header': 'CustomValue'
    }
    
    # Query parameters
    params = {
        'param1': 'value1',
        'param2': 'value2',
        'search': 'python'
    }
    
    response = requests.get(url, headers=headers, params=params)
    return response.json()

# การใช้ Cookies
def request_with_cookies():
    """Request พร้อม cookies"""
    url = "https://httpbin.org/cookies"
    
    cookies = {
        'session_id': 'abc123',
        'user_preference': 'dark_mode',
        'language': 'th'
    }
    
    response = requests.get(url, cookies=cookies)
    return response.json()

# การจัดการ Sessions
def session_example():
    """ใช้ session เพื่อรักษา cookies และ headers"""
    session = requests.Session()
    
    # ตั้งค่า headers สำหรับทุก request
    session.headers.update({
        'User-Agent': 'My Python App/1.0',
        'Accept': 'application/json'
    })
    
    # Login request
    login_data = {'username': 'user', 'password': 'pass'}
    response = session.post('https://example.com/login', json=login_data)
    
    if response.status_code == 200:
        # Session จะรักษา cookies อัตโนมัติ
        response = session.get('https://example.com/profile')
        return response.json()
    
    return None
```

### Authentication Methods
```python
import requests
from requests.auth import HTTPBasicAuth, HTTPDigestAuth
import base64

# Basic Authentication
def basic_auth_example():
    """Basic Authentication"""
    url = "https://httpbin.org/basic-auth/user/pass"
    
    # วิธีที่ 1: ใช้ HTTPBasicAuth
    auth = HTTPBasicAuth('user', 'pass')
    response = requests.get(url, auth=auth)
    
    # วิธีที่ 2: ใช้ tuple
    response = requests.get(url, auth=('user', 'pass'))
    
    return response.json()

# Digest Authentication
def digest_auth_example():
    """Digest Authentication"""
    url = "https://httpbin.org/digest-auth/auth/user/pass"
    
    auth = HTTPDigestAuth('user', 'pass')
    response = requests.get(url, auth=auth)
    
    return response.json()

# Bearer Token (JWT)
def bearer_token_example():
    """Bearer Token Authentication"""
    url = "https://api.example.com/protected"
    
    token = "your_jwt_token_here"
    headers = {
        'Authorization': f'Bearer {token}',
        'Content-Type': 'application/json'
    }
    
    response = requests.get(url, headers=headers)
    return response.json()

# API Key Authentication
def api_key_example():
    """API Key Authentication"""
    url = "https://api.example.com/data"
    
    # วิธีที่ 1: Header
    headers = {
        'X-API-Key': 'your_api_key_here',
        'Content-Type': 'application/json'
    }
    response = requests.get(url, headers=headers)
    
    # วิธีที่ 2: Query parameter
    params = {
        'api_key': 'your_api_key_here',
        'format': 'json'
    }
    response = requests.get(url, params=params)
    
    return response.json()

# Custom Authentication
def custom_auth_example():
    """Custom Authentication"""
    url = "https://api.example.com/data"
    
    # สร้าง custom header
    api_key = "your_api_key"
    api_secret = "your_api_secret"
    
    # สร้าง signature (ตัวอย่าง)
    import hashlib
    import time
    
    timestamp = str(int(time.time()))
    message = f"{timestamp}{api_key}"
    signature = hashlib.sha256(message.encode()).hexdigest()
    
    headers = {
        'X-API-Key': api_key,
        'X-Timestamp': timestamp,
        'X-Signature': signature
    }
    
    response = requests.get(url, headers=headers)
    return response.json()
```

---

## 🛠️ การจัดการ Response

### Response Handling
```python
import requests
from requests.exceptions import RequestException

def handle_response(response):
    """จัดการ response อย่างสมบูรณ์"""
    print(f"Status Code: {response.status_code}")
    print(f"Headers: {dict(response.headers)}")
    print(f"URL: {response.url}")
    print(f"Encoding: {response.encoding}")
    
    # ตรวจสอบ status code
    if response.status_code == 200:
        print("✅ Success")
    elif response.status_code == 201:
        print("✅ Created")
    elif response.status_code == 204:
        print("✅ No Content")
    elif response.status_code == 400:
        print("❌ Bad Request")
    elif response.status_code == 401:
        print("❌ Unauthorized")
    elif response.status_code == 403:
        print("❌ Forbidden")
    elif response.status_code == 404:
        print("❌ Not Found")
    elif response.status_code == 500:
        print("❌ Server Error")
    else:
        print(f"⚠️ Unknown status: {response.status_code}")
    
    # ดู content type
    content_type = response.headers.get('content-type', '')
    
    if 'application/json' in content_type:
        try:
            data = response.json()
            print(f"JSON Data: {data}")
        except ValueError:
            print("Invalid JSON")
    elif 'text/html' in content_type:
        print(f"HTML Content (length: {len(response.text)})")
    elif 'application/xml' in content_type:
        print(f"XML Content (length: {len(response.text)})")
    else:
        print(f"Other content: {content_type}")
    
    return response

def safe_request(url, method='GET', **kwargs):
    """Request อย่างปลอดภัย"""
    try:
        response = requests.request(method, url, **kwargs)
        response.raise_for_status()  # โยน exception สำหรับ 4xx/5xx
        return handle_response(response)
    except requests.exceptions.ConnectionError:
        print("❌ Connection Error")
    except requests.exceptions.Timeout:
        print("❌ Request Timeout")
    except requests.exceptions.HTTPError as e:
        print(f"❌ HTTP Error: {e}")
    except requests.exceptions.RequestException as e:
        print(f"❌ Request Error: {e}")
    except Exception as e:
        print(f"❌ Unexpected Error: {e}")
    
    return None

# การใช้งาน
response = safe_request('https://httpbin.org/get', params={'test': 'value'})
```

### Response Streaming
```python
def download_large_file(url, filename):
    """ดาวน์โหลดไฟล์ขนาดใหญ่"""
    try:
        response = requests.get(url, stream=True)
        response.raise_for_status()
        
        total_size = int(response.headers.get('content-length', 0))
        downloaded = 0
        
        with open(filename, 'wb') as file:
            for chunk in response.iter_content(chunk_size=8192):
                if chunk:  # filter out keep-alive chunks
                    file.write(chunk)
                    downloaded += len(chunk)
                    
                    # แสดงความคืบหน้า
                    if total_size > 0:
                        progress = (downloaded / total_size) * 100
                        print(f"\rDownloaded: {downloaded}/{total_size} bytes ({progress:.1f}%)", end='')
        
        print(f"\n✅ Downloaded: {filename}")
        return True
        
    except Exception as e:
        print(f"❌ Download Error: {e}")
        return False

def stream_api_response(url):
    """อ่าน response แบบ stream"""
    try:
        response = requests.get(url, stream=True)
        response.raise_for_status()
        
        for line in response.iter_lines():
            if line:
                # decode bytes to string
                data = line.decode('utf-8')
                print(f"Received: {data}")
                
    except Exception as e:
        print(f"❌ Streaming Error: {e}")
```

---

## 🎯 การใช้งานจริง

### Weather API Client
```python
import requests
from datetime import datetime

class WeatherAPIClient:
    """Client สำหรับ Weather API"""
    
    def __init__(self, api_key, base_url="https://api.openweathermap.org/data/2.5"):
        self.api_key = api_key
        self.base_url = base_url
        self.session = requests.Session()
    
    def get_current_weather(self, city, units="metric"):
        """ดูสภาพอากาศปัจจุบัน"""
        url = f"{self.base_url}/weather"
        params = {
            'q': city,
            'appid': self.api_key,
            'units': units,
            'lang': 'th'
        }
        
        try:
            response = self.session.get(url, params=params)
            response.raise_for_status()
            data = response.json()
            
            return {
                'city': data['name'],
                'country': data['sys']['country'],
                'temperature': data['main']['temp'],
                'feels_like': data['main']['feels_like'],
                'humidity': data['main']['humidity'],
                'description': data['weather'][0]['description'],
                'timestamp': datetime.now().isoformat()
            }
            
        except requests.exceptions.RequestException as e:
            print(f"Weather API Error: {e}")
            return None
    
    def get_forecast(self, city, days=5):
        """ดูพยากรณ์อากาศ"""
        url = f"{self.base_url}/forecast"
        params = {
            'q': city,
            'appid': self.api_key,
            'units': 'metric',
            'lang': 'th'
        }
        
        try:
            response = self.session.get(url, params=params)
            response.raise_for_status()
            data = response.json()
            
            forecast = []
            for item in data['list'][:days * 8]:  # 8 forecasts per day
                forecast.append({
                    'datetime': item['dt_txt'],
                    'temperature': item['main']['temp'],
                    'description': item['weather'][0]['description'],
                    'humidity': item['main']['humidity']
                })
            
            return forecast
            
        except requests.exceptions.RequestException as e:
            print(f"Forecast API Error: {e}")
            return None

# การใช้งาน
weather_client = WeatherAPIClient("your_api_key_here")
current = weather_client.get_current_weather("Bangkok")
if current:
    print(f"สภาพอากาศ {current['city']}: {current['temperature']}°C - {current['description']}")
```

### REST API Client
```python
import requests
import json
from typing import Dict, Any, Optional, List

class RESTAPIClient:
    """REST API Client ทั่วไป"""
    
    def __init__(self, base_url: str, api_key: str = None, timeout: int = 30):
        self.base_url = base_url.rstrip('/')
        self.api_key = api_key
        self.timeout = timeout
        self.session = requests.Session()
        
        # ตั้งค่า default headers
        self.session.headers.update({
            'Content-Type': 'application/json',
            'Accept': 'application/json',
            'User-Agent': 'Python REST Client/1.0'
        })
        
        # เพิ่ม API key header ถ้ามี
        if self.api_key:
            self.session.headers['Authorization'] = f'Bearer {self.api_key}'
    
    def _make_request(self, method: str, endpoint: str, **kwargs) -> Optional[Dict[Any, Any]]:
        """ทำ request ไปยัง API"""
        url = f"{self.base_url}/{endpoint.lstrip('/')}"
        
        try:
            response = self.session.request(
                method=method,
                url=url,
                timeout=self.timeout,
                **kwargs
            )
            response.raise_for_status()
            
            # คืนค่า JSON ถ้ามี content
            if response.content:
                return response.json()
            return {'success': True}
            
        except requests.exceptions.Timeout:
            print(f"❌ Timeout: {method} {url}")
        except requests.exceptions.ConnectionError:
            print(f"❌ Connection Error: {method} {url}")
        except requests.exceptions.HTTPError as e:
            print(f"❌ HTTP Error: {e}")
            if e.response.content:
                try:
                    error_data = e.response.json()
                    print(f"Error details: {error_data}")
                except ValueError:
                    print(f"Error response: {e.response.text}")
        except Exception as e:
            print(f"❌ Unexpected Error: {e}")
        
        return None
    
    def get(self, endpoint: str, params: Dict = None) -> Optional[Dict[Any, Any]]:
        """GET request"""
        return self._make_request('GET', endpoint, params=params)
    
    def post(self, endpoint: str, data: Dict = None) -> Optional[Dict[Any, Any]]:
        """POST request"""
        return self._make_request('POST', endpoint, json=data)
    
    def put(self, endpoint: str, data: Dict = None) -> Optional[Dict[Any, Any]]:
        """PUT request"""
        return self._make_request('PUT', endpoint, json=data)
    
    def delete(self, endpoint: str) -> Optional[Dict[Any, Any]]:
        """DELETE request"""
        return self._make_request('DELETE', endpoint)
    
    def patch(self, endpoint: str, data: Dict = None) -> Optional[Dict[Any, Any]]:
        """PATCH request"""
        return self._make_request('PATCH', endpoint, json=data)

# การใช้งานกับ JSONPlaceholder API
def test_jsonplaceholder():
    """ทดสอบกับ JSONPlaceholder API"""
    client = RESTAPIClient("https://jsonplaceholder.typicode.com")
    
    # GET ข้อมูลผู้ใช้
    users = client.get("/users")
    print(f"Found {len(users)} users")
    
    # GET ข้อมูลผู้ใช้เฉพาะ
    user = client.get("/users/1")
    print(f"User: {user['name']} ({user['email']})")
    
    # POST สร้างโพสต์ใหม่
    new_post = {
        "title": "My New Post",
        "body": "This is the content of my new post",
        "userId": 1
    }
    created = client.post("/posts", new_post)
    print(f"Created post with ID: {created['id']}")
    
    # PUT อัปเดตโพสต์
    updated_post = {
        "id": 1,
        "title": "Updated Title",
        "body": "Updated content",
        "userId": 1
    }
    updated = client.put("/posts/1", updated_post)
    print(f"Updated post: {updated['title']}")
    
    # DELETE ลบโพสต์
    deleted = client.delete("/posts/1")
    print(f"Delete result: {deleted}")

test_jsonplaceholder()
```

---

## 🎪 การจัดการ Rate Limiting และ Caching

### Rate Limiting
```python
import time
from collections import deque
from threading import Lock

class RateLimiter:
    """Rate Limiter สำหรับ API calls"""
    
    def __init__(self, max_calls: int, time_window: int):
        self.max_calls = max_calls
        self.time_window = time_window
        self.calls = deque()
        self.lock = Lock()
    
    def wait_if_needed(self):
        """รอถ้าจำเป็น"""
        with self.lock:
            now = time.time()
            
            # ลบ calls เก่าๆ ออก
            while self.calls and self.calls[0] <= now - self.time_window:
                self.calls.popleft()
            
            # ตรวจสอบว่าเกิน limit หรือไม่
            if len(self.calls) >= self.max_calls:
                # คำนวณเวลาที่ต้องรอ
                oldest_call = self.calls[0]
                sleep_time = self.time_window - (now - oldest_call)
                if sleep_time > 0:
                    time.sleep(sleep_time)
            
            # บันทึก call ปัจจุบัน
            self.calls.append(now)

class RateLimitedAPIClient:
    """API Client พร้อม Rate Limiting"""
    
    def __init__(self, base_url: str, max_calls: int = 100, time_window: int = 60):
        self.base_url = base_url
        self.rate_limiter = RateLimiter(max_calls, time_window)
        self.session = requests.Session()
    
    def make_request(self, endpoint: str, **kwargs):
        """ทำ request พร้อม rate limiting"""
        self.rate_limiter.wait_if_needed()
        
        url = f"{self.base_url}/{endpoint}"
        response = self.session.get(url, **kwargs)
        response.raise_for_status()
        return response.json()

# การใช้งาน
client = RateLimitedAPIClient("https://api.example.com", max_calls=10, time_window=60)

for i in range(15):
    try:
        data = client.make_request(f"data/{i}")
        print(f"Request {i+1}: Success")
    except Exception as e:
        print(f"Request {i+1}: Error - {e}")
```

### Caching
```python
import requests
import json
import time
from pathlib import Path
from typing import Optional, Any

class APICache:
    """Cache สำหรับ API responses"""
    
    def __init__(self, cache_dir: str = "api_cache", ttl: int = 3600):
        self.cache_dir = Path(cache_dir)
        self.cache_dir.mkdir(exist_ok=True)
        self.ttl = ttl  # Time to live in seconds
    
    def _get_cache_key(self, url: str, params: dict = None) -> str:
        """สร้าง cache key"""
        import hashlib
        key_data = f"{url}_{str(params) if params else ''}"
        return hashlib.md5(key_data.encode()).hexdigest()
    
    def _get_cache_path(self, cache_key: str) -> Path:
        """ดู path ของ cache file"""
        return self.cache_dir / f"{cache_key}.json"
    
    def get(self, url: str, params: dict = None) -> Optional[Any]:
        """ดึงข้อมูลจาก cache"""
        cache_key = self._get_cache_key(url, params)
        cache_path = self._get_cache_path(cache_key)
        
        if not cache_path.exists():
            return None
        
        try:
            with open(cache_path, 'r', encoding='utf-8') as file:
                cache_data = json.load(file)
            
            # ตรวจสอบว่าหมดอายุหรือไม่
            if time.time() - cache_data['timestamp'] > self.ttl:
                cache_path.unlink()  # ลบ cache เก่า
                return None
            
            return cache_data['data']
            
        except Exception:
            return None
    
    def set(self, url: str, data: Any, params: dict = None):
        """บันทึกข้อมูลลง cache"""
        cache_key = self._get_cache_key(url, params)
        cache_path = self._get_cache_path(cache_key)
        
        cache_data = {
            'timestamp': time.time(),
            'url': url,
            'params': params,
            'data': data
        }
        
        try:
            with open(cache_path, 'w', encoding='utf-8') as file:
                json.dump(cache_data, file, indent=2)
        except Exception as e:
            print(f"Cache write error: {e}")

class CachedAPIClient:
    """API Client พร้อม Caching"""
    
    def __init__(self, base_url: str, cache_ttl: int = 3600):
        self.base_url = base_url
        self.cache = APICache(ttl=cache_ttl)
        self.session = requests.Session()
    
    def get(self, endpoint: str, params: dict = None, use_cache: bool = True):
        """GET request พร้อม cache"""
        url = f"{self.base_url}/{endpoint}"
        
        # พยายามดูจาก cache ก่อน
        if use_cache:
            cached_data = self.cache.get(url, params)
            if cached_data is not None:
                print("📦 From cache")
                return cached_data
        
        # ถ้าไม่มีใน cache ให้ request จาก API
        print("🌐 From API")
        response = self.session.get(url, params=params)
        response.raise_for_status()
        data = response.json()
        
        # บันทึกลง cache
        if use_cache:
            self.cache.set(url, data, params)
        
        return data

# การใช้งาน
client = CachedAPIClient("https://jsonplaceholder.typicode.com", cache_ttl=300)  # 5 นาที

# Request ครั้งแรก (จาก API)
data1 = client.get("/posts/1")

# Request ครั้งที่สอง (จาก cache)
data2 = client.get("/posts/1")
```

---

## 🛠️ การจัดการ Error และ Retry

### Retry Mechanism
```python
import time
import random
from functools import wraps
from requests.exceptions import RequestException

def retry_on_failure(max_retries: int = 3, delay: float = 1.0, backoff: float = 2.0):
    """Decorator สำหรับ retry request"""
    def decorator(func):
        @wraps(func)
        def wrapper(*args, **kwargs):
            last_exception = None
            
            for attempt in range(max_retries + 1):
                try:
                    return func(*args, **kwargs)
                except RequestException as e:
                    last_exception = e
                    
                    if attempt == max_retries:
                        print(f"❌ Failed after {max_retries} retries")
                        break
                    
                    # คำนวณ delay พร้อม jitter
                    current_delay = delay * (backoff ** attempt)
                    jitter = random.uniform(0.1, 0.5) * current_delay
                    total_delay = current_delay + jitter
                    
                    print(f"⚠️ Retry {attempt + 1}/{max_retries} after {total_delay:.2f}s")
                    time.sleep(total_delay)
            
            raise last_exception
        
        return wrapper
    return decorator

class RobustAPIClient:
    """API Client พร้อม retry และ error handling"""
    
    def __init__(self, base_url: str, timeout: int = 30):
        self.base_url = base_url
        self.timeout = timeout
        self.session = requests.Session()
    
    @retry_on_failure(max_retries=3, delay=1.0, backoff=2.0)
    def get(self, endpoint: str, **kwargs):
        """GET request พร้อม retry"""
        url = f"{self.base_url}/{endpoint}"
        response = self.session.get(url, timeout=self.timeout, **kwargs)
        response.raise_for_status()
        return response.json()
    
    @retry_on_failure(max_retries=5, delay=0.5, backoff=1.5)
    def post(self, endpoint: str, **kwargs):
        """POST request พร้อม retry"""
        url = f"{self.base_url}/{endpoint}"
        response = self.session.post(url, timeout=self.timeout, **kwargs)
        response.raise_for_status()
        return response.json()

# การใช้งาน
client = RobustAPIClient("https://httpbin.org")

try:
    data = client.get("/get", params={'test': 'value'})
    print("✅ Request successful")
except Exception as e:
    print(f"❌ Final error: {e}")
```

### Circuit Breaker Pattern
```python
import time
from enum import Enum
from typing import Optional, Any

class CircuitState(Enum):
    CLOSED = "closed"
    OPEN = "open"
    HALF_OPEN = "half_open"

class CircuitBreaker:
    """Circuit Breaker สำหรับ API calls"""
    
    def __init__(self, failure_threshold: int = 5, recovery_timeout: int = 60):
        self.failure_threshold = failure_threshold
        self.recovery_timeout = recovery_timeout
        self.failure_count = 0
        self.last_failure_time = None
        self.state = CircuitState.CLOSED
    
    def __call__(self, func):
        def wrapper(*args, **kwargs):
            if self.state == CircuitState.OPEN:
                if self._should_attempt_reset():
                    self.state = CircuitState.HALF_OPEN
                else:
                    raise Exception("Circuit breaker is OPEN")
            
            try:
                result = func(*args, **kwargs)
                self._on_success()
                return result
            except Exception as e:
                self._on_failure()
                raise
        
        return wrapper
    
    def _should_attempt_reset(self) -> bool:
        """ตรวจสอบว่าควรพยายาม reset หรือไม่"""
        return (time.time() - self.last_failure_time) >= self.recovery_timeout
    
    def _on_success(self):
        """เมื่อสำเร็จ"""
        self.failure_count = 0
        self.state = CircuitState.CLOSED
    
    def _on_failure(self):
        """เมื่อล้มเหลว"""
        self.failure_count += 1
        self.last_failure_time = time.time()
        
        if self.failure_count >= self.failure_threshold:
            self.state = CircuitState.OPEN

# การใช้งาน
@CircuitBreaker(failure_threshold=3, recovery_timeout=30)
def unreliable_api_call():
    """API call ที่ไม่น่าเชื่อถือ"""
    import random
    if random.random() < 0.7:  # 70% โอกาสล้มเหลว
        raise Exception("API call failed")
    return {"status": "success", "data": "some data"}

# ทดสอบ
for i in range(10):
    try:
        result = unreliable_api_call()
        print(f"✅ Call {i+1}: Success")
    except Exception as e:
        print(f"❌ Call {i+1}: {e}")
    
    time.sleep(1)
```

---

## 💡 Best Practices

### 1. ใช้ Sessions สำหรับ Multiple Requests
```python
# ✅ ดี: ใช้ session
session = requests.Session()
session.headers.update({'Authorization': 'Bearer token'})

for i in range(10):
    response = session.get(f'https://api.example.com/data/{i}')

# ❌ ไม่ดี: สร้าง connection ใหม่ทุกครั้ง
for i in range(10):
    response = requests.get(f'https://api.example.com/data/{i}', 
                           headers={'Authorization': 'Bearer token'})
```

### 2. ตั้งค่า Timeout เสมอ
```python
# ✅ ดี: ตั้งค่า timeout
response = requests.get(url, timeout=30)

# ❌ ไม่ดี: ไม่มี timeout อาจรอชั่วนิจนิตร
response = requests.get(url)
```

### 3. จัดการ Errors อย่างเหมาะสม
```python
# ✅ ดี: จัดการ errors ตามประเภท
try:
    response = requests.get(url, timeout=30)
    response.raise_for_status()
    return response.json()
except requests.exceptions.Timeout:
    print("Request timeout")
except requests.exceptions.ConnectionError:
    print("Connection error")
except requests.exceptions.HTTPError as e:
    print(f"HTTP error: {e}")
except requests.exceptions.RequestException as e:
    print(f"Request error: {e}")

# ❌ ไม่ดี: จัดการรวมๆ
try:
    response = requests.get(url)
    return response.json()
except Exception as e:
    print(f"Error: {e}")
```

### 4. ใช้ Environment Variables สำหรับ API Keys
```python
# ✅ ดี: ใช้ environment variables
import os
from dotenv import load_dotenv

load_dotenv()
api_key = os.getenv('API_KEY')

# ❌ ไม่ดี: hardcode API keys
api_key = "sk-1234567890abcdef"
```

---

## 📝 สรุป HTTP Status Codes

| Range | Status Codes | ความหมาย |
|-------|-------------|-----------|
| **2xx** | 200, 201, 204 | Success |
| **3xx** | 301, 302, 304 | Redirection |
| **4xx** | 400, 401, 403, 404 | Client Error |
| **5xx** | 500, 502, 503 | Server Error |

---

## 🎯 แบบฝึกหัด

### แบบฝึกหัด 1: Weather Client
```python
# เขียน client สำหรับ weather API
class WeatherClient:
    pass
```

### แบบฝึกหัด 2: File Upload API
```python
# เขียนฟังก์ชันอัปโหลดไฟล์ไปยัง API
def upload_file(api_url, file_path):
    pass
```

### แบบฝึกหัด 3: API Monitor
```python
# เขียนเครื่องมือตรวจสอบสถานะ API
class APIMonitor:
    pass
```

---

*สร้างเมื่อ: 4 มีนาคม 2026*  
*ปรับปรุงล่าสุด: 4 มีนาคม 2026*
