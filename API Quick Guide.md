# 🚀 Hướng dẫn tích hợp API cho Khách hàng

## 📋 **Thông tin API**

| Thông tin | Chi tiết |
|-----------|----------|
| **Base URL** | `https://cms-one-mauve.vercel.app/api/v1` |
| **Protocol** | HTTP/1.1 REST API |
| **Authentication** | API Key |
| **Data Format** | JSON |
| **Content-Type** | `application/json` |

---

## ✅ **Xác thực API Key**

### **Lấy API Key:**
1. Đăng nhập vào dashboard của bạn
2. Vào phần **API Keys**
3. Tạo API Key mới
4. Copy API Key (dạng: `pk_live_xxxxx...`)

### **Sử dụng API Key trong request:**

**✅ Cách 1: X-Api-Key Header (Khuyến nghị)**
```http
X-Api-Key: your_api_key_here
Content-Type: application/json
```

**✅ Cách 2: Authorization Bearer**
```http
Authorization: Bearer your_api_key_here
Content-Type: application/json
```

---

## 📡 **Endpoints có sẵn**

### **📄 Posts (Bài viết)**
| Method | Endpoint | Mô tả |
|--------|----------|-------|
| `GET` | `/posts` | Lấy danh sách bài viết |
| `POST` | `/posts` | Tạo bài viết mới |
| `GET` | `/posts/{id}` | Lấy bài viết theo ID |
| `PUT` | `/posts/{id}` | Cập nhật bài viết |
| `DELETE` | `/posts/{id}` | Xóa bài viết |

### **🏷️ Tags (Thẻ)**
| Method | Endpoint | Mô tả |
|--------|----------|-------|
| `GET` | `/tags` | Lấy danh sách thẻ |
| `POST` | `/tags` | Tạo thẻ mới |

### **📂 Categories (Danh mục)**
| Method | Endpoint | Mô tả |
|--------|----------|-------|
| `GET` | `/categories` | Lấy danh sách danh mục |
| `POST` | `/categories` | Tạo danh mục mới |

### **🖼️ Media (Tệp media)**
| Method | Endpoint | Mô tả |
|--------|----------|-------|
| `GET` | `/media` | Lấy danh sách media |
| `POST` | `/media` | Upload tệp media |

### **📊 Analytics (Thống kê)**
| Method | Endpoint | Mô tả |
|--------|----------|-------|
| `GET` | `/analytics` | Lấy báo cáo thống kê |
| `POST` | `/analytics` | Ghi nhận lượt xem |

---

## 💻 **Ví dụ theo ngôn ngữ lập trình**

### **🌐 JavaScript/TypeScript**

**Cài đặt:** Không cần thư viện bổ sung (sử dụng fetch có sẵn)

```javascript
class CMSClient {
  constructor(apiKey, baseUrl = 'https://cms-one-mauve.vercel.app/api/v1') {
    this.apiKey = apiKey;
    this.baseUrl = baseUrl;
  }

  async request(endpoint, options = {}) {
    const url = `${this.baseUrl}${endpoint}`;
    const config = {
      headers: {
        'X-Api-Key': this.apiKey,
        'Content-Type': 'application/json',
        ...options.headers
      },
      ...options
    };

    if (options.body && typeof options.body === 'object') {
      config.body = JSON.stringify(options.body);
    }

    const response = await fetch(url, config);
    const data = await response.json();

    if (!response.ok) {
      throw new Error(data.error || `HTTP ${response.status}`);
    }

    return data;
  }

  // Lấy danh sách bài viết
  async getPosts(params = {}) {
    const query = new URLSearchParams(params).toString();
    const endpoint = query ? `/posts?${query}` : '/posts';
    return this.request(endpoint);
  }

  // Tạo bài viết mới
  async createPost(postData) {
    return this.request('/posts', {
      method: 'POST',
      body: postData
    });
  }

  // Lấy bài viết theo ID
  async getPost(id) {
    return this.request(`/posts/${id}`);
  }

  // Cập nhật bài viết
  async updatePost(id, postData) {
    return this.request(`/posts/${id}`, {
      method: 'PUT',
      body: postData
    });
  }

  // Xóa bài viết
  async deletePost(id) {
    return this.request(`/posts/${id}`, {
      method: 'DELETE'
    });
  }

  // Lấy danh sách tags
  async getTags() {
    return this.request('/tags');
  }

  // Tạo tag mới
  async createTag(tagData) {
    return this.request('/tags', {
      method: 'POST',
      body: tagData
    });
  }

  // Lấy analytics
  async getAnalytics(days = 30) {
    return this.request(`/analytics?days=${days}`);
  }
}

// Sử dụng
const client = new CMSClient('your_api_key_here');

// Ví dụ sử dụng
async function example() {
  try {
    // Lấy danh sách bài viết
    const posts = await client.getPosts({ status: 'published', limit: 10 });
    console.log(`Tìm thấy ${posts.total} bài viết:`, posts.data);

    // Tạo bài viết mới
    const newPost = await client.createPost({
      title: 'Bài viết mới',
      content: 'Nội dung bài viết...',
      status: 'published'
    });
    console.log('Bài viết đã tạo:', newPost.data);

    // Lấy thống kê
    const analytics = await client.getAnalytics(7);
    console.log('Thống kê 7 ngày:', analytics.data);

  } catch (error) {
    console.error('Lỗi:', error.message);
  }
}

example();
```

