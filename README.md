# real-time-quiz-system

![diagrams](real-time-quiz-system.png)

## 1. System Overview

## 1. System Overview

### 1. Client (React Native)
- **Role**: The React Native app serves as the user-facing application to interact with the system (joining quizzes, submitting answers, viewing the leaderboard).
- **Communication**: Uses REST API for login, joining quizzes, retrieving quiz information, leaderboard, etc.; uses WebSocket for real-time updates on scores and leaderboard.

### 2. WebSocket Server (NestJS)
- **Role**: Manages WebSocket connections between the server and clients, providing real-time updates on scores and leaderboard.
- **How It Works**: Each time a user joins or submits an answer, the server sends updates via WebSocket to all users in the quiz session. When the leaderboard changes, the server also sends updates to all users via WebSocket.

### 3. Backend (NestJS)
- **Role**: The main server responsible for handling quiz logic, scoring, authentication, and database queries.
- **Components**:
  - **Authentication Module**: Authenticates users, handling requests related to registration, login, and user information.
  - **Quiz Module**: Handles requests related to joining quizzes, submitting answers, score calculation, and leaderboard requests.
  - **Leaderboard Module**: Handles requests related to the leaderboard.
  - **Prisma Module**: Provides Prisma ORM service for interacting with the database.

### 4. Database (MongoDB)
- **Role**: Stores quiz sessions, user scores, and related user data.
- **Stored Data**:
  - **Users**: Stores user information (ID, name, score, etc.).
  - **Quiz**: Stores quiz information (questions, answers, correct answers, etc.).
  - **Quiz Participant**: Stores quiz session information, participants, and quiz status.

## 2. Data Flow

### Step 1: User Registration/Login
- When the user enters a username and password in the React Native app, it sends a request via API to the backend.
- The backend checks for the username (unique); if it exists, it validates the password. If not, it creates and stores the user information in MongoDB.
- Depending on the condition, the server will return either an error or the user information along with an access token.

### Step 2: User Joins Quiz
- When the user enters a quiz ID in the React Native app, the app sends a join request via API to the backend.
- The backend verifies the quiz ID, then stores the user information in the corresponding quiz session in MongoDB.
- The server sends a confirmation of successful join via WebSocket and updates the participant list for all users in the session.

### Step 3: User Submits an Answer
- When the user submits an answer, the app sends the answer details via API to the backend.
- The backend calculates the score and updates it in MongoDB.
- After updating the score, the backend sends the new score via WebSocket to all users in the quiz session.

### Step 4: Leaderboard Update
- Whenever there is a score change, the backend recalculates and sorts the leaderboard.
- The updated leaderboard is sent via WebSocket to all users in the quiz session for instant result display.

## 3. Technologies and Tools

## Technologies and Tools

### 1. React Native
- **Purpose**: React Native is used to build a cross-platform mobile application for iOS and Android, ensuring a consistent user experience.
- **Justification**: React Native allows for rapid development with reusable components, and it supports real-time updates via WebSocket, which is essential for the live quiz feature. Its large ecosystem and community support also make it a robust choice for maintaining and expanding app functionality.

### 2. MongoDB
- **Purpose**: MongoDB serves as the main database, storing user information, quiz details, session data, and scores.
- **Justification**: MongoDB is a NoSQL database that provides flexibility in data modeling, allowing for easy adjustments to the structure of quiz sessions, participants, and scoring data as requirements evolve. Its scalability also supports the app as it grows, handling both structured and unstructured data effectively, making it suitable for real-time applications.

### 3. Prisma
- **Purpose**: Prisma acts as an ORM (Object-Relational Mapping) tool to interact with the database.
- **Justification**: Prisma simplifies database access in NestJS by providing a type-safe and easy-to-use API for CRUD operations. Its compatibility with MongoDB ensures streamlined interactions with data, which enhances maintainability and reduces error risks, especially as the project scales. Prisma's support for query optimization also improves performance in data-intensive operations, such as leaderboard calculations.

### 4. NestJS
- **Purpose**: NestJS is the backend framework responsible for handling API requests, managing business logic, and facilitating WebSocket connections.
- **Justification**: NestJS is a highly modular, TypeScript-compatible framework that supports scalable server-side applications. It is well-suited for building RESTful APIs, handling WebSocket connections, and integrating with Prisma, providing a cohesive ecosystem for the entire backend infrastructure. NestJS's modular architecture also aids in separating concerns, such as authentication, quiz management, and leaderboard updates, which is ideal for the layered logic in the quiz app.

### 5. Redis
- **Purpose**: Redis is used for caching frequently accessed data and as a Pub/Sub (Publish/Subscribe) system to manage real-time data.
- **Justification**: Redis's in-memory data store provides high-speed caching, which reduces database load and improves response times for frequently requested data like leaderboard information. Redis Pub/Sub enables real-time data flow, making it ideal for broadcasting quiz updates to users and ensuring low-latency messaging, which is essential for quick score updates and accurate leaderboard displays.

### 6. WebSocket
- **Purpose**: WebSocket enables bidirectional, low-latency communication between the client (React Native app) and the server.
- **Justification**: WebSocket is essential for real-time communication in the quiz app, allowing instantaneous updates on scores and leaderboard positions without the need for frequent HTTP requests. This protocol is particularly suitable for interactive applications like live quizzes, as it provides a consistent connection, reducing latency and improving user experience during real-time data updates.

### 7. Docker
- **Purpose**: Docker is used to containerize the MongoDB database and potentially other services, making the deployment and management process more efficient.
- **Justification**: Docker ensures consistent environments across development, staging, and production by containerizing each component. This approach simplifies scaling and provides an easy setup for deploying MongoDB and other services, leading to more reliable integration and quicker iteration cycles during development.
