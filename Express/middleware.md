# 📘 Express Middleware Notes

## 🔹 What is Middleware?
- Middleware functions are functions that have access to:
  - **req** (request object)  
  - **res** (response object)  
  - **next** (callback to move to the next middleware)  

- Middleware is used for:
  - Logging
  - Parsing data
  - Authentication
  - Error handling
  - Request/response modifications  

---

## 🔑 Core / Built-in Middleware in Express
1. **express.json()**  
   Parses incoming JSON requests.  
   ```js
   app.use(express.json());
````

Example: For APIs receiving JSON data.

2. **express.urlencoded()**
   Parses form data (URL-encoded).

   ```js
   app.use(express.urlencoded({ extended: true }));
   ```

   Example: Handling HTML form submissions.

3. **express.static()**
   Serves static files (images, CSS, JS).

   ```js
   app.use(express.static("public"));
   ```

---

## 🔑 Commonly Used Third-Party Middleware

1. **morgan** – HTTP request logger

   ```js
   import morgan from 'morgan';
   app.use(morgan('dev'));
   ```

2. **cookie-parser** – Parse cookies

   ```js
   import cookieParser from 'cookie-parser';
   app.use(cookieParser());
   ```

3. **cors** – Enable Cross-Origin Resource Sharing

   ```js
   import cors from 'cors';
   app.use(cors());
   ```

4. **express-session** – Session management

   ```js
   import session from 'express-session';
   app.use(session({ secret: 'mySecret', resave: false, saveUninitialized: true }));
   ```

5. **helmet** – Secure HTTP headers

   ```js
   import helmet from 'helmet';
   app.use(helmet());
   ```

6. **compression** – Response compression

   ```js
   import compression from 'compression';
   app.use(compression());
   ```

---

## 🔑 Custom Middleware Examples

1. **Logger Middleware**

   ```js
   const logger = (req, res, next) => {
     console.log(`${req.method} ${req.url}`);
     next();
   };
   app.use(logger);
   ```

2. **Authentication Middleware**

   ```js
   const protect = (req, res, next) => {
     if (req.user) {
       next();
     } else {
       res.status(401).json({ message: "Unauthorized" });
     }
   };
   ```

3. **Error Handling Middleware**

   ```js
   app.use((err, req, res, next) => {
     console.error(err.stack);
     res.status(500).json({ message: 'Something broke!' });
   });
   ```

---

## 📝 Summary

* **Built-in:** `express.json()`, `express.urlencoded()`, `express.static()`
* **Third-party:** `morgan`, `cookie-parser`, `cors`, `helmet`, `compression`, `express-session`
* **Custom:** Logging, Authentication, Error Handling

```

---

