# Monday 21st October

- [Log](#log)
- [Signatures](#signatures)
- [Flows](#flows)
	- [Flows to Consider](#flows-to-consider)
	- [Overall Flow](#overall-flow)
- [Random String Generator](#random-string-generator)

---

## Log

- Tried out a new paper workflow
- Investigated some new node packages
  - [Passport](https://www.passportjs.org/)
  - [Nodemailer](https://nodemailer.com/)
  - [express-validator](https://express-validator.github.io)

---

## Signatures

- [ ] Use `value` & `secret` to generate a `signature`
- [ ] Send `signature` & `value` to the server
- [ ] Server uses `secret` to check authenticity

---

## Flows

### Flows to Consider

#### Stateless Authentication

##### Stateless Flow

1. Ask user for a password on login
2. Read the hash from the DB
3. Check the hashes are the same with bcrypt.compare
4. Set a signed cookie with any user info we might need
5. (later) read the cookie to find out info about the user

##### Stateless Issues

- Cookies have a 4kb size limit, so you can’t fit too much info in them.
- The server cannot log users out. Any device with a valid cookie is “logged in”.
- The server cannot enforce expiry time, since users can edit their cookies in the browser.

#### Session Authentication

##### Session Flow

1. Ask user for a password on login
2. Read the hash from the DB
3. Check the hashes are the same with bcrypt.compare
4. Insert row into sessions table with user info
5. Set a signed cookie with the session ID
6. (later) read the session ID from the cookie
7. (later) look up the session info from the DB

##### Session Benefits

- We only store an ID in the cookie, so the 4kb limit doesn’t matter
- The server can log users out by deleting the corresponding row from the DB
- The server can store session expiry dates in the DB to ensure an old cookie cannot be reused

### Overall Flow

#### Signing Up

1. User submits their email and password
2. Hash the password with BCrypt
3. Insert a new user into the DB to store email and hash
4. Create new session in the DB
5. Set a signed cookie with the session ID
6. Redirect to whatever page comes next

#### Logging In

1. User submits their email and password
2. Retrieve the stored user where email matches
3. Compare submitted password with stored hash
4. If they match create a new session in the DB
5. Set a signed cookie with the session ID
6. Redirect to whatever page comes next

#### Checking Auth

1. Read the session ID from the signed cookie
2. Retrieve the stored session from DB
3. (optional) remove the session & cookie if expired
4. Get the user ID from the session
5. (optional) Retrieve the stored user from the DB using the user ID
6. Decide whether the user is allowed to see the page

---

## Random String Generator

YES IS GOOD

```js
import crypto from "node:crypto";

const id = crypto.randomBytes(18).toString("base64");
```

---