### **🐍 Python**

**Cài đặt:**
```bash
pip install requests
```

**Ví dụ sử dụng:**
```python
import requests
import json

class CMSClient:
    def __init__(self, api_key, base_url='https://cms-one-mauve.vercel.app/api/v1'):
        self.api_key = api_key
        self.base_url = base_url
        self.headers = {
            'X-Api-Key': api_key,
            'Content-Type': 'application/json'
        }
    
    def _request(self, method, endpoint, data=None, params=None):
        url = f'{self.base_url}{endpoint}'
        
        kwargs = {
            'headers': self.headers,
            'params': params
        }
        
        if data:
            kwargs['json'] = data
            
        response = requests.request(method, url, **kwargs)
        
        if response.status_code >= 400:
            error_data = response.json() if response.content else {}
            raise Exception(f"API Error {response.status_code}: {error_data.get('error', 'Unknown error')}")
            
        return response.json()
    
    def get_posts(self, **params):
        """Lấy danh sách bài viết"""
        return self._request('GET', '/posts', params=params)
    
    def create_post(self, title, content, **kwargs):
        """Tạo bài viết mới"""
        data = {
            'title': title,
            'content': content,
            **kwargs
        }
        return self._request('POST', '/posts', data=data)
    
    def get_post(self, post_id):
        """Lấy bài viết theo ID"""
        return self._request('GET', f'/posts/{post_id}')
    
    def update_post(self, post_id, **data):
        """Cập nhật bài viết"""
        return self._request('PUT', f'/posts/{post_id}', data=data)
    
    def delete_post(self, post_id):
        """Xóa bài viết"""
        return self._request('DELETE', f'/posts/{post_id}')
    
    def get_tags(self):
        """Lấy danh sách tags"""
        return self._request('GET', '/tags')
    
    def create_tag(self, name, **kwargs):
        """Tạo tag mới"""
        data = {
            'name': name,
            **kwargs
        }
        return self._request('POST', '/tags', data=data)
    
    def get_analytics(self, days=30):
        """Lấy thống kê"""
        return self._request('GET', '/analytics', params={'days': days})

# Sử dụng
if __name__ == '__main__':
    client = CMSClient('your_api_key_here')
    
    try:
        # Lấy danh sách bài viết
        posts = client.get_posts(status='published', limit=5)
        print(f"Tìm thấy {posts['total']} bài viết")
        
        # Tạo bài viết mới
        new_post = client.create_post(
            title='Bài viết từ Python',
            content='Nội dung bài viết được tạo từ Python',
            status='published'
        )
        print(f"Đã tạo bài viết ID: {new_post['data']['id']}")
        
        # Lấy thống kê
        analytics = client.get_analytics(7)
        print(f"Tổng lượt xem 7 ngày: {analytics['data']['totalViews']}")
        
    except Exception as e:
        print(f"Lỗi: {e}")
```

### **🌐 PHP**

