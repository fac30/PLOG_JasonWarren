# Week 7: Authentication

- [Learning Outcomes](https://learn.foundersandcoders.com/course/syllabus/developer/week07-project04-authentication/learning-outcomes/)

## 1. Achievements

<details>
<summary>Event Bubbling: A Detective Story</summary>

---

I was pleased with this one, as I managed to solve it using only console logging and one tiny bit of googling. This is the `<Header/>` component, which sits right at the top level of my `<App />`.

![Header Bar](../assets/images/images_07/header.jpg)

Inside it are two elements. 

On the left, we have a `<div />` to hold the `<h1>` and, eventually, a logo. The `<h1>` carries an `onClick` handler that sets `view` (a state in the `StoreContext` that dictates which components render in the main `Content` section) to `"landing"`.

On the right, `<UserButtonGroup />` holds `<LogInButton />` (or other buttons for a logged in user). That button's `onClick` handler sets `view` to `"login"`.

And yet... when I clicked the login button, the landing page would appear. I added console logs everywhere:

- the login button's `handleClick()`, to make sure the right element registered a click
- `function Content() { ... }` component, to see what it thought the `view` state was
- the `<h1>`'s `onClick()`, to see if it was somehow being called

Eventually, I realised there was one place I wasn't logging; my contexts are stored in their own components, and I hadn't added any logs to the state itself. I added a log within a `useEffect()` triggered by changes to `view` and I saw that the state *was* being correctly set to `"login"`... but a fraction of a second later, it was set back to `"landing"`.

I didn't know about event bubbling at this point, but I understood the structure of the code well enough to know the following:

- The `onClick()` handler in the button was being called correctly.
- By the time `<Content />` checked value of `view`, it had been set back to `"landing"`
- Something in between `<LogInButton />` and `<Content />` was calling `setView("landing")`
- The possible culprits were:
	1. `<UserButtonGroup />`
	2. `<Header />`
	3. `<App />`
	4. `<Content />`

I had already ruled out `<Content />`, there is barely any code in `<App />` and `<UserButtonGroup />` was just a wrapper with no access to the state. `<Header />` was the only option left, but to make sure I also added logs to each of these components that would fire when their exported function was called and when they were about to return.

```zsh
Login Button clicked by logged out user
Setting view to: login
Calling UserButtonGroup
Calling Header
Setting view to: landing
Calling Content
Calling return on Content: landing
```

There it was. Somehow, `setView()` was being called in `<Header />`. I knew intuitively that this had to be somehow related to the `onClick()` handler in the `<h1>` element, but at that point I was at the limit of my detective skills and dove into StackOverflow.

---
</details>

<details>
<summary>Quantums: Components to Handle Conditional Content</summary>

---

I settled on a type of component that would exist purely to handle conditional content. They would be fragments that contained a ternary operator with the following structure:

```ts
import { useUser } from "../../context/User";
import componentTrue from "../category/componentTrue";
import componentFalse from "../category/componentFalse";

function Quantum() {
	const { stateToCheck } = useUser();

	return (
		<>
			{stateToCheck === "true" ? <componentTrue /> : <componentFalse />}
		</>
	)
}
```

The most obvious use case for this is a login/logout button.

```ts
import { useUser } from '../../context/User'
import LogInButton from '../buttons/LogInButton';
import LogOutButton from '../buttons/LogOutButton'

function LogButtonQuantum () {
	const { isLoggedIn } = useUser();

  return (
		<>
			{isLoggedIn ? <LogOutButton /> : <LogInButton />}
		</>
	)
}

export default LogButtonQuantum
```

---
</details>

<details>
<summary>SECTIONS TO FINISH</summary>

---

- Form Inputs with autocomplete attributes
- One form, two submit Buttons
- `{ action, setAction() }` State
- 1 `handleSubmit()` to handle 2 routes
- Using Regex to derive username from email

---
</details>

---

## 2. Difficulties

<details>
<summary>TO FINISH: AWS Bollocks</summary>

---

xxx

![get in the sea 1](../assets/images/images_07/cdk1.jpg)

> trolololol

![get in the sea 2](../assets/images/images_07/cdk2.jpg)

---
</details>

---

## 3. Full Progress List

### Key

<details>
<summary>Toggle Key</summary>

---

- [X] I feel like I've learned/demonstrated this skill in the past week
- I have acquired some skill but need to develop further
- [ ] I am not yet comfortable in this skill

---

</details>

### List

<details>
<summary>Authentication</summary>

---

- [X] Learn how to handle user registration, login, and logout functionality securely
- [X] Understand the principles of authentication and authorisation in web applications
- [ ] Develop strategies to mitigate security threats such as Cross-site Request Forgery (CSRF) attacks

---
</details>

<details>
<summary>React</summary>

---

- [ ] Implement navigation between different pages using React Router
- [X] Understand using the context API and how it can help with complicated state management in React
- Have working knowledge of the useReducer hook and how it can aid in using useContext for state management

---
</details>

---

## 4. Feedback

|                  |                         |
| ---------------- | ----------------------- |
| *Name*           |                         |
| *What Went Well* |                         |
| *Even Better If* |                         |
