# JWT Authentication Demo

A complete Node.js, Express, and MongoDB authentication system demonstrating how to implement JSON Web Tokens (JWT) for secure user authentication and route protection.

> **Reference & Learning Resource:**  
> This project is inspired by and references [The JSON Web Token Handbook – Learn to Use JWTs for Web Authentication](https://www.freecodecamp.org/news/the-json-web-token-handbook-learn-to-use-jwts-for-web-authentication/) by freeCodeCamp.

---

## 📌 Features

- **User Registration**: Secure user signup with password hashing using `bcryptjs`.
- **User Login**: Credentials verification and JWT signing.
- **Route Protection**: Custom middleware (`authMiddleware`) to protect routes by validating Bearer tokens.
- **Protected Profile Endpoint**: Access restricted user details using a valid JWT.
- **MongoDB Integration**: Schema validation and persistence via Mongoose.

---

## 🏗️ Project Architecture

```
jwt-auth-demo/
├── config/
│   └── db.js                 # MongoDB connection configuration
├── controllers/
│   └── authController.js     # Route handlers for register, login, and profile
├── middlewares/
│   └── authMiddleware.js     # JWT verification middleware
├── models/
│   └── User.js               # Mongoose User schema & model
├── routes/
│   └── auth.js               # Authentication routes
├── services/
│   ├── hashService.js        # Password hashing & comparison utilities
│   └── jwtService.js         # JWT generation and verification logic
├── .env                      # Environment variables (ignored by git)
├── .gitignore                # Git ignore rules
├── package.json              # Project dependencies and scripts
├── server.js                 # Express server entry point
└── README.md
```

---

## ⚙️ Prerequisites

- [Node.js](https://nodejs.org/) (v16+ recommended)
- [MongoDB](https://www.mongodb.com/) (running locally or a cloud MongoDB Atlas instance)

---

## 🚀 Getting Started

### 1. Clone & Install Dependencies

```bash
git clone <repository-url>
cd jwt-auth-demo
npm install
```

### 2. Configure Environment Variables

Create a `.env` file in the root directory (do not commit this file to version control):

```env
PORT=5000
MONGO_URI=mongodb://127.0.0.1:27017/jwtauthdemo
JWT_SECRET=your_super_secret_key_here
```

### 3. Run the Server

- **Development mode** (with nodemon):
  ```bash
  npm run dev
  ```

- **Production mode**:
  ```bash
  npm start
  ```

The server will start listening on `http://localhost:5000`.

---

## 📡 API Endpoints

### 1. Health Check
- **GET** `/`
- **Response**: `Hello World! Your server is working 🚀`

---

### 2. Register User
- **POST** `/api/auth/register`
- **Body** (`application/json`):
  ```json
  {
    "name": "John Doe",
    "email": "john@example.com",
    "password": "secretpassword"
  }
  ```
- **Success Response (201 Created)**:
  ```json
  {
    "message": "User registered successfully",
    "user": {
      "id": "64f1a2b3c4d5e6f7a8b9c0d1",
      "name": "John Doe",
      "email": "john@example.com"
    }
  }
  ```

---

### 3. User Login
- **POST** `/api/auth/login`
- **Body** (`application/json`):
  ```json
  {
    "email": "john@example.com",
    "password": "secretpassword"
  }
  ```
- **Success Response (200 OK)**:
  ```json
  {
    "message": "Login successful",
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
  }
  ```

---

### 4. Get Protected Profile
- **GET** `/api/auth/profile`
- **Headers**:
  ```http
  Authorization: Bearer <your_jwt_token>
  ```
- **Success Response (200 OK)**:
  ```json
  {
    "user": {
      "_id": "64f1a2b3c4d5e6f7a8b9c0d1",
      "name": "John Doe",
      "email": "john@example.com"
    }
  }
  ```
- **Unauthorized Response (401 Unauthorized)**:
  ```json
  {
    "message": "Access denied. No token provided."
  }
  ```

---

## 🔑 How JWT Authentication Works

1. **Client Request**: The client sends user credentials (`email` and `password`) to `/api/auth/login`.
2. **Server Verification**: The server verifies the credentials against the database using `bcryptjs`.
3. **Token Creation**: If credentials match, the server signs a JWT containing payload data (like user ID) and sends it back to the client.
4. **Token Storage**: The client stores the token (e.g., in memory, HTTP-only cookie, or local storage).
5. **Authenticated Requests**: For protected routes (`/api/auth/profile`), the client attaches the token in the `Authorization` header (`Bearer <token>`).
6. **Token Verification**: The server's `authMiddleware` validates the signature and expiration before serving the resource.

---

## 📚 References & Further Reading

- [The JSON Web Token Handbook – Learn to Use JWTs for Web Authentication (freeCodeCamp)](https://www.freecodecamp.org/news/the-json-web-token-handbook-learn-to-use-jwts-for-web-authentication/)
- [JWT.io Introduction to JSON Web Tokens](https://jwt.io/introduction)
- [Mongoose Documentation](https://mongoosejs.com/docs/)
- [Express.js Guide](https://expressjs.com/)

