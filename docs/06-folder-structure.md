# Folder Structure: BookShelf v1

**Source:** docs/03-database-design.md, docs/04-routes.md, docs/05-pages.md
**Stack:** Node.js, Express, MongoDB (Mongoose), EJS
**Date:** 7 Oct 2026

## 1. Tree

```
BookShelf/
├── docs/                           project documents
│   ├── meetings/
│   ├── 00-client-request.md
│   ├── 01-prd.md
│   ├── 02-user-stories.md
│   ├── 03-database-design.md
│   ├── 04-routes.md
│   ├── 05-pages.md
│   └── 06-folder-structure.md
│
├── frontend/                       what the browser gets
│   ├── views/                      EJS templates
│   │   ├── layouts/
│   │   │   └── boilerplate.ejs
│   │   ├── partials/
│   │   │   ├── navbar.ejs
│   │   │   ├── footer.ejs
│   │   │   ├── pagination.ejs
│   │   │   └── formErrors.ejs
│   │   ├── alerts/
│   │   │   ├── flash.ejs
│   │   │   ├── modal.ejs
│   │   │   └── error.ejs
│   │   ├── pages/
│   │   │   └── landing.ejs
│   │   ├── auth/
│   │   │   ├── register.ejs
│   │   │   ├── verifyEmail.ejs
│   │   │   ├── login.ejs
│   │   │   ├── forgotPassword.ejs
│   │   │   └── resetPassword.ejs
│   │   ├── books/
│   │   │   ├── index.ejs           library page
│   │   │   ├── _results.ejs        cards, count, pagination (also sent alone for dynamic search)
│   │   │   ├── _form.ejs           shared by new and edit
│   │   │   ├── new.ejs
│   │   │   ├── edit.ejs
│   │   │   └── show.ejs
│   │   └── notes/
│   │       └── edit.ejs
│   └── public/                     static files
│       ├── css/
│       │   └── styles.css
│       ├── js/
│       │   ├── formValidation.js   client checks, clear error on typing
│       │   ├── library.js          dynamic search and filter
│       │   ├── bookForm.js         status and page fields, cover preview
│       │   ├── otpTimer.js         resend countdown
│       │   ├── flash.js            close button
│       │   ├── modal.js            delete confirmation
│       │   └── navbar.js           hamburger
│       ├── images/
│       │   ├── logo.png
│       │   └── placeholderCover.png
│       └── icons/                  SVG icons
│           ├── eyeOpen.svg
│           ├── eyeClosed.svg
│           ├── cross.svg
│           ├── menu.svg
│           ├── search.svg
│           ├── plus.svg
│           ├── edit.svg
│           ├── trash.svg
│           ├── check.svg
│           ├── warning.svg
│           ├── chevronLeft.svg
│           ├── chevronRight.svg
│           └── google.svg
│
├── backend/                        server logic
│   ├── config/                     setup of outside services
│   │   ├── cloudinary.js           Cloudinary and Multer storage
│   │   ├── passport.js             Google OAuth strategy
│   │   └── mailer.js               Nodemailer transport
│   │   ├── session.js              express-session and connect-mongo setup
│   ├── routes/                     URL to controller mapping only
│   │   ├── pageRoutes.js           /
│   │   ├── authRoutes.js           register, verify, login, logout, forgot, reset, google
│   │   ├── bookRoutes.js           /books and /books/:id
│   │   └── noteRoutes.js           /books/:id/notes
│   ├── controllers/                request in, response out
│   │   ├── pageController.js
│   │   ├── authController.js
│   │   ├── bookController.js
│   │   └── noteController.js
│   ├── services/                   business logic, no req or res
│   │   ├── otpService.js           make, hash, check, expire OTP
│   │   ├── emailService.js         send OTP and reset emails
│   │   ├── tokenService.js         sign and verify JWT, cookie options
│   │   └── bookService.js          status and page rules, search, filter, pagination
│   ├── middleware/
│   │   ├── auth.js                 requireLogin, redirectIfLoggedIn, loadUser
│   │   ├── validate.js             run a Joi schema on body or query
│   │   ├── upload.js               Multer, file type and size limit
│   │   ├── rateLimiters.js         login, verify, resend, forgot, reset
│   │   ├── flash.js                copy flash messages from the session to views
│   │   └── errorHandler.js         404 and error pages
│   ├── schemas/                    Joi schemas, check request input
│   │   ├── authSchema.js           register, login, verify, forgot, reset
│   │   ├── bookSchema.js           book add and edit, query params for /books
│   │   └── noteSchema.js
│   ├── utils/
│   │   ├── catchAsync.js
│   │   ├── ExpressError.js
│   │   ├── pagination.js           build page links and numbers
│   │   └── constants.js            genres, statuses, page size, OTP settings
│   ├── app.js                      builds the Express app
│   └── index.js                    loads env, connects DB, starts server
│
├── database/
│   ├── models/                     Mongoose schemas and models
│   │   ├── user.js
│   │   ├── book.js
│   │   └── note.js
│   ├── seeds/
│   │   └── seedBooks.js            fake data for development
│   └── connect.js                  Mongoose connection
│
├── tests/
│   ├── auth.test.js
│   ├── books.test.js
│   └── notes.test.js
│
├── .env                            secrets, never in git
├── .env.example                    same keys, no values, in git
├── .gitignore
├── package.json                    one package.json at the root
└── README.md
```

