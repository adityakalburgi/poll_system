# 🖥️ Intervue Server

Backend server for the Intervue Live Polling & Quiz System. Built with Express, Socket.io, Prisma, and PostgreSQL.

---

## 🚀 Quick Start

### Prerequisites

- Node.js 18+
- PostgreSQL database
- OpenAI API key

### Installation

```bash
# Navigate to server directory
cd server

# Install dependencies
npm install

# Create .env file
cp .env.example .env

# Edit .env with your credentials
DATABASE_URL="postgresql://user:password@localhost:5432/Intervue"
OPENAI_API_KEY="your-openai-api-key"

# Generate Prisma client
npm run prisma:generate

# Run migrations
npm run prisma:migrate

# Start development server
npm run dev
```

Server runs on `http://localhost:3000`

---

## 📁 Project Structure

```
server/
├── prisma/
│   └── schema.prisma          # Database schema
├── src/
│   ├── config/
│   │   └── database.ts        # Database connection
│   ├── routes/
│   │   └── quizRoutes.ts      # Quiz API routes
│   ├── services/
│   │   ├── pollService.ts     # Poll business logic
│   │   ├── studentService.ts  # Student management
│   │   └── quizService.ts     # Quiz & AI logic
│   ├── socket/
│   │   └── pollSocket.ts      # WebSocket handlers
│   ├── types/
│   │   └── index.ts           # TypeScript types
│   └── index.ts               # Server entry point
├── .env                       # Environment variables
└── package.json
```

---

## 🔌 WebSocket Events

### Teacher Events (Emit)

```typescript
// Connect as teacher
socket.emit('teacher:connect');

// Create new poll
socket.emit('teacher:create-poll', {
    question: string,
    options: string[],
    correctAnswer: string,
    timeLimit: number
});

// Stop active poll
socket.emit('teacher:stop-poll', { pollId: string });

// Remove student
socket.emit('teacher:remove-student', { studentId: string });

// Get poll history
socket.emit('teacher:get-history');
```

### Student Events (Emit)

```typescript
// Join session
socket.emit("student:join", {
  name: string,
  sessionId: string,
});

// Submit answer
socket.emit("student:submit-answer", {
  pollId: string,
  optionId: string,
  studentId: string,
});
```

### Broadcast Events (Listen)

```typescript
// New poll created
socket.on("poll:new", (data) => {
  // data.poll: { id, question, options, timeLimit, timeRemaining }
});

// Live results update
socket.on("poll:update", (data) => {
  // data: { pollId, results, timeRemaining, totalResponses }
});

// Poll ended
socket.on("poll:ended", (data) => {
  // data: { pollId, results, correctAnswer }
});

// Participants updated
socket.on("participants:update", (participants) => {
  // participants: Array<{ id, name, sessionId, hasAnswered }>
});

// Student kicked
socket.on("student:kicked", (data) => {
  // data: { studentId }
});

// Chat message
socket.on("chat:message", (message) => {
  // message: { id, sender, text, timestamp, role }
});
```

---

## 🛣️ REST API Endpoints

### Quiz Routes

```typescript
// Generate quiz with AI
POST /api/v1/quiz/generate
Body: {
    topic: string,
    level: 'easy' | 'medium' | 'hard',
    creatorSessionId: string,
    creatorName: string
}
Response: { quiz: Quiz }

// Get all quizzes
GET /api/v1/quiz/all
Response: { quizzes: Quiz[] }

// Get quiz by ID
GET /api/v1/quiz/:quizId
Response: { quiz: Quiz }

// Submit quiz attempt
POST /api/v1/quiz/submit
Body: {
    quizId: string,
    sessionId: string,
    studentName: string,
    answers: Record<string, string>,
    score: number
}
Response: { attempt: Attempt }

// Get student attempts
GET /api/v1/quiz/student/:sessionId/attempts
Response: { attempts: Attempt[] }
```

---

## 🗄️ Database Schema

### Poll

