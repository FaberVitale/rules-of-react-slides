---
# You can also start simply with 'default'
theme: default
info: |
  ## Rules of react slides
  See also https://react.dev/reference/react#rules-of-react
# apply unocss classes to the current slide
class: text-center
# https://sli.dev/features/drawing
drawings:
  persist: false
# slide transition: https://sli.dev/guide/animations.html#slide-transitions
transition: slide-left
# enable MDC Syntax: https://sli.dev/features/mdc
mdc: true
# take snapshot for each slide in the overview
overviewSnapshots: true
title: 'Rules of react'
---

<style>
  @keyframes rotate {
    from {
      rotate: 0deg;
    }

    to {
      rotate: 360deg
    }
  }

  .spin-anim {
    animation: 4s linear infinite running rotate;
  }
</style>

# Rules of react <img class="inline-block relative -top-1 spin-anim" width="64" height="64" alt="react logo" src="/media/react-logo-dark.svg">

<div class="fixed bottom-2 right-2">
<h2>Author: Fabrizio A. Vitale</h2>
</div>

---
layout: intro
---

# So... what are them?

---
layout: intro
---

<h1>They are a set of guidelines that helps us write good quality react components</h1>

---
layout: intro
---

# Uhmm... that's not right 🤔

---
layout: intro
---


<h1 class="relative"><strong class="absolute -top-16 right-54" style="rotate: -20deg;">RULES</strong> They are a set of <s>guidelines</s> that makes us write good quality and <strong>bug-free</strong> react components</h1>

---
layout: intro
---

# Why we called them rules?

## Because breaches of those rules will lead to buggy and non determinstic applications

<span class="text-8xl fixed bottom-2 right-2">🐛</span>

---
layout: intro
---

<h1>Rule #1 <br><v-click>React components and hooks are <strong>pure</strong> 😇</v-click></h1>

---
layout: center
---

# Purity - part 1

<a class="text-3xl" href="https://react.dev/reference/rules/components-and-hooks-must-be-pure#side-effects-must-run-outside-of-render">A component performs side effects outside the render</a>

<p>Effects are confined to listeners ad useEffect</p>

---
layout: center
---

# Purity part 1 - Side effects in render 👎

```tsx
export function BadComponent(props) {
  const [user, setUser] = useState(null);
  
  // BAD
  fetch(props.url)
    .then((res) => res.json())
    .then(setUser)

  return <p>{user?.firstname} {user?.lastname} </p>
}
```

---
layout: center
---

# Purity part 1 - Side effects ouside render phase 👍

```tsx
export function BadComponent(props) {
  const [user, setUser] = useState(null);
  
  // GOOD
  useEffect(() => {
    const controller = new AbortController();
    fetch(props.url, { signal: controller.signal })
      .then((res) => res.json())
      .then(setUser);

    return () => { controller.abort() }
  }, [props.url]);


  return <p>{user?.firstname} {user?.lastname} </p>;
}
```

---
layout: center
---

# Purity - part 2

<a class="text-3xl" href="https://react.dev/reference/rules/components-and-hooks-must-be-pure#components-and-hooks-must-be-idempotent">Components must be idempotent</a>

---
layout: center
---

# Idempotent meaning:
<span class="text-3xl">Components must <strong>always return</strong> the same output with respect to their dependencies – props, state, and context.</span>

---
layout: center
---

<h1>In other words,  <br> if props, state, context do not change <br> the component's output must remain the same</h1>

---
layout: center
---

# Purity - part 2 Non idempotent component 👎

```tsx
export function Clock() {
  const time = new Date();

  return (
    <time dateTime={time.toISOString()}>
      {time.toLocaleString()}
    </time>
  );
}
```

---
layout: center
---

# Purity - part 3

<a class="text-3xl" href="https://react.dev/reference/rules/components-and-hooks-must-be-pure#mutation">React components and hooks do not mutate non-local values</a>
---
layout: center
---

# Local mutation 👍

```tsx
function FriendList({ friends }) {
  const items = []; // ✅ Good: locally created
  for (let i = 0; i < friends.length; i++) {
    const friend = friends[i];
    items.push(
      <Friend key={friend.id} friend={friend} />
    ); // ✅ Good: local mutation is okay
  }
  return <section>{items}</section>;
}
```

---
layout: center
---

# Mutation of external dependencies 👎

```tsx
const items = []; // not local

function FriendList({ friends }) {
  for (let i = 0; i < friends.length; i++) {
    const friend = friends[i];
    items.push(
      <Friend key={friend.id} friend={friend} />
    ); // 👎 mutation of external values
  }
  return <section>{items}</section>;
}
```

---
layout: intro
---

# So...why component's and hooks's purity matters?

---
layout: center
---

# React relies on [referential transparency](https://stackoverflow.com/a/210869) in its internal implementation.

---
layout: center
---

<h1>Several internal optimizations and <a href="https://www.linkedin.com/pulse/understanding-react-re-rendering-overview-shallow-examples-pandey">heuristics</a> are based on this contract</h1>

---
layout: center
---

<h1>Public apis, such as <a href="https://react.dev/reference/react/memo">memo</a>, works correctly if the purity is upheld.</h1>

---
layout: center
---

<h1><a href="https://react.dev/learn/react-compiler">react compiler</a> works only if components are pure.</h1>

---
layout: intro
---

# Uhm... seems important 🧐

## How can we detect if components are not pure?


---
layout: center
---

# We can use enable [strict mode](https://react.dev/reference/react/StrictMode#fixing-bugs-found-by-double-rendering-in-development) to detect components that are not pure.

---
layout: center
---

# In strict mode, components are [rendered twice](https://react.dev/reference/react/StrictMode#fixing-bugs-found-by-double-rendering-in-development), helping us detecting impure components
