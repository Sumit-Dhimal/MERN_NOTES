# Web Security Guide for MERN Blog App

This guide provides essential web security practices for building a secure blog application using the MERN stack.

---

## 🔐 Why Web Security Matters

Even a simple blog app can be a target:
- Users can **register & login**
- Users can **write content**
- Admins can **delete/edit posts**
- Attackers may try to steal accounts, inject scripts, access admin routes, or crash the server

Security protects:
- 🔑 User accounts
- 🧠 Database
- 🧱 Server
- 📊 Developer reputation

---

## 🧠 Core Web Security Concepts (MERN Focused)

### 1️⃣ Password Security

- **Never store plain passwords**
- Use **bcrypt** to hash passwords

```js
import bcrypt from "bcrypt";

const hashedPassword = await bcrypt.hash(password, 10);
```

### 2️⃣ Authentication vs Authorization

- **Authentication** → Who are you?
- **Authorization** → What are you allowed to do?

Example:
- Auth: Is user logged in?
- Authorization: Can user edit **their own** post?

Use JWT and middleware in Express.

### 3️⃣ JWT Security

Best practices:
- Store JWT in **HTTP-only cookies**
- Set expiration time
- Use a strong secret

```js
res.cookie("token", jwtToken, {
  httpOnly: true,
  secure: true,
  sameSite: "strict"
});
```

### 4️⃣ Input Validation

- Validate all user input
- Prevent scripts, injections, and broken data

Use `express-validator` and Mongoose schema validation:

```js
body("email").isEmail();
body("password").isLength({ min: 6 });
```

### 5️⃣ XSS (Cross-Site Scripting)

- Avoid rendering untrusted HTML
- Sanitize content using libraries like `dompurify`

### 6️⃣ CSRF (Cross-Site Request Forgery)

- Use CSRF tokens
- Use `sameSite` cookies
- Avoid unsafe GET requests for actions

### 7️⃣ MongoDB Injection

- Always whitelist fields

```js
// Safe
User.findOne({ email: req.body.email });
```

### 8️⃣ Rate Limiting

Protect login and register routes:

```js
import rateLimit from "express-rate-limit";

const limiter = rateLimit({
  windowMs: 15 * 60 * 1000,
  max: 100
});
```

### 9️⃣ Secure Headers

Use **Helmet.js** to automatically add security headers:

```js
import helmet from "helmet";
app.use(helmet());
```

---

## 🔐 Minimum Security Checklist for Your Blog App

✅ Hashed passwords  
✅ JWT authentication  
✅ Role-based access (admin/user)  
✅ Input validation  
✅ XSS protection  
✅ Secure cookies  
✅ Rate limiting  
✅ Proper error handling

---

## 🚀 Learning Security Step-by-Step

1️⃣ Build blog app without security  
2️⃣ Add authentication (JWT)  
3️⃣ Add authorization (admin/user)  
4️⃣ Add validation & sanitization  
5️⃣ Add rate limiting & headers  
6️⃣ Try to break your own app to learn vulnerabilities

---

## 🧩 Next Steps

- Create a **security-first blog architecture**
- Explore **real attack examples** on a blog app
- Design a **secure fold