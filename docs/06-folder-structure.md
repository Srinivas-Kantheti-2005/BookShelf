# Folder Structure: BookShelf v1

**Source:** docs/01-prd.md, docs/03-database-design.md, docs/04-routes.md, docs/05-pages.md, docs/08-design-system.md
**Stack:** Node.js, Express, MongoDB (Mongoose), EJS
**Date:** 16 Oct 2026

## 1. Tree

```
BookShelf/
├── docs/                               project documents
│   ├── meetings/
│   ├── 00-client-request.md
│   ├── 01-prd.md
│   ├── 02-user-stories.md
│   ├── 03-database-design.md
│   ├── 04-routes.md
│   ├── 05-pages.md
│   ├── 06-folder-structure.md
│   ├── 07-design-brief.md
│   └── 08-design-system.md
│
├── frontend/                           what the browser gets
│   ├── views/                          EJS templates
│   │   ├── layouts/
│   │   │   └── boilerplate.ejs         head, skip link, navbar, flash, main, footer
│   │   ├── partials/                   one partial per design component
│   │   │   ├── navbar.ejs              visitor and logged-in versions, mobile menu
│   │   │   ├── footer.ejs
│   │   │   ├── pageHeader.ejs
│   │   │   ├── pagination.ejs
│   │   │   ├── formErrors.ejs
│   │   │   ├── passwordField.ejs       password input with the show and hide eye
│   │   │   ├── codeInput.ejs           the six box code input
│   │   │   ├── avatar.ejs              image or first letter circle
│   │   │   ├── bookCard.ejs
│   │   │   ├── noteCard.ejs
│   │   │   ├── statusPill.ejs
│   │   │   ├── progressBar.ejs
│   │   │   └── emptyState.ejs
│   │   ├── alerts/
│   │   │   ├── flash.ejs
│   │   │   ├── modal.ejs               confirm dialog
│   │   │   └── error.ejs               404 and 500, one template
│   │   ├── pages/
│   │   │   └── landing.ejs
│   │   ├── auth/
│   │   │   ├── register.ejs
│   │   │   ├── verifyEmail.ejs
│   │   │   ├── login.ejs
│   │   │   ├── forgotPassword.ejs
│   │   │   └── resetPassword.ejs
│   │   ├── books/
│   │   │   ├── index.ejs               library page
│   │   │   ├── _results.ejs            count, cards, pagination (also sent alone for dynamic search)
│   │   │   ├── _form.ejs               shared by new and edit
│   │   │   ├── _noteForm.ejs           add note form on the book page
│   │   │   ├── new.ejs
│   │   │   ├── edit.ejs
│   │   │   └── show.ejs
│   │   ├── notes/
│   │   │   └── edit.ejs
│   │   └── profile/
│   │       ├── show.ejs
│   │       ├── edit.ejs
│   │       └── password.ejs
│   └── public/                         static files, served as they are
│       ├── css/                        load in this order
│       │   ├── tokens.css              the only file with raw values: colors, fonts, spacing, radius, shadows
│       │   ├── base.css                reset, body, headings, links, focus ring
│       │   ├── components.css          buttons, inputs, cards, alerts, pills, modal, pagination
│       │   └── pages.css               page level layouts, mobile first with 600 and 900 px queries
│       ├── js/
│       │   ├── formValidation.js       field checks, errors clear on typing, password eye
│       │   ├── navbar.js               mobile menu
│       │   ├── flash.js                close button
│       │   ├── modal.js                confirm dialog
│       │   ├── library.js              dynamic search and filters
│       │   ├── bookForm.js             status and pages rules, counter, cover preview
│       │   ├── otpInput.js             six box code input
│       │   ├── otpTimer.js             resend countdown
│       │   └── avatarUpload.js         photo check, preview, live first letter
│       ├── images/
│       │   ├── logo.svg
│       │   ├── favicon.svg
│       │   └── empty-books.svg         empty state art
│       └── icons/                      20 SVG icons, stroke set to currentColor
│           ├── eyeOpen.svg      eyeClosed.svg    cross.svg        menu.svg        search.svg
│           ├── plus.svg         edit.svg         trash.svg        check.svg       verified.svg
│           ├── info.svg         warning.svg      error.svg        chevronLeft.svg chevronRight.svg
│           └── chevronDown.svg  book.svg         upload.svg       spinner.svg     google.svg
│
├── backend/                            server logic
│   ├── config/                         setup of outside services
│   │   ├── env.js                      reads .env and stops the app if a key is missing
│   │   ├── cloudinary.js               Cloudinary client
│   │   ├── passport.js                 Google OAuth strategy
│   │   ├── mailer.js                   Nodemailer transport
│   │   └── session.js                  express-session and connect-mongo
│   ├── routes/                         URL to controller mapping only
│   │   ├── pageRoutes.js               /
│   │   ├── authRoutes.js               register, verify, login, logout, forgot, reset, google
│   │   ├── bookRoutes.js               /books and /books/:id
│   │   ├── noteRoutes.js               /books/:id/notes
│   │   └── profileRoutes.js            /profile, /profile/edit, /profile/password
│   ├── controllers/                    request in, response out
│   │   ├── pageController.js
│   │   ├── authController.js
│   │   ├── bookController.js
│   │   ├── noteController.js
│   │   └── profileController.js
│   ├── services/                       business rules, no req or res
│   │   ├── userService.js              create, find, link Google, generated username
│   │   ├── passwordService.js          hash, compare, password rule
│   │   ├── otpService.js               make, hash, check, expire, attempts, resend wait
│   │   ├── emailService.js             sends the code and notice emails
│   │   ├── tokenService.js             sign and verify JWT, cookie options
│   │   ├── imageService.js             covers and avatars: upload, copy from a URL, delete
│   │   ├── bookService.js              status and page rules, search, filter, pagination
│   │   ├── noteService.js              note page check against the book
│   │   └── profileService.js           library numbers for the profile
│   ├── middleware/
│   │   ├── locals.js                   puts user, flash, current path, and icons into views
│   │   ├── auth.js                     loadUser, requireLogin, requireGuest, requirePending
│   │   ├── validate.js                 runs a Joi schema on body, query, or params
│   │   ├── upload.js                   uploadCover, uploadAvatar, removes the file if validation fails
│   │   ├── rateLimiters.js             register, login, verify, resend, forgot, reset, change password
│   │   └── errorHandler.js             404 and 500 pages
│   ├── schemas/                        Joi schemas, check request input
│   │   ├── authSchema.js               register, login, verify, forgot, reset
│   │   ├── bookSchema.js               book add and edit, library query
│   │   ├── noteSchema.js
│   │   └── profileSchema.js            username, change password
│   ├── emails/                         email templates
│   │   ├── verifyCode.ejs
│   │   ├── resetCode.ejs
│   │   ├── googleLoginNotice.ejs
│   │   └── passwordChanged.ejs
│   ├── jobs/
│   │   ├── index.js                    starts the scheduled jobs
│   │   └── cleanupUnverified.js        daily, deletes unverified accounts older than 24 hours
│   ├── utils/
│   │   ├── catchAsync.js
│   │   ├── ExpressError.js
│   │   ├── constants.js                genres, statuses, page size, code settings, limits
│   │   ├── pagination.js               page numbers and links for the bar
│   │   ├── format.js                   dates and percent
│   │   ├── escapeRegex.js              makes search text safe
│   │   └── icons.js                    reads the SVG files once, gives views an icon helper
│   ├── app.js                          builds the Express app
│   └── index.js                        loads env, connects the DB, starts jobs and the server
│
├── database/
│   ├── models/                         Mongoose schemas, indexes, and hooks
│   │   ├── user.js
│   │   ├── book.js
│   │   └── note.js
│   ├── seeds/
│   │   └── seedBooks.js                fake data for development
│   └── connect.js                      Mongoose connection
│
├── tests/
│   ├── setup.js                        in-memory MongoDB, mocks for Cloudinary and email
│   ├── auth.test.js
│   ├── library.test.js                 search, filters, pagination
│   ├── books.test.js
│   ├── notes.test.js
│   ├── profile.test.js
│   └── jobs.test.js
│
├── .env                                secrets, never in git
├── .env.example                        same keys, no values, in git
├── .gitignore
├── .prettierrc
├── eslint.config.js
├── jest.config.js
├── package.json                        one package.json at the root
└── README.md
```

