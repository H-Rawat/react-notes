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
