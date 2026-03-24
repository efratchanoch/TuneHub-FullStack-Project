# TuneHub FullStack Project

## Overview
TuneHub is a social music networking platform built as a full-stack application:
- **Backend**: Java Spring Boot (`backend/`)
- **Frontend**: Angular (`frontend/`)

It supports user onboarding, profile management, post creation with media (images/audio/video), search, following, comments, likes, favorites, and role-based access.

---

## Project Architecture

### Backend (Spring Boot)

- Package root: `com.example.tunehub`
- Controllers under: `com.example.tunehub.controller`
- Services under: `com.example.tunehub.service`
- Repositories under: `com.example.tunehub.repository`
- Security: `com.example.tunehub.security` (JWT + roles)
- Data model: `com.example.tunehub.model`
- DTOs and mappers for entity boundary translation
- DB access via Spring Data JPA
- Media management in `backend/ffmpeg` and `FileUtils` routines

### Frontend (Angular)

- Angular app source: `frontend/src/app`
- Components: `frontend/src/app/Components` (Chat, Comments, Notification, Post, SheetMusic, Teachers, Users, etc.)
- Services: `frontend/src/app/Services` (login, user, post, comment, sheetmusic, etc.)
- Models in: `frontend/src/app/Models`
- Routing in: `frontend/src/app/app.routes.ts` and `app.routes.server.ts`
- Server render entry: `frontend/src/main.server.ts`, `frontend/src/server.ts`

---

## Tech Stack

- Java 17+ (Spring Boot)
- Spring Web, Spring Data JPA, Spring Security (JWT)
- Hibernate
- Maven
- PostgreSQL (or H2/other based on config)
- Angular 16+ (TypeScript)
- RxJS
- HTML + CSS
- WebSocket / notification layer (assumed in NotificationService)

---

## API Endpoints Overview

### 1) Authentication + Users (`/api/users`)
- `POST /signUp` — user registration (`multipart/form-data`: image + profile)
- `POST /signIn` — login (returns JWT cookie)
- `POST /signOut` — logout (clears cookie)
- `GET /me` — current user profile
- `GET /userById/{id}`
- `GET /users`, `GET /musicians`, `GET /userByCity/{city}`, etc.
- `PUT /updateCurrentUser` — profile update
- `PUT /update-user-type/{userId}/{newType}`
- `PUT /currentUserTypeToMusician` (self-upgrade)
- `GET /countActive`
- `DELETE /delete/{userId}`
- Chat & AI endpoint: `POST /chat`

### 2) Posts (`/api/post`)
- `GET /posts`, `GET /postById/{id}`, `GET /postsByUserId/{id}`
- `GET /postsByTitle/{title}`, `GET /postsByDate/{date}`
- `GET /newPosts`
- `POST /uploadPost` — with `data` payload and optional media
- `DELETE /deletePostByPostId/{id}` (admin/superadmin)
- Media streaming: `GET /audio/{audioPath}`, `GET /video/{videoPath}`, `GET /image/{imagePath}`

### 3) Comments, Interaction, Favorites, Notifications, Sheets, Teachers, Instruments

- ` /api/comment` controller, including add/list/delete comment flows
- ` /api/interaction` (likes, favorites, follow/unfollow behavior)
- ` /api/notification` (fetch/mark read etc.)
- ` /api/sheetmusic`, `/api/sheetmusiccategory`, `/api/teacher`, `/api/instrument`, `/api/role`
- ` /api/general-search` for global search context

> Note: for full method signatures, refer to each controller under `backend/src/main/java/com/example/tunehub/controller`.

---

## Setup and Run (Development)

### Prerequisites
- Node.js 18+ & npm
- Java 17+ (OpenJDK/Oracle)
- Maven 3.8+
- Git
- Optional: PostgreSQL (or local H2 if configured)

### Backend
1. Open a terminal in `backend/`.
2. Copy or create `src/main/resources/application.properties` with appropriate DB and JWT secrets.
   - Example: `spring.datasource.url=jdbc:postgresql://localhost:5432/tunehub`
   - `spring.datasource.username`, `spring.datasource.password`
3. Build:
   - Windows: `mvnw.cmd clean package`
   - Linux/Mac: `./mvnw clean package`
4. Run:
   - `mvnw.cmd spring-boot:run` or `java -jar target/backend-*[version].jar`
5. Backend default endpoint: `http://localhost:8080`

### Frontend
1. Open a terminal in `frontend/`.
2. Install dependencies:
   - `npm install`
3. Start dev server:
   - `npm start` (or `ng serve` if angular cli available)
4. App URL: `http://localhost:4200`

### CORS / Proxy
- Ensure CORS is enabled in backend security config to allow `http://localhost:4200`.
- Alternatively, configure Angular proxy (`proxy.conf.json`) to forward `/api` to `http://localhost:8080`.

---

## Local Development Workflow

1. Start backend.
2. Start frontend.
3. Register user + login.
4. Create posts (with optional media uploads).
5. Use search/comment/interaction features.
6. Monitor backend logs for errors.

---

## Useful Commands

- Backend tests: `mvnw.cmd test`
- Frontend tests: `npm test`
- Linter: `npm run lint`

---

## Contributing
1. Fork repository.
2. Branch from `main`: `feature/your-feature`.
3. Implement and add tests.
4. Ensure formatting and lint pass.
5. Open PR with description and screenshots.

---

## Notes
- If you switch DB, review entity mappings and migrations.
- If JWT expires, adjust TTL in security config.
- For production, use HTTPS and secure cookie flags.
