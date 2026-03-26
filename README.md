# Dev Collab

Dev Collab is a full-stack collaborative development workspace where teams can manage projects, track tasks, chat in real time, share code snippets, view activity history, and use AI assistance for code generation and explanation.

## Tech Stack

- Frontend: Next.js 16, React 19, TypeScript, Tailwind CSS, Axios, Socket.IO Client
- Backend: Node.js, Express 5, TypeScript, MongoDB (Mongoose), Socket.IO
- Auth & Security: JWT, bcrypt password hashing, CORS, Helmet, rate limiting, Zod validation
- AI: Groq chat completion API (multi-model fallback)

## Core Features

### 1) Authentication & User Profile

- Sign up with name, email, and password
- Login with email and password
- JWT-based protected APIs and socket authentication
- Fetch current user profile (`/auth/me`)
- Update profile name/email (`PATCH /auth/me`)
- Email uniqueness checks on registration and profile updates

### 2) Project Workspace

- Create projects with name and description
- List projects where current user is a member
- Open project details by ID
- Project lifecycle status:
	- `active`
	- `completed`
- Owner/admin can close (complete) a project
- Tracks completion metadata (`completedAt`, `completedBy`)

### 3) Role-Based Collaboration

Project roles:

- `owner`
- `admin`
- `member`
- `none`

Permissions implemented:

- Invite members: owner/admin
- Change member role: owner only
- Create tasks: any member (owner/admin/member)
- Update/delete tasks: owner/admin only
- Create snippets: owner/admin only
- Complete project: owner/admin only

### 4) Member Discovery & Invitations

- Search users by name or email (`/users/search`)
- Invite users to a project with role assignment (`admin` or `member`)
- Prevent duplicate membership additions
- Invited users receive notifications

### 5) Task Management (Kanban)

- Create tasks with:
	- title
	- description
	- optional assignee
	- optional due date (API supported)
- View tasks per project
- Update task fields and status (`todo`, `in-progress`, `done`)
- Delete tasks
- Drag-and-drop Kanban board UI for status movement
- Assignment triggers user notifications

### 6) Real-Time Project Chat (Socket.IO)

- Join per-project rooms
- Send real-time chat messages
- Live message stream updates for all project members
- Typing indicator events
- Presence tracking (`presence:update`) with online member count
- Edit previously sent messages with constraints:
	- Only original sender can edit
	- Edit window is 15 minutes from message creation
- Chat errors surfaced to client (`chat:error`)

### 7) Snippet Sharing

- Save code snippets by language inside projects
- Syntax-highlighted snippet rendering
- Copy snippet to clipboard
- Snippets are linked to creator and project

### 8) AI Coding Assistant

- Generate code from natural-language prompt (`/ai/generate`)
- Explain existing code snippets (`/ai/explain`)
- Language-aware requests (UI includes TypeScript, JavaScript, Python, Java)
- Server-side model fallback strategy across multiple Groq models
- Graceful error responses for missing API key/model/API failures

### 9) Notifications

- In-app notification feed per user
- Unread count support
- Mark notification as read
- Notification sources include:
	- project invitation
	- task assignment
	- new chat message in shared project

### 10) Activity Timeline & Audit Trail

- Project-scoped activity logging for key events:
	- project created/completed
	- member invited
	- member role changed
	- task created/updated/deleted
	- chat message sent/updated
	- snippet shared
- Timeline panel shows latest project activity with relative timestamps
- API returns up to 100 most recent activities per project

### 11) UX Flow

- Root route auto-redirects:
	- authenticated users -> dashboard
	- unauthenticated users -> login
- Dashboard includes:
	- project creation form
	- project list with status badges
	- notifications dropdown
	- logout
- Project page includes:
	- member invite + role management
	- task creation + Kanban board
	- real-time chat panel
	- snippet + AI tools panel
	- activity timeline panel

## API Modules

Base server prefix: `/api`

- Auth: `/auth`
- Projects: `/projects`
- Tasks: `/tasks`
- Chat history: `/chat`
- Snippets: `/snippets`
- Notifications: `/notifications`
- Users: `/users`
- Activity: `/activity`
- AI: `/ai`
- Health: `/health`

## Security & Reliability

- Password hashing with bcrypt
- JWT verification for REST and sockets
- Helmet security headers
- CORS restricted to configured client URL
- Request logging via Morgan
- JSON request body size limit (`1mb`)
- Global rate limiting (300 requests / 15 minutes)
- Input validation using Zod schemas
- Centralized Express error handler

## Project Structure

- `client/` - Next.js frontend
- `server/` - Express + Socket.IO backend

## Environment Variables

### Server (`server/.env`)

- `PORT` (default `5000`)
- `MONGO_URI` (required)
- `JWT_SECRET` (required)
- `CLIENT_URL` (default `http://localhost:3000`)
- `GROQ_API_KEY` (required for AI endpoints)

### Client (`client/.env.local`)

- `NEXT_PUBLIC_API_URL` (default `http://localhost:5000/api`)
- `NEXT_PUBLIC_SOCKET_URL` (default `http://localhost:5000`)

## Run Locally

### 1) Install dependencies

```bash
cd client
npm install

cd ../server
npm install
```

### 2) Configure environment

- Add server environment variables in `server/.env`
- (Optional) Add client environment variables in `client/.env.local`

### 3) Start backend

```bash
cd server
npm run dev
```

### 4) Start frontend

```bash
cd client
npm run dev
```

Client: `http://localhost:3000`  
Server API: `http://localhost:5000/api`

## Production Scripts

### Server

- Build: `npm run build`
- Start: `npm start`

### Client

- Build: `npm run build`
- Start: `npm start`
