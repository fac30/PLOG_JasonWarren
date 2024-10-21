# Authentication

## Curriculum

- [Workshop](https://learn.foundersandcoders.com/learn/auth/)
- [Challenge](https://github.com/foundersandcoders/auth-challenge)

## Workshop

### Using cookies for authentication

Now we know how to use cookies to store info in the browser we can keep a user logged in. Once we’ve verified their password we can store a cookie that effectively says “this is oli”. On subsequent requests the server would read the cookie to find out which user it came from. E.g. the cookie might look like this: userid=1.

Unfortunately this wouldn’t be very secure. Anyone can send any HTTP headers they like to your server. You could use dev tools to edit your request to send userid=2 to pretend to be a totally different user.

We need a way to ensure our cookie hasn’t been tampered with. Luckily cryptography has a solution for us: signatures. This means combining a value with a secret in a one-way mathematical operation. This is called a “signature”, and it can only be reproduced by someone with both the value and the secret.

If we send both the value and the signature in the cookie we can recalculate the signature on the server (using the secret) to make sure the cookie wasn’t tampered with. E.g. if someone changed userid=1 to userid=2 it would break the signature.

Express’ cookie-parser middleware supports signing cookies automatically. You just have to pass a secret in when you initialise the middleware, then set the signed option when you create a cookie:

server.use(cookieParser("random-string-that-should-be-an-env-var"));

// ...
response.cookie("hello", "world", {
  signed: true,
  httpOnly: true,
  maxAge: 6000,
  sameSite: "lax",
});
Now when you read cookies they’ll be available at request.signedCookies:

// Reading a later request...
console.log(request.signedCookies);
// { hello: "world" }
If the signature didn’t match the cookie will not be present in the object.

You should use signed cookies for pretty much everything

### Stateless authentication flow 

Our current auth flow looks like this:

Ask user for a password on login
Read the hash from the DB
Check the hashes are the same with bcrypt.compare
Set a signed cookie with any user info we might need
(later) read the cookie to find out info about the user
This is known as “stateless” auth, because we store all the info we might need for authentication in the cookie. The server doesn’t need to check the DB to know whether a request is authenticated.

This is convenient but has some downsides:

Cookies have a 4kb size limit, so you can’t fit too much info in them.
The server cannot log users out. Any device with a valid cookie is “logged in”.
The server cannot enforce expiry time, since users can edit their cookies in the browser.
Ideally we need a system that lets us keep track of who is logged in on the server, so we can revoke that access when necessary.

### Session authentication flow 

Instead of storing all the user info in the cookie, we can just store a random “session ID” that corresponds to info stored in our DB. Now the cookie itself has no power, the server decides whether the user is logged in based on the session info.

Here’s a modified auth flow:

Ask user for a password on login
Read the hash from the DB
Check the hashes are the same with bcrypt.compare
Insert row into sessions table with user info
Set a signed cookie with the session ID
(later) read the session ID from the cookie
(later) look up the session info from the DB
This avoids all the problems with stateless auth:

We only store an ID in the cookie, so the 4kb limit doesn’t matter
The server can log users out by deleting the corresponding row from the DB
The server can store session expiry dates in the DB to ensure an old cookie cannot be reused
Here’s roughly how this might be implemented. First we need a table to store our session in:

-- schema.sql
CREATE TABLE IF NOT EXISTS users (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  -- plus other columns...
);

CREATE TABLE IF NOT EXISTS sessions (
  id TEXT PRIMARY KEY,
  user_id INTEGER REFERENCES users(id),
  expires_at DATETIME NOT NULL
)
For now we’re just storing the user_id, which we can use to look up other user info later. We’ll also store the expiry time so we can ensure users aren’t logged in forever.

Then we need a function to generate a random session ID and insert the session:

// model.js
const crypto = require("node:crypto");

// Sets the expiry to current date + 7 days
const insert_session = db.prepare(`INSERT INTO sessions VALUES (
  $id,
  $user_id,
  DATE('now', '+7 days')
)`);

function createSession(user_id) {
  // quick way to generate a random string in Node
  const id = crypto.randomBytes(18).toString("base64");
  insert_session.run({ id, user_id });
  // return the generated ID so we can store in a cookie
  return id;
}
We’ll also need function to retrieve a session:

// model.js

const select_session = db.prepare(`
  SELECT id, user_id, expires_at
  FROM sessions WHERE id = ?
`);

function getSession(sid) {
  return select_session.get(sid);
}
Now handlers can read the session cookie to find out which user (if any) made the request:

// routes/private.js

function get(req, res) {
  const sid = req.signedCookies.sid;
  const session = model.getSession(sid);
  if (session && session.user_id) {
    // we have a logged in user
    res.send("<h1>Private stuff</h1>");
  } else {
    // request is not authenticated
    res.status(401).send("<h1>Please log in to view this page</h1>");
  }
}
Since this logic will be repeated in any request that needs to check the session it would be worth abstracting into an Express middleware.

### Authentication summary

Things got a little complicated, so let’s recap our final authentication flow to make it clear:

#### Signing up

User submits their email and password
Hash the password with BCrypt
Insert a new user into the DB to store email and hash
Create new session in the DB
Set a signed cookie with the session ID
Redirect to whatever page comes next

#### Logging in

User submits their email and password
Retrieve the stored user where email matches
Compare submitted password with stored hash
If they match create a new session in the DB
Set a signed cookie with the session ID
Redirect to whatever page comes next

#### Checking auth

Read the session ID from the signed cookie
Retrieve the stored session from DB
(optional) remove the session & cookie if expired
Get the user ID from the session
(optional) Retrieve the stored user from the DB using the user ID
Decide whether the user is allowed to see the page

---

## Authentication Challenge

> Under Construction

---

## Holding Bay