## 2. What each main folder does

| Folder | Job |
|---|---|
| `frontend/` | Everything the browser receives: EJS pages, CSS, client JavaScript, images, icons |
| `backend/` | Everything the server does: routes, controllers, services, middleware, input checks, emails, jobs, config |
| `database/` | Everything about stored data: Mongoose models, connection, seed data |
| `tests/` | Automated tests |
| `docs/` | Project documents |

## 3. Request flow

```
Browser
  → backend/routes/            match URL and method
  → backend/middleware/        rate limit, loadUser, requireLogin, upload, validate (Joi schema)
  → backend/controllers/       read the request, call services, choose the response
  → backend/services/          business rules
  → database/models/           MongoDB
  → frontend/views/            render EJS, or redirect with a flash message
```

- `GET /books` with the header `X-Requested-With: XMLHttpRequest` renders only `books/_results.ejs`. Without the header it renders `books/index.ejs`, which includes `_results.ejs`.
- Emails: `emailService` renders a template from `backend/emails/` and sends it through `config/mailer.js`.
- Images: `middleware/upload.js` receives the file, and `imageService` talks to Cloudinary. Controllers never call Cloudinary directly.

## 4. Layer rules

- **routes** only connect URL, middleware, and controller. No logic.
- **controllers** handle `req` and `res`, and are wrapped in `catchAsync`. They call services and never write database queries themselves.
- **services** hold the business rules and never touch `req` or `res`, so they are easy to test.
- **models** hold the Mongoose schema rules, indexes, and hooks (password hash).
- **schemas** are Joi schemas. They check the shape of request input. Mongoose models check data on save. Neither replaces the other.
- **views** show data only. No database calls in templates. Partials match the design components, so a component is built once.
- **jobs** call services and are started from `index.js`.
- Every query on Book and Note includes `owner`.
- Two things are called a schema. A Joi schema in `backend/schemas/` checks the request. A Mongoose schema in `database/models/` is the shape of saved data.

