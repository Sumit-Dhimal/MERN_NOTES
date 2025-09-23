# MERN Frontend Structure 📂
```
frontend/
│── public/ # Static files (index.html, favicon, images)
│ └── index.html # Root HTML template
│
│── src/
│ │── assets/ # Images, fonts, CSS, etc.
│ │
│ │── components/ # Reusable UI components
│ │ ├── Navbar.jsx
│ │ ├── Footer.jsx
│ │ └── Button.jsx
│ │
│ │── pages/ # Main pages (routed)
│ │ ├── Home.jsx
│ │ ├── About.jsx
│ │ ├── Login.jsx
│ │ ├── Register.jsx
│ │ └── Dashboard.jsx
│ │
│ │── layouts/ # Page layouts (e.g. with sidebar, navbar, etc.)
│ │ └── MainLayout.jsx
│ │
│ │── hooks/ # Custom React hooks
│ │ ├── useAuth.js
│ │ └── useFetch.js
│ │
│ │── context/ # React Context API for global state
│ │ └── AuthContext.jsx
│ │
│ │── services/ # API calls (Axios or Fetch wrapper)
│ │ ├── api.js # Axios instance
│ │ ├── authService.js # Login/Register API
│ │ └── userService.js
│ │
│ │── utils/ # Helper functions
│ │ ├── validators.js
│ │ └── formatDate.js
│ │
│ │── routes/ # Centralized routing
│ │ └── AppRoutes.jsx
│ │
│ │── styles/ # Global styles
│ │ ├── index.css
│ │ └── theme.js
│ │
│ │── App.jsx # Main App component
│ │── main.jsx # Entry point (ReactDOM.render)
│ └── index.css # Base styles
│
├── .env # Environment variables (frontend API URL)
├── package.json
└── vite.config.js / webpack.config.js
```
---
# Explanation of Key Parts 📌
* **`public/index.html`** → The root HTML file React injects into.
* **`components/`** → Small, reusable UI parts (buttons, modals, navbar).
* **`pages/`** → Full pages used in routing (Home, Login, Dashboard).
* **`layouts/`** → Structures wrapping pages (e.g., Dashboard with sidebar).
* **`hooks/`** → Custom logic hooks (like `useAuth`, `useFetch`).
* **`context/`** → Global state management with React Context API.
* **`services/`** → All backend API calls handled here (with Axios).
* **`utils/`** → Utility functions/helpers to keep code clean.
* **`routes/`** → Centralized React Router v6 routes.
* **`App.jsx`** → Root component where routing and providers are set.
* **`main.jsx`** → Renders `<App />` into DOM.
---
⚡ This keeps your frontend **modular, scalable, and easy to maintain**.
For bigger projects, you can also integrate **Redux Toolkit** or **TanStack Query**
for state & server-side caching.