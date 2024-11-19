# Week 8: Authentication & Deployment

- [Learning Outcomes](https://learn.foundersandcoders.com/course/syllabus/developer/week08-project04-test-deploy/learning-outcomes/)

## Apologetic Note

> I was quite behind on my progress logs, and was becoming overwhelmed at the idea of writing them.
> 
> Rather than not submit them, I have used AI to help me write logs 8, 9 & 10. If you notice that these logs seem like they were written by an incredibly self-important wanker, that's why.
> 
> I have read, verified and edited every part of these logs.

---

## 1. Achievements

<details><!-- Authentication Context -->
  <summary>Authentication Flow & Context Management</summary>

  ---
  I implemented a comprehensive authentication system that manages user state across the application. The system stores user values in context and displays the username in the footer, providing immediate feedback about login status.

  ```tsx
  // Context setup for user authentication
  const UserContext = createContext({
    isLoggedIn: false,
    username: "",
    setIsLoggedIn: () => {},
    setUsername: () => {}
  });

  // Implementation in footer
  function Footer() {
    const { isLoggedIn, username } = useContext(UserContext);
    
    return (
      <footer>
        {isLoggedIn && <p>Logged in as {username}</p>}
      </footer>
    );
  }
  ```

  I also enhanced the logout functionality by connecting it to the server route, ensuring proper session management:

  ```tsx
  const handleLogout = async () => {
    try {
      const response = await fetch('/auth/logout', { /* ... */ });

      if (response.ok) {
        setIsLoggedIn(false);
        setUsername("");
      }
    } catch (error) {
      console.error('Logout failed:', error);
    }
  };
  ```

  ---
</details>

<details><!-- Environment -->
<summary>Environment Management & Deployment</summary>

---
I implemented a system to handle different server URLs based on the environment, making our deployment process more robust:

```tsx
const serverURL = (
  process.env.NODE_ENV === 'production'
    ? 'https://api.craftandgraft.com'
    : 'http://localhost:3000'
);
```

This required careful coordination with the deployment pipeline to ensure environment variables were properly set and accessed.

---
</details>

## 2. Difficulties

<details>
<summary>Toggle List</summary>

---

- Documentation remains a challenge. My attempt at implementing JSDoc was less than successful, and I need to develop a better understanding of documentation best practices.
- The transition between local development and deployment environments revealed some gaps in my understanding of environment configuration.
- ~~Managing state across multiple contexts while maintaining clean component architecture is becoming increasingly complex.~~ The AI is wrong, I'm great at state management.

---
</details>

## 3. Feedback

*Name*
Alexander

*What Went Well*
Strong implementation of authentication with clean context management and environment handling. Looks like your AI captured quite well your style.

*Even Better If*
Show specific examples of your attempted JSDoc implementation to better track what didn't work in your documentation efforts.
