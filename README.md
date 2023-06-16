# REACT NOTES

## COMPONENTS

- piece of UI that has its own login and appearance
- they are js functions that return markup
- they start with a capital letter

## JSX

- stricter than HTML. Eg: you have to close tags like <br />
- cant return multiple JSX tags, they should be wrapped in parent component like react **fragments**
- use this [online converter](https://transform.tools/html-to-jsx) to convert your HTML to JSX.

## Escape into Javascript

You can escape into JS by using curly braces.

`<h1>{user.name}</h1>`

## CSS in JSX

`style={{color: 'yellow'}}`

You can use a regular object inside the `style={}`. Use **style** attribute when your styles depend on the JS variables.

## KEY ATTRIBUTE

You should pass a string or a number to each item in a map to uniquely identify the items.  
React uses keys to check if an item has been deleted, inserted or reordered

## Props

Information that you pass down to the children components.

Passing props to children components is called "lifting state up".

# THINKING IN REACT

- Break the UI into a component hierarchy
  - A component should ideally do only one thing
  - seperate your UI into components where each component matches one piece of your data model.
- Build a static version
- Find the minimal but complete representation of UI state
  - to check if you even need state:
    - Does it remain unchanged over time? If so, it isn’t state.
    - Is it passed in from a parent via props? If so, it isn’t state.
    - Can you compute it based on existing state or props in your component? If so, it definitely isn’t state!
- Identify where your state should live
  - Identify every component that renders something based on that state.
  - Find their closest common parent component—a component above them all in the hierarchy.
  - Decide where the state should live:
    - Often, you can put the state directly into their common parent.
    - You can also put the state into some component above their common parent.
    - If you can’t find a component where it makes sense to own the state, create a new component solely for holding the state and add it somewhere in the hierarchy above the common parent component.
- Add inverse data flow
