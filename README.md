# 🎯 Intervue – Live Polling & Quiz System

A **real-time polling & AI-powered quiz platform** for teachers and students with live results, chat, analytics, and seamless WebSocket communication.

🌐 **Live Demo**: [https://poll.nirajjha.xyz](https://poll.nirajjha.xyz)
📦 **Repository**: [https://github.com/jha-niraj/Intervue-Live-Poll](https://github.com/jha-niraj/Intervue-Live-Poll)

---

## ⭐ Key Features

### 🗳️ **Live Polling**

- Real-time poll creation and responses
- Animated results & smart timer
- Auto-close on timeout or all responses
- Poll history & analytics
- Participant tracking

### 🤖 **AI Quiz System**

- Generate quizzes instantly using **OpenAI**
- Difficulty levels (Easy/Medium/Hard)
- Progress tracking & instant scoring
- Leaderboard with top performers
- Retry quizzes anytime

### 💬 **Communication**

- Real-time chat using Socket.io
- Online participants list
- Smart poll notifications across all pages
- Role-based UI for teacher & student

### 🎨 **User Experience**

- Fully responsive (mobile/tablet/desktop)
- Modern UI with smooth animations
- Redux-powered state management
- Clean error handling & session management

---

## 🏗️ Tech Stack

**Frontend:** React 19, TypeScript, Redux Toolkit, Tailwind CSS
**Backend:** Express.js, TypeScript, Socket.io
**Database:** PostgreSQL + Prisma ORM
**AI:** OpenAI API
**Deployment:**

- Frontend → Vercel
- Backend → Render
- DB → Render PostgreSQL

---

## 🚀 Getting Started

### Prerequisites

- Node.js 18+
- PostgreSQL
- OpenAI API Key

### Installation

```bash
# Clone repo
git clone https://github.com/jha-niraj/Intervue-Live-Poll.git
cd Intervue-Live-Poll
```

#### Backend

```bash
cd server
npm install
cp .env.example .env   # add DB + OPENAI_KEY
npm run prisma:generate
npm run dev
```

#### Frontend

```bash
cd ../client
npm install
npm run dev
```

---

## 🔌 Architecture Overview

```
Frontend (React + Redux)
    ↕ WebSocket (Socket.io)
Backend (Express + Socket.io)
    ↕
PostgreSQL (via Prisma)
```

---

## 🧪 Testing Locally

1. Open **Teacher** in one tab
2. Open **Student** in another tab
3. Create poll → Answer → View live updates
4. Test quizzes, chat, notifications

---

## 🤝 Contributing

Suggestions and improvements are welcome.
This project was built with ❤️ by **Aditya Kalburgi**.

---

## 📄 License

MIT License