## 5. Where each feature lives

| Epic | Routes and controller | Services | Views | Client JS |
|---|---|---|---|---|
| Landing page | `pageRoutes`, `pageController` | | `pages/landing` | `navbar.js` |
| Register and verify email | `authRoutes`, `authController` | `userService`, `passwordService`, `otpService`, `emailService`, `imageService` | `auth/register`, `auth/verifyEmail`, `partials/codeInput`, `partials/passwordField` | `avatarUpload`, `otpInput`, `otpTimer`, `formValidation` |
| Remove unverified accounts | `jobs/cleanupUnverified` | `userService`, `imageService` | | |
| Login and session | `authRoutes`, `authController`, `middleware/auth` | `tokenService`, `passwordService`, `userService` | `auth/login` | `formValidation` |
| Forgot and reset password | `authRoutes`, `authController` | `otpService`, `emailService`, `passwordService` | `auth/forgotPassword`, `auth/resetPassword` | `otpInput`, `otpTimer`, `formValidation` |
| Google login | `authRoutes`, `authController`, `config/passport` | `userService`, `imageService` | buttons on login and register | |
| Library | `bookRoutes`, `bookController` | `bookService` | `books/index`, `books/_results`, `partials/pagination`, `bookCard`, `statusPill`, `progressBar`, `emptyState` | `library.js` |
| Books and cover | `bookRoutes`, `bookController` | `bookService`, `imageService` | `books/new`, `edit`, `show`, `_form`, `alerts/modal` | `bookForm`, `modal` |
| Notes | `noteRoutes`, `noteController` | `noteService` | `notes/edit`, `books/_noteForm`, `partials/noteCard` | `formValidation`, `modal` |
| Profile | `profileRoutes`, `profileController` | `profileService`, `imageService`, `passwordService`, `tokenService`, `emailService` | `profile/show`, `edit`, `password`, `partials/avatar` | `avatarUpload`, `formValidation` |
| Feedback and validation | `middleware/errorHandler`, `middleware/validate` | | `alerts/flash`, `alerts/error`, `partials/formErrors` | `flash`, `formValidation`, `modal` |

## 6. Naming

- Files: camelCase (`bookController.js`). Models: singular (`book.js`). Routes, controllers, services, schemas, and tests are grouped by feature.
- Schema files end with `Schema`, tests end with `.test.js`, and email templates are named for what they send.
- Partials that are fragments used by one folder start with `_`.
- URLs: lowercase, plural for resources (`/books`).
- Constants: UPPER_SNAKE_CASE in `backend/utils/constants.js`.
- CSS: class names in lowercase with dashes. Colors and sizes come from the variables in `tokens.css`, never raw values.
- Icons: camelCase, one meaning per file (`trash.svg`).

## 7. Environment variables (`.env`)

```
NODE_ENV=development
PORT=3000
MONGO_URL=
JWT_SECRET=
SESSION_SECRET=
EMAIL_USER=
EMAIL_PASS=
EMAIL_FROM=
GOOGLE_CLIENT_ID=
GOOGLE_CLIENT_SECRET=
GOOGLE_CALLBACK_URL=http://localhost:3000/auth/google/callback
CLOUDINARY_CLOUD_NAME=
CLOUDINARY_KEY=
CLOUDINARY_SECRET=
```

