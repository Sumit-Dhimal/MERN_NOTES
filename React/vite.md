# **Vite Proxy Notes**

### **Purpose**

The Vite proxy allows your frontend development server (e.g., `localhost:3000`) to forward API requests to a backend running on a different port (e.g., `localhost:5000`) without running into **CORS issues**.

---

### **How it works**

1. Frontend makes a request to a path that matches the proxy rule (e.g., `/api/users/register`).
2. Vite intercepts the request and forwards it to the **target backend** (e.g., `http://localhost:5000`).
3. Backend handles the request and sends back a response.
4. Frontend receives the response as if it came from the same origin (`localhost:3000`).

---

### **Basic configuration**

`vite.config.js`:

```js
import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react';

export default defineConfig({
  plugins: [react()],
  server: {
    port: 3000, // Frontend dev server port
    proxy: {
      '/api': {              // Match paths starting with /api
        target: 'http://localhost:5000',  // Backend server
        changeOrigin: true,                // Fix CORS issues
        // optional: rewrite path if backend has different route prefix
        // rewrite: (path) => path.replace(/^\/api/, '')
      }
    }
  }
});
```

---

### **Example**

Frontend Axios call:

```js
axios.post('/api/users/register', { username, email, password });
```

* Without proxy → Browser calls `localhost:3000/api/users/register` → 404.
* With proxy → Vite forwards to `http://localhost:5000/api/users/register` → Backend responds.

---

### **Key Points**

* `/api` is just the **match rule**; it does not exist as a real server path.
* `target` is the **actual backend URL**.
* `changeOrigin: true` helps avoid some CORS or origin issues.
* Optional `rewrite` lets you remove or modify path prefixes if backend routes differ.
* Useful only in **development**; in production, frontend usually calls backend via full URL.

---

### **Summary**

Vite proxy = a **bridge between frontend dev server and backend API** → avoids CORS issues and allows using relative paths (`/api/...`) in Axios requests.

---

 