**Ví dụ sử dụng:**
```php
<?php

class CMSClient {
    private $apiKey;
    private $baseUrl;
    
    public function __construct($apiKey, $baseUrl = 'https://cms-one-mauve.vercel.app/api/v1') {
        $this->apiKey = $apiKey;
        $this->baseUrl = $baseUrl;
    }
    
    private function request($method, $endpoint, $data = null) {
        $url = $this->baseUrl . $endpoint;
        
        $headers = [
            'X-Api-Key: ' . $this->apiKey,
            'Content-Type: application/json'
        ];
        
        $ch = curl_init();
        curl_setopt($ch, CURLOPT_URL, $url);
        curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
        curl_setopt($ch, CURLOPT_HTTPHEADER, $headers);
        curl_setopt($ch, CURLOPT_CUSTOMREQUEST, $method);
        
        if ($data && in_array($method, ['POST', 'PUT', 'PATCH'])) {
            curl_setopt($ch, CURLOPT_POSTFIELDS, json_encode($data));
        }
        
        $response = curl_exec($ch);
        $httpCode = curl_getinfo($ch, CURLINFO_HTTP_CODE);
        curl_close($ch);
        
        $responseData = json_decode($response, true);
        
        if ($httpCode >= 400) {
            throw new Exception($responseData['error'] ?? "HTTP Error $httpCode");
        }
        
        return $responseData;
    }
    
    public function getPosts($params = []) {
        $endpoint = '/posts';
        if (!empty($params)) {
            $endpoint .= '?' . http_build_query($params);
        }
        return $this->request('GET', $endpoint);
    }
    
    public function createPost($title, $content, $additionalData = []) {
        $data = array_merge([
            'title' => $title,
            'content' => $content
        ], $additionalData);
        
        return $this->request('POST', '/posts', $data);
    }
    
    public function getPost($id) {
        return $this->request('GET', "/posts/$id");
    }
    
    public function updatePost($id, $data) {
        return $this->request('PUT', "/posts/$id", $data);
    }
    
    public function deletePost($id) {
        return $this->request('DELETE', "/posts/$id");
    }
    
    public function getTags() {
        return $this->request('GET', '/tags');
    }
    
    public function createTag($name, $additionalData = []) {
        $data = array_merge(['name' => $name], $additionalData);
        return $this->request('POST', '/tags', $data);
    }
    
    public function getAnalytics($days = 30) {
        return $this->request('GET', "/analytics?days=$days");
    }
}

// Sử dụng
try {
    $client = new CMSClient('your_api_key_here');
    
    // Lấy danh sách bài viết
    $posts = $client->getPosts(['status' => 'published', 'limit' => 5]);
    echo "Tìm thấy " . $posts['total'] . " bài viết\n";
    
    // Tạo bài viết mới
    $newPost = $client->createPost(
        'Bài viết từ PHP',
        'Nội dung bài viết được tạo từ PHP',
        ['status' => 'published']
    );
    echo "Đã tạo bài viết ID: " . $newPost['data']['id'] . "\n";
    
    // Lấy thống kê
    $analytics = $client->getAnalytics(7);
    echo "Tổng lượt xem 7 ngày: " . $analytics['data']['totalViews'] . "\n";
    
} catch (Exception $e) {
    echo "Lỗi: " . $e->getMessage() . "\n";
}

?>
```

### **💎 Ruby**

**Cài đặt:**
```bash
gem install httparty
```

**Ví dụ sử dụng:**
```ruby
require 'httparty'

class CMSClient
  include HTTParty
  
  def initialize(api_key, base_uri = 'https://cms-one-mauve.vercel.app/api/v1')
    @headers = {
      'X-Api-Key' => api_key,
      'Content-Type' => 'application/json'
    }
    self.class.base_uri base_uri
  end
  
  def get_posts(**params)
    response = self.class.get('/posts', headers: @headers, query: params)
    handle_response(response)
  end
  
  def create_post(title, content, **additional_data)
    data = { title: title, content: content }.merge(additional_data)
    response = self.class.post('/posts', headers: @headers, body: data.to_json)
    handle_response(response)
  end
  
  def get_post(id)
    response = self.class.get("/posts/#{id}", headers: @headers)
    handle_response(response)
  end
  
  def update_post(id, **data)
    response = self.class.put("/posts/#{id}", headers: @headers, body: data.to_json)
    handle_response(response)
  end
  
  def delete_post(id)
    response = self.class.delete("/posts/#{id}", headers: @headers)
    handle_response(response)
  end
  
  def get_tags
    response = self.class.get('/tags', headers: @headers)
    handle_response(response)
  end
  
  def create_tag(name, **additional_data)
    data = { name: name }.merge(additional_data)
    response = self.class.post('/tags', headers: @headers, body: data.to_json)
    handle_response(response)
  end
  
  def get_analytics(days = 30)
    response = self.class.get('/analytics', headers: @headers, query: { days: days })
    handle_response(response)
  end
  
  private
  
  def handle_response(response)
    if response.success?
      response.parsed_response
    else
      error_message = response.parsed_response['error'] rescue "HTTP Error #{response.code}"
      raise "API Error: #{error_message}"
    end
  end
end

# Sử dụng
begin
  client = CMSClient.new('your_api_key_here')
  
  # Lấy danh sách bài viết
  posts = client.get_posts(status: 'published', limit: 5)
  puts "Tìm thấy #{posts['total']} bài viết"
  
  # Tạo bài viết mới
  new_post = client.create_post(
    'Bài viết từ Ruby',
    'Nội dung bài viết được tạo từ Ruby',
    status: 'published'
  )
  puts "Đã tạo bài viết ID: #{new_post['data']['id']}"
  
  # Lấy thống kê
  analytics = client.get_analytics(7)
  puts "Tổng lượt xem 7 ngày: #{analytics['data']['totalViews']}"
  
rescue => e
  puts "Lỗi: #{e.message}"
end
```