`.env.example` has the same names with empty values. `config/env.js` stops the app at start if a key is missing.

## 8. Packages

**Runtime**
- express, ejs, ejs-mate
- mongoose
- joi
- bcrypt
- jsonwebtoken, cookie-parser
- express-session, connect-mongo, connect-flash
- passport, passport-google-oauth20
- nodemailer
- multer, cloudinary, multer-storage-cloudinary
- method-override
- express-rate-limit
- helmet
- node-cron
- dotenv

**Dev**
- nodemon
- jest, supertest, mongodb-memory-server
- eslint, prettier

Pin versions that must match each other (cloudinary and multer-storage-cloudinary).

## 9. Scripts (`package.json`)

| Script | What |
|---|---|
| `npm run dev` | nodemon backend/index.js |
| `npm start` | node backend/index.js |
| `npm test` | jest |
| `npm run seed` | node database/seeds/seedBooks.js |
| `npm run lint` | eslint . |
| `npm run format` | prettier --write . |

## 10. Git

- Branches: `main` (stable), `dev` (integration), and feature branches named with the Jira key: `BS-12-add-book`.
- Commit message: `BS-12 add book form validation`.
- Merge a feature into `dev` after self review and tests. Merge `dev` into `main` at the end of each sprint.
- Never commit `.env` or `node_modules`.

## 11. Decisions

- Three main folders (`frontend`, `backend`, `database`) so each part is easy to find. One `package.json` at the root, because the frontend is server-rendered EJS and has nothing to build.
- `backend/app.js` sets the views folder to `frontend/views` and serves `frontend/public`, both with `path.join(__dirname, ...)`.
- Middleware order in `app.js`: `helmet`, static files, body parsers, `method-override`, `cookie-parser`, session, `locals` and flash, `loadUser`, routes, 404, `errorHandler`. Static files come before the session, so assets do not create session cookies.
- Joi schemas stay in `backend/schemas/` because they check requests. Mongoose schemas stay in `database/models/`.
- `imageService` handles covers and avatars, so Cloudinary code lives in one place. `userService` holds the account rules (Google linking, the generated username), so controllers stay thin.
- Icons are SVG files that `utils/icons.js` reads once and puts inline in the page. Inline SVG lets CSS color them (`currentColor`), which an `<img>` tag cannot.
- Email templates are EJS files in `backend/emails/`, kept apart from the page views.
- CSS is split in four files loaded in order: tokens, base, components, pages. Raw hex values exist only in `tokens.css`.
- Flash messages, the Google `state` check, and the pending email use the session (`express-session` with `connect-mongo`). Login stays JWT. Passport is used for the Google handshake only, with login sessions off (`session: false`). Afterwards the app issues its own JWT cookie.
- Cookie settings (JWT and session cookies): `httpOnly`, `sameSite: lax`, `secure` in production. CSRF tokens are a later hardening step.
- `books/_results.ejs` is the one template for the list, used by the full page and by dynamic search.
- Jobs start from `index.js`, after the database connects, with `node-cron`.
- Tests run on an in-memory MongoDB. Cloudinary and email are mocked, so tests need no accounts and no network.
- Docs live in the same repo, so they change with the code.

## 12. Requirements covered

| Area | Requirements | Main files |
|---|---|---|
| Landing | FR-1 to FR-3 | `pageController`, `landing.ejs` |
| Register, verify, cleanup | FR-4 to FR-12 | `authController`, `userService`, `otpService`, `cleanupUnverified` |
| Login and session | FR-13 to FR-18 | `authController`, `tokenService`, `middleware/auth` |
| Forgot and reset | FR-19 to FR-23 | `authController`, `otpService`, `emailService` |
| Google | FR-24 to FR-28 | `config/passport`, `userService`, `imageService` |
| Library | FR-29 to FR-37 | `bookService`, `books/_results`, `library.js` |
| Books and cover | FR-38 to FR-50 | `bookController`, `bookService`, `imageService` |
| Notes | FR-51 to FR-54 | `noteController`, `noteService` |
| Profile | FR-55 to FR-65 | `profileController`, `profileService`, `partials/avatar` |
| Feedback and validation | FR-66 to FR-69 | `validate`, `errorHandler`, `alerts/`, `formValidation.js` |
| Quality and security | NFR-5 to NFR-8, NFR-10 | `middleware/`, `schemas/`, `tests/` |