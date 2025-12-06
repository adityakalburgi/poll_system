# 🎨 Intervue Client

Frontend application for the Intervue Live Polling & Quiz System. Built with React 19, TypeScript, Redux Toolkit, and Tailwind CSS.

---

## 🚀 Quick Start

### Prerequisites

- Node.js 18+
- Running backend server

### Installation

```bash
# Navigate to client directory
cd client

# Install dependencies
npm install

# Start development server
npm run dev
```

Application runs on `http://localhost:5173`

---

## 📁 Project Structure

```
client/
├── src/
│   ├── components/          # React components
│   │   ├── Badge.tsx
│   │   ├── ChatPopup.tsx
│   │   ├── ErrorFallback.tsx
│   │   ├── KickedUser.tsx
│   │   ├── PollNotification.tsx
│   │   ├── PollResults.tsx
│   │   ├── QuizCreate.tsx
│   │   ├── QuizList.tsx
│   │   ├── QuizTake.tsx
│   │   ├── RoleSelection.tsx
│   │   ├── StudentNameEntry.tsx
│   │   ├── StudentPoll.tsx
│   │   ├── StudentWaiting.tsx
│   │   ├── TeacherCreatePoll.tsx
│   │   └── TeacherDashboard.tsx
│   ├── config/
│   │   └── urls.ts          # API & Socket URLs
│   ├── store/
│   │   ├── store.ts         # Redux store
│   │   ├── quizSlice.ts     # Quiz state
│   │   └── hooks.ts         # Typed hooks
│   ├── types/
│   │   └── index.ts         # TypeScript types
│   ├── utils/
│   │   ├── socket.ts        # WebSocket client
│   │   ├── sessionStorage.ts
│   │   └── api-reference.ts # Axios reference
│   ├── App.tsx              # Main app component
│   ├── main.tsx             # Entry point
│   └── index.css            # Global styles
├── public/                  # Static assets
├── .env                     # Environment variables
└── package.json
```

---

## 🎯 Key Features

### Real-time Polling

- Live poll updates via WebSocket
- Animated progress bars
- Timer countdown
- Instant result display

### AI Quiz System

- Browse available quizzes
- Create quizzes with AI
- Take quizzes with progress tracking
- View leaderboard
- Retry functionality

### Smart Notifications

- **Poll notifications appear even while taking quizzes!**
- Slide-in animation from top-right
- Poll question preview
- Quick navigation to poll
- Dismiss option

### Communication

- Real-time chat
- Participant list
- Role-based message styling
- Auto-scroll to latest

### State Management

- Redux for quiz data
- Eliminates redundant API calls
- Proper cleanup on unmount

---

## 🔔 Poll Notification System

**How it works:**

1. Student is on any quiz page (`/quiz`, `/quiz/create`, `/quiz/:id`)
2. Teacher creates a poll
3. Notification slides in from top-right corner
4. Shows poll question preview
5. Student can:
   - Click "Join Poll" → Navigate to poll page
   - Click "Later" → Dismiss notification
6. Notification auto-hides when poll ends

**Component:** `PollNotification.tsx`

```typescript
// Listens for new polls
socket.on("poll:new", (data) => {
  setPollQuestion(data.poll.question);
  setShowNotification(true);
});

// Auto-hide when poll ends
socket.on("poll:ended", () => {
  setShowNotification(false);
});
```

---

## 🗺️ Routes

```typescript
// Main routes
/                          # Role selection
/student/name              # Student name entry
/student/poll              # Student poll page
/teacher/create            # Teacher dashboard

// Quiz routes
/quiz                      # Quiz list
/quiz/create               # Create quiz
/quiz/:quizId              # Take quiz
```

---

## 🎨 Components Overview

### Core Components

**RoleSelection** - Choose teacher or student role  
**StudentNameEntry** - Enter name to join  
**StudentPoll** - Answer polls and view results  
**StudentWaiting** - Wait for teacher's poll  
**TeacherDashboard** - Create polls and manage students  
**TeacherCreatePoll** - Poll creation form  
**PollResults** - Display live results

### Quiz Components

**QuizList** - Browse all quizzes  
**QuizCreate** - Generate quiz with AI  
**QuizTake** - Take quiz with leaderboard

### Utility Components

**ChatPopup** - Real-time chat with tabs  
**PollNotification** - Poll alerts on quiz pages  
**ErrorFallback** - Error handling UI  
**KickedUser** - Kicked student screen  
**Badge** - Reusable badge component

---

## 🔌 WebSocket Integration

### Socket Initialization

```typescript
// Initialize socket
import { initSocket, getSocket } from "./utils/socket";

const socket = initSocket();

// Listen for events
socket.on("poll:new", (data) => {
  // Handle new poll
});

// Emit events
socket.emit("student:join", { name, sessionId });
```

### Auto-initialization in ChatPopup

ChatPopup automatically initializes socket if not already connected:

```typescript
// Ensures socket exists
let socket = getSocket();
if (!socket) {
  socket = initSocket();

  // Auto-register based on role
  if (role === "student") {
    socket.emit("student:join", { name, sessionId });
  }
}
```

---

## 🎨 Styling

### Tailwind CSS Configuration

```javascript
// tailwind.config.js
theme: {
    extend: {
        colors: {
            purple: { 600: '#8F64E1' },
            blue: { 600: '#1D68BD' }
        },
        animation: {
            'slide-in': 'slide-in 0.3s ease-out'
        }
    }
}
```

### Design System

- **Primary Gradient**: `from-[#8F64E1] to-[#1D68BD]`
- **Background**: `bg-[#F2F2F2]`
- **Buttons**: `rounded-3xl` with gradient
- **Cards**: `rounded-xl` with shadow
- **Animations**: Smooth transitions

---

## 🔧 Configuration

### Environment Variables

```bash
# .env
VITE_SOCKET_URL=http://localhost:3000
```

### URL Configuration

```typescript
// src/config/urls.ts
const isDevelopment = import.meta.env.DEV;

export const SERVER_URL = isDevelopment
  ? "http://localhost:3000"
  : "https://your-backend.onrender.com";

export const serverUrl = `${SERVER_URL}/api/v1`;
export const SOCKET_URL = SERVER_URL;
```

---

## 🛠️ Available Scripts

```bash
# Development
npm run dev              # Start dev server

# Build
npm run build            # Build for production
npm run preview          # Preview production build

# Linting
npm run lint             # Run ESLint
```

---

## 📦 Dependencies

### Core

- **react** (19.x) - UI library
- **react-router-dom** - Routing
- **socket.io-client** - WebSocket client
- **axios** - HTTP client
- **@reduxjs/toolkit** - State management
- **react-redux** - Redux bindings

### UI

- **tailwindcss** - Utility-first CSS
- **@tailwindcss/forms** - Form styles

### Dev Tools

- **typescript** - Type safety
- **vite** - Build tool
- **eslint** - Linting

---

## 🚢 Deployment

### Vercel Deployment

1. **Connect Repository**

   - Import from GitHub
   - Select `client` as root directory

2. **Configure Build**

   - Build Command: `npm run build`
   - Output Directory: `dist`
   - Install Command: `npm install`

3. **Environment Variables**

   ```
   VITE_SOCKET_URL=https://your-backend.onrender.com
   ```

4. **Deploy**
   - Click "Deploy"
   - Wait for build to complete

---

## 🎯 User Flows

### Teacher Flow

```
Role Selection → Create Poll → View Live Results →
Stop Poll → View History / Create New Poll
```

### Student Flow

```
Role Selection → Enter Name → Wait for Poll →
Answer Poll → View Results → Wait for Next Poll

OR

Browse Quizzes → Take Quiz → View Results →
See Leaderboard → Retry / Take Another
```

---

## 🐛 Troubleshooting

**WebSocket Not Connecting:**

```bash
# Check VITE_SOCKET_URL in .env
# Verify backend is running
# Check browser console for errors
```

**Messages Not Appearing:**

```bash
# Check browser console for:
# "🔌 Initializing socket in ChatPopup"
# "📤 Sending message: ..."
# "📨 Received chat message: ..."
```

**Poll Notifications Not Showing:**

```bash
# Verify socket connection
# Check if student has joined session
# Look for "poll:new" event in console
```

**Redux State Issues:**

```bash
# Install Redux DevTools extension
# Check state in DevTools
# Verify actions are dispatched
```

---

## 📖 Additional Documentation

- **[Main README](../README.md)** - Project overview
- **[Server README](../server/README.md)** - Backend documentation
- **[WebSocket Guide](../CHAT_WEBSOCKET_GUIDE.md)** - Complete WebSocket guide
- **[Quick Reference](../CHAT_QUICK_REFERENCE.md)** - Quick WebSocket reference
