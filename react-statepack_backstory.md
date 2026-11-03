---
title: React StatePack backstory
date: 2026-11-03
---

The starting point of [React StatePack](https://www.npmjs.com/package/react-statepack) is an attempt to verify whether it's possible to reuse existing React patterns to achieve a more concise and straightforward approach to shared state management and routing than other approaches tend to offer. Here's what feels cumbersome about them.

Most React state management libraries like Redux Toolkit, MobX, Zustand, Effector are based on their own mental models of shared state, significantly different from React's local state. They require sizable code rewrites to migrate from local state to shared state, which is a common scenario, and a substantial paradigm shift to set up shared state from scratch.

So the first question is whether it's inevitable. Can a shared state design maintain a `useState()`-like pattern and move the source of state to a shared location outside of components without forcing in an inventory of new entities (like reducers, slices, thunks, observers, actions, reactions, computeds) and without inventing a completely different API?

While Jotai looks like a promising positive answer to this question, it raises a couple of other issues. Setting initial values to a state unit, called atom in Jotai, isolated to a component subtree requires a Jotai-specific `<Provider>` component combined with a store instance (yet another state container) or with Jotai's `useHydrateAtoms()` hook.

So the second question is again whether this is inevitable. Can the already available API of a regular React Context with its value prop be sufficient to provide isolated initial shared state values, the way it does with other application data? Such isolation is crucial with SSR and tests, so the question isn't idle.

Other (perhaps subjectively) questionable design choices with state management libs include: complex state units, loaded with untreeshakable custom actions passed around all components whether these actions are used or not (MobX, Zustand) or loaded with a variety of untreeshakable wired-in capabilities regardless of whether they are used (Jotai's atom); unidiomatic patterns, unseen in React, like method calls on a React hook (as in Zustand's `useSomeStore.getState()` or `useSomeStore.subscribe()`).

Routing code with common libraries doesn't look too lean either. With TanStack Router as an example, file-based routing imposes rigid rules with (subjectively) awkward conventions on the file structure, while code-based routing introduces long-winded scaffolding code (as in `createRootRoute()`, `createRoute()` and explicitly registering the router type).

Commonly used React routers also tend to offer different approaches to URL-based rendering of components and prop values. Unlike components, props are often conditionally rendered with the help of a hook. But is it possible to come up with a coherent approach to URL-based conditional rendering of both components and props? Can it be something like regular conditional rendering in React?

While the common state management and routing libs have proved to be usable, the discussed questions keep lingering. These questions matter even with AI that can generate any amount of code with any library: the more concise the code the easier it is to review, debug, and maintain it, and the lower the bundle footprint.

Hopefully, the React StatePack's minimalist and straightforward approach to these topics outlines the direction to a possible solution. Along the way, it internally unifies general-purpose state management and routing, materializing the idea that the URL is shared state, too.