---

## 📊 **Định dạng Response**

### **✅ Response thành công:**
```json
{
  "success": true,
  "data": {
    "id": "abc123",
    "title": "Tiêu đề bài viết",
    "content": "Nội dung...",
    "status": "published",
    "organizationId": "org_123",
    "createdAt": "2025-07-17T10:30:00Z",
    "updatedAt": "2025-07-17T10:30:00Z"
  },
  "total": 1
}
```

### **❌ Response lỗi:**
```json
{
  "success": false,
  "error": "Mô tả lỗi cụ thể"
}
```

---

## 🔧 **Query Parameters**

### **Posts API:**
```
?status=published        # Lọc theo trạng thái
?categoryId=123         # Lọc theo danh mục
?authorId=456           # Lọc theo tác giả
?limit=10               # Giới hạn số kết quả
?search=keyword         # Tìm kiếm theo từ khóa
```

### **Analytics API:**
```
?days=30                # Thống kê trong 30 ngày qua
```

**Ví dụ sử dụng:**
```
GET /api/v1/posts?status=published&limit=5
GET /api/v1/analytics?days=7
```

---

## ⚡ **Quick Test với cURL**

### **Test kết nối:**
```bash
curl -H "X-Api-Key: your_api_key_here" \
     https://cms-one-mauve.vercel.app/api/v1/posts
```

### **Tạo bài viết mới:**
```bash
curl -X POST \
     -H "X-Api-Key: your_api_key_here" \
     -H "Content-Type: application/json" \
     -d '{
       "title": "Test Post",
       "content": "Test content",
       "status": "published"
     }' \
     https://cms-one-mauve.vercel.app/api/v1/posts
```

### **Lấy thống kê:**
```bash
curl -H "X-Api-Key: your_api_key_here" \
     https://cms-one-mauve.vercel.app/api/v1/analytics?days=7
```

---

## 🚨 **HTTP Status Codes**

| Code | Ý nghĩa | Mô tả |
|------|---------|-------|
| `200` | OK | Thành công |
| `201` | Created | Tạo mới thành công |
| `400` | Bad Request | Dữ liệu đầu vào không hợp lệ |
| `401` | Unauthorized | API key không hợp lệ |
| `403` | Forbidden | Không có quyền truy cập |
| `404` | Not Found | Không tìm thấy resource |
| `429` | Too Many Requests | Vượt quá rate limit |
| `500` | Internal Server Error | Lỗi server |

---

## � **Bảo mật & Best Practices**

### **🔑 API Key Security:**
- ✅ **Lưu trữ an toàn:** Sử dụng environment variables
- ✅ **Không commit:** Không đưa API key vào code repository
- ✅ **HTTPS only:** Chỉ sử dụng HTTPS trong production
- ✅ **Rotate keys:** Thường xuyên đổi API key

### **⚡ Performance:**
- ✅ **Rate limiting:** Tôn trọng giới hạn request/phút
- ✅ **Pagination:** Sử dụng limit cho datasets lớn
- ✅ **Error handling:** Xử lý lỗi một cách graceful
- ✅ **Retry logic:** Implement retry cho network failures

### **📝 Data Handling:**
- ✅ **Validate input:** Luôn validate dữ liệu trước khi gửi
- ✅ **Check success:** Kiểm tra field `success` trong response
- ✅ **Handle errors:** Xử lý các error codes appropriately

---

## 📞 **Support & Resources**

### **📚 Documentation:**
- API Reference: `/docs/api`
- Examples: `/docs/examples`
- SDKs: Available cho major languages

### **🛠️ Development:**
- **Environment:** Sandbox environment available
- **Testing:** Use provided test API keys
- **Rate Limits:** 1000 requests/hour (adjustable)

### **🆘 Help:**
- **Status Page:** Check API status
- **Support Email:** api-support@cms-one-mauve.vercel.app
- **Discord/Slack:** Developer community

---

## 🌟 **Tính năng nổi bật**

- ✅ **Multi-tenant:** Dữ liệu tự động phân tách theo organization
- ✅ **Real-time:** WebSocket support cho real-time updates
- ✅ **File Upload:** Hỗ trợ upload images, documents
- ✅ **Search:** Full-text search capabilities  
- ✅ **Analytics:** Built-in analytics tracking
- ✅ **Webhooks:** Event-driven notifications
- ✅ **SDKs:** Official libraries cho popular languages

**🚀 Sẵn sàng tích hợp ngay hôm nay!**