## 2. What each main folder does

| Folder | Job |
|---|---|
| `frontend/` | Everything the browser receives: EJS pages, CSS, client JavaScript, images, icons. |
| `backend/` | Everything the server does: routes, controllers, services, middleware, input checks, config. |
| `database/` | Everything about stored data: Mongoose models, connection, seed data. |
| `docs/` | Project documents. |
| `tests/` | Automated tests. |

## 3. Request flow

```
Browser
  → backend/routes/          match URL and method
  → backend/middleware/      rate limit, requireLogin, upload, validate (Joi schema)
  → backend/controllers/     read request, call services and models, choose response
  → backend/services/        business rules
  → database/models/         MongoDB
  → frontend/views/          render EJS, or redirect
```

## 4. Layer rules

- **routes** only connect URL, middleware, and controller. No logic.
- **controllers** handle req and res. Always wrapped in `catchAsync`.
- **services** hold business rules and never touch req or res, so they are easy to test.
- **models** (database) hold Mongoose schema rules, indexes, and hooks (password hash).
- **schemas** (backend) are Joi schemas. They check the shape of request input. Mongoose models check data on save. Neither replaces the other.
- **views** show data only. No database calls in templates.
- **alerts** (flash, modal, error) are shared and included from the layout, not copied into pages.
- Every query on Book and Note includes `owner`.

Two things are both called a schema:
- Joi schema in `backend/schemas/` = checks the request.
- Mongoose schema inside `database/models/` = shape of saved data.

## 5. Naming

- Files: camelCase (`bookController.js`). Models: singular (`book.js`). Routes, controllers, services, schemas: grouped by feature.
- Schema files end with `Schema` (`bookSchema.js`).
- URLs: lowercase, plural for resources (`/books`).
- Constants: UPPER_SNAKE_CASE in `backend/utils/constants.js`.
- Views: partials that are fragments used by one folder start with `_`.
- Icons: camelCase, one meaning per file (`trash.svg`).

## 6. Environment variables (`.env`)

```
NODE_ENV=development
PORT=3000
MONGO_URL=
JWT_SECRET=
SESSION_SECRET=
COOKIE_SECRET=
EMAIL_USER=
EMAIL_PASS=
GOOGLE_CLIENT_ID=
GOOGLE_CLIENT_SECRET=
GOOGLE_CALLBACK_URL=http://localhost:3000/auth/google/callback
CLOUDINARY_CLOUD_NAME=
CLOUDINARY_KEY=
CLOUDINARY_SECRET=
```

`.env.example` has the same names with empty values.

## 7. Packages

**Runtime**
- express, ejs, ejs-mate
- mongoose
- joi
- bcrypt
- jsonwebtoken, cookie-parser
- passport, passport-google-oauth20
- nodemailer
- multer, cloudinary, multer-storage-cloudinary
- method-override
- express-rate-limit
- helmet
- dotenv
- express-session, connect-mongo
- connect-flash

**Dev**
- nodemon
- jest, supertest
- eslint, prettier

Pin versions that must match each other (cloudinary and multer-storage-cloudinary).

## 8. Scripts (`package.json`)

| Script | What |
|---|---|
| `npm run dev` | nodemon backend/index.js |
| `npm start` | node backend/index.js |
| `npm test` | jest |
| `npm run seed` | node database/seeds/seedBooks.js |
| `npm run lint` | eslint . |

## 9. Git

- Branches: `main` (stable), `dev` (integration), feature branches named with the Jira key: `BS-12-add-book`.
- Commit message: `BS-12 add book form validation`.
- Merge feature into `dev` after self review and testing. Merge `dev` into `main` at the end of each sprint.
- Never commit `.env` or `node_modules`.

## 10. Decisions

- Three main folders (`frontend`, `backend`, `database`) so each part is easy to find.
- One `package.json` at the root. Frontend here is server-rendered EJS, so there is nothing to install or build separately.
- Express reads views from `frontend/views` and static files from `frontend/public`. Both paths are set in `backend/app.js` with `path.join(__dirname, ...)`.
- Joi schemas stay in `backend/schemas/` because they check requests. Mongoose schemas stay in `database/models/` with the models.
- `views/alerts/` holds flash, modal, and the error page. 404 and 500 share one `error.ejs`.
- `services/` exists so controllers stay thin and rules (status and pages, OTP, pagination) can be tested alone.
- Login uses JWT in an httpOnly cookie. A server session (`express-session` with `connect-mongo`) is added only for temporary data: flash messages, Google OAuth `state`, and the pending email during verify and reset. Session never holds login state.
- Passport is used only for the Google handshake, with login sessions off (`session: false`). Its OAuth `state` check uses the express-session session. After Google returns, the app issues its own JWT cookie like normal login.
- Cookie settings (JWT cookie and session cookie): `httpOnly`, `sameSite: lax`, `secure` in production. State-changing requests are POST, PUT, or DELETE only. CSRF tokens are a later hardening step.
- Middleware order in `app.js`: `cookie-parser`, `session`, `flash`, then JWT `loadUser`, then routes.
- `books/_results.ejs` is the one template for the list, used by the full page and by dynamic search.
- Icons are SVG files in `frontend/public/icons/`, used with `<img>` in v1.
- Docs live in the same repo so they change with the code.