```prisma
model Poll {
  id            String     @id @default(uuid())
  question      String
  correctAnswer String
  status        String     @default("pending")
  timeLimit     Int        @default(60)
  createdAt     DateTime   @default(now())
  endedAt       DateTime?
  options       Option[]
  responses     Response[]
}
```

### Student

```prisma
model Student {
  id        String     @id @default(uuid())
  name      String
  sessionId String     @unique
  joinedAt  DateTime   @default(now())
  isKicked  Boolean    @default(false)
  responses Response[]
  attempts  Attempt[]
}
```

### Quiz

```prisma
model Quiz {
  id          String     @id @default(uuid())
  title       String
  topic       String
  level       String
  createdBy   String
  creatorName String
  createdAt   DateTime   @default(now())
  questions   Question[]
  attempts    Attempt[]
}
```

### Question

```prisma
model Question {
  id            String  @id @default(uuid())
  quizId        String
  question      String
  options       String[]
  correctAnswer String
  order         Int
  quiz          Quiz    @relation(fields: [quizId], references: [id])
}
```

### Attempt

```prisma
model Attempt {
  id             String   @id @default(uuid())
  quizId         String
  studentId      String
  studentName    String
  score          Float
  totalQuestions Int
  completedAt    DateTime @default(now())
  quiz           Quiz     @relation(fields: [quizId], references: [id])
  student        Student  @relation(fields: [studentId], references: [id])
}
```

---

## 🤖 AI Quiz Generation

Uses OpenAI GPT-4 to generate quizzes:

```typescript
// Example prompt
const prompt = `Generate 10 multiple choice questions about ${topic} 
at ${level} difficulty level. Return as JSON array with:
- question: string
- options: string[] (4 options)
- correctAnswer: string`;

// Response format
[
  {
    question: "What is React?",
    options: ["Library", "Framework", "Language", "Tool"],
    correctAnswer: "Library",
  },
  // ... 9 more questions
];
```

---

## 🔧 Environment Variables

```bash
# Database
DATABASE_URL="postgresql://user:password@host:5432/database"

# OpenAI (for quiz generation)
OPENAI_API_KEY="sk-..."

# Server
PORT=3000
NODE_ENV="development"

# CORS (optional)
CLIENT_URL="http://localhost:5173"
```

---

## 🛠️ Available Scripts

```bash
# Development
npm run dev          # Start with tsx watch

# Build
npm run build        # Compile TypeScript

# Production
npm start            # Run compiled code

# Database
npm run prisma:generate  # Generate Prisma client
npm run prisma:migrate   # Run migrations
npm run prisma:studio    # Open Prisma Studio
```

---

## 🚢 Deployment

### Render Deployment

1. **Create Web Service**

   - Connect GitHub repository
   - Root Directory: `server`
   - Build Command: `npm install && npm run build`
   - Start Command: `npm start`

2. **Add Environment Variables**

   - `DATABASE_URL`
   - `OPENAI_API_KEY`
   - `NODE_ENV=production`

3. **Create PostgreSQL Database**
   - Add PostgreSQL service
   - Copy Internal Database URL
   - Add as `DATABASE_URL`

---

## 🔍 Key Features

### Real-time Polling

- WebSocket-based live updates
- Automatic timer management
- Smart auto-close logic
- Participant tracking

### AI Quiz Generation

- OpenAI GPT-4 integration
- Configurable difficulty
- Instant generation
- Structured JSON output

### Chat System

- Real-time messaging
- Broadcast to all clients
- Role-based identification
- Message history

### Student Management

- Session-based tracking
- Kick functionality
- Answer status tracking
- Unique session IDs

---

## 🐛 Troubleshooting

**Database Connection Error:**

```bash
# Check DATABASE_URL format
postgresql://USER:PASSWORD@HOST:PORT/DATABASE

# Test connection
npm run prisma:studio
```

**WebSocket Not Working:**

```bash
# Check CORS configuration in src/index.ts
# Ensure client URL is allowed
```

**AI Quiz Generation Failing:**

```bash
# Verify OPENAI_API_KEY is set
# Check API quota/limits
# Review error logs
```
