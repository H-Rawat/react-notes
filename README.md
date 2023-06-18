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

# DESCRIBING THE UI

- Never nest definitions of components
- A file can have only one default export but can have multiple named exports

## RULES OF JSX

- Return a single root element because under the hood JSX is transformed into plain JS objects. You can't return two objects from a function without wrapping them into an array.
- Close all the tags
- camelCase most of the things because attributes written in JSX become keys of JS objects.

### Specifying a default value for a PROP

```
function Avatar({person, size = 100}) {
  // ...
}
```

- default value of size is only used if the size prop is missing or if you pass `size={undefined}`

### Conditional Rendering

- Return `null` when you dont want to return anything from a component

- If messageCount is 0, then 0 will be returned from the expression `messageCount && <p>New messages</p>`

### KEYS

- Don't use indexes as keys.

## PURE COMPONENTS

- Pure function
  - It minds its own business : it doesnt change any obj or variables that existed before it was called.
  - Same input, same output

Eg :

```
function double(number) {
  return 2 * number;
}
```

- React uses strict mode to call functions twice to look for any impure functions.

### Side Effects

Side effects happen on the side and not during rendering.

Updating the screen, starting an animation, changing the data are called side effects.

Event handlers dont need to be pure so you can handle side effects inside event handlers.

Side effects can also be handled inside a `useEffect`, but this should be the last resort.
