
# OAuth in MERN – Step-by-Step Guide (Google Example)

This document explains how to implement **OAuth (Social Login)** in a **MERN stack** application using **Passport.js**, **JWT**, and **cookies**.

---

## Big Picture Flow

```
Frontend → Backend → OAuth Provider → Backend → Frontend
```

OAuth is **backend-driven**.  
The frontend never communicates directly with Google/Facebook.

---

## STEP 0: Decide Your OAuth Strategy

- Use **Passport.js** for OAuth verification
- Use **JWT + HTTP-only cookies** (same as normal login)
- OAuth replaces **email/password verification only**

---

## STEP 1: Create OAuth App (Google)

1. Go to **Google Cloud Console**
2. Create a project
3. Enable **Google OAuth API**
4. Create **OAuth Credentials**
5. Set Redirect URI:
   ```
   http://localhost:5000/api/auth/google/callback
   ```

Store credentials in `.env`:
```env
GOOGLE_CLIENT_ID=your_client_id
GOOGLE_CLIENT_SECRET=your_client_secret
```

---

## STEP 2: Install Dependencies

```bash
npm install passport passport-google-oauth20
```

---

## STEP 3: Configure Passport Strategy

**`config/passport.js`**

```js
import passport from "passport";
import { Strategy as GoogleStrategy } from "passport-google-oauth20";
import User from "../models/userModel.js";

passport.use(
  new GoogleStrategy(
    {
      clientID: process.env.GOOGLE_CLIENT_ID,
      clientSecret: process.env.GOOGLE_CLIENT_SECRET,
      callbackURL: "/api/auth/google/callback", // process.env.GOOGLE_CALLBACK_URL
    },
    async (accessToken, refreshToken, profile, done) => {
      try {
        let user = await User.findOne({ googleId: profile.id });

        if (!user) {
          user = await User.create({
            name: profile.displayName,
            email: profile.emails[0].value,
            googleId: profile.id,
            authProvide: "google",
          });
        }

        done(null, user);
      } catch (error) {
        done(error, null);
      }
    }
  )
);
```

---

## STEP 4: Initialize Passport

**`server.js`**

```js
import passport from "passport";
import "./config/passport.js";

app.use(passport.initialize());
```

> No sessions needed if using JWT.

---

## STEP 5: OAuth Routes

### Start OAuth Login

```js
router.get(
  "/google",
  passport.authenticate("google", {
    scope: ["profile", "email"],
  })
);
```

### OAuth Callback

```js
import generateToken from "../utils/generateToken.js";

router.get(
  "/google/callback",
  passport.authenticate("google", {
    session: false,
    failureRedirect: "/login",
  }),
  (req, res) => {
    generateToken(res, req.user._id);
    res.redirect("http://localhost:3000");
  }
);
```

---

## STEP 6: JWT Token Generation

**`utils/generateToken.js`**

```js
import jwt from "jsonwebtoken";

const generateToken = (res, userId) => {
  const token = jwt.sign({ userId }, process.env.JWT_SECRET, {
    expiresIn: "30d",
  });

  res.cookie("jwt", token, {
    httpOnly: true,
    secure: process.env.NODE_ENV === "production",
    sameSite: "strict",
  });
};

export default generateToken;
```

---

## STEP 7: Frontend Login Button

```jsx
<a href="http://localhost:5000/api/auth/google">
  Login with Google
</a>
```

> OAuth must use redirect, not Axios/fetch.

---

## STEP 8: Protect Routes

Use the same `protect` middleware as normal login:

- Read JWT from cookies
- Verify token
- Attach user to `req.user`

OAuth and normal users are treated identically after login.

---

## STEP 9: User Model Example

```js
const userSchema = new mongoose.Schema({
  name: String,
  email: String,
  password: String, // optional for OAuth users
  googleId: String,
  isOAuthUser: Boolean,
});
```

---

## Where Passport.js Fits

Passport.js:
- Verifies OAuth provider
- Returns authenticated user

Passport.js does NOT:
- Generate JWT
- Manage cookies
- Protect routes

---

## OAuth vs Normal Login

| Normal Login | OAuth Login |
|-------------|------------|
| Email + Password | Google/Facebook |
| bcrypt | Provider verifies |
| JWT Cookie | JWT Cookie |
| protect middleware | protect middleware |

---

## Common Mistakes

- Calling OAuth provider from frontend
- Using Axios/fetch for OAuth
- Storing provider access tokens in DB
- Mixing sessions with JWT unnecessarily

---

## Recommended Next Steps

- Add GitHub OAuth
- Implement account linking (same email)
- Handle OAuth-only users (no password)
- Add role-based access control

---

**Author:** MERN OAuth Guide  
