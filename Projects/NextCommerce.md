## Setup Project

Install nextjs, tailwind, shadcn

**Authentication**
- make account on clerk for authentication -> new project -> add env.local
- install clerk/nextjs
- Create (auth)/(routes) and within it create sign-in and sign-up routes and page.jsx as prescribed by [Clerk website](https://clerk.com/docs/references/nextjs/custom-signup-signin-pages)
- (auth)/layout.jsx for extra markup around auth pages


## Thinking in React / Design UI

1. Break UI into smallest components 
2. code each state (test all) 
	- a data should be *state* if it isn't prop, can't be computed from other data and changes over time. 
3. Create heirarchy tree of components
4. Build a static version of page
5. Decide minimum **state data** that app needs
6. Refactor state into props and contexts when needed

