---
# You can also start simply with 'default'
theme: default
info: |
  ## Slidev Starter Template
  Presentation slides for developers.

  Learn more at [Sli.dev](https://sli.dev)
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

# Rules of react <img class="inline-block relative -top-1" width="64" height="64" alt="react logo" src="/media/react-logo-dark.svg">

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

<h1>Rule #1 <br><v-click>React components are <strong>pure</strong> 😇</v-click></h1>

---
layout: center
---

# Purity - part 1

<a class="text-3xl" href="https://react.dev/reference/rules/components-and-hooks-must-be-pure#side-effects-must-run-outside-of-render">A component performs side effects outside the render</a>


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