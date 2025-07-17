# 🚀 NextJS Template API CMS

A blazing fast, minimal, and extensible **Next.js template** for building modern **CMS APIs** with support for Firebase, RESTful endpoints, authentication, and full backend-service structure.

> Built with ❤️ by Phuong Linh Ventures – powering the digital future of content management.

---

## 🌟 Features

- ⚡️ **Next.js App Router** (latest best practices)
- 🔥 **Firebase Firestore** as database (NoSQL, scalable)
- 🧩 Modular **API route structure** (RESTful)
- 🔐 Built-in **authentication middleware** (API Key / Token-based)
- 🗃️ Example modules: Posts, Categories, Users
- 🛠️ Easy to plug into any CMS frontend
- 💼 Suitable for internal tools, admin dashboards, or external CMS platforms

---

## 📁 Project Structure

```
.
├── app/                      # Next.js App Router
│   ├── api/                  # API routes
│   │   └── v1/
│   │       ├── posts/
│   │       ├── categories/
│   │       └── users/
│   └── page.tsx             # Landing / root route
│
├── lib/
│   ├── firebase.ts          # Firebase config
│   ├── firestore.ts         # Firestore helper functions
│   └── middleware.ts        # Authentication middleware
│
├── services/                # Business logic layer
│   ├── PostService.ts
│   ├── CategoryService.ts
│   └── UserService.ts
│
├── types/                   # TypeScript interfaces
├── .env.local               # API keys & secrets
└── README.md                # This file
```

---

## ⚙️ Getting Started

### 1. Clone the repo

```bash
git clone https://github.com/your-org/NextJS-Template-API-CMS.git
cd NextJS-Template-API-CMS
```

### 2. Install dependencies

```bash
npm install
```

### 3. Setup Firebase credentials

Tạo file `.env.local` và thêm:

```env
NEXT_PUBLIC_FIREBASE_API_KEY=your_api_key
NEXT_PUBLIC_FIREBASE_AUTH_DOMAIN=your_project.firebaseapp.com
NEXT_PUBLIC_FIREBASE_PROJECT_ID=your_project
NEXT_PUBLIC_FIREBASE_STORAGE_BUCKET=your_project.appspot.com
NEXT_PUBLIC_FIREBASE_MESSAGING_SENDER_ID=your_id
NEXT_PUBLIC_FIREBASE_APP_ID=your_app_id
FIREBASE_ADMIN_PRIVATE_KEY="-----BEGIN PRIVATE KEY-----\n...\n-----END PRIVATE KEY-----\n"
FIREBASE_ADMIN_CLIENT_EMAIL=your_service_account_email
```

> Đăng nhập Firebase > Project Settings > Service Accounts để lấy các thông tin trên.

### 4. Run Dev Server

```bash
npm run dev
```

Truy cập: [http://localhost:3000](http://localhost:3000)

---

## 🧪 Sample API Endpoints

| Method | Endpoint                  | Description          |
|--------|---------------------------|----------------------|
| GET    | `/api/v1/posts`           | Get all posts        |
| POST   | `/api/v1/posts`           | Create a new post    |
| GET    | `/api/v1/categories`      | List categories      |
| GET    | `/api/v1/users/:id`       | Get user by ID       |

> Sử dụng `Authorization: Bearer <API_KEY>` trong header để truy cập.

---

## 📌 Deployment

Có thể deploy nhanh gọn trên:

- **Vercel** (recommended)
- **Firebase Hosting**
- **Render / Railway / Heroku**

---

## 📚 Tech Stack

- [Next.js](https://nextjs.org/)
- [Firebase Firestore](https://firebase.google.com/docs/firestore)
- [TypeScript](https://www.typescriptlang.org/)
- [Node.js](https://nodejs.org/)
- [Vercel](https://vercel.com/)

---

## 🧠 Mục tiêu phát triển

- [ ] Hỗ trợ dynamic roles & permissions
- [ ] Tích hợp OpenAPI docs
- [ ] Giao diện Admin UI sẵn sàng
- [ ] Kết nối đa CMS frontend (Next.js, Nuxt, Flutter, v.v.)

---

## 📞 Liên hệ & Đóng góp

💌 Gửi tình yêu hoặc đóng góp vào repo:  
📧 hello@phuonglinh.ventures  
🌐 [phuonglinh.ventures](https://phuonglinh.ventures)

---

> “Không có hệ thống nào là nhỏ – chỉ có hệ thống chưa được đầu tư đúng tầm.”

Made with 💚 by **Phuong Linh Ventures Tech Lab**.
