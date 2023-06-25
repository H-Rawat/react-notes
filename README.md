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

# ADDING INTERACTIVITY

## State: A component's memory

- Local variables dont persist between renders
- Changes to local variables won't trigger renders
- To update a component with new data - provided by useState hook
  - Retain the data between renders
  - Trigger React to render the component with new data.
- **You can't call hooks inside conditionals, loops, or other nested functions**

## Render

- Component renders when:
  - Its the component's initial render
  - The component's state has been updated

On initial render, React calls the root component.  
For subsequent renders, React calls the component whos state update triggered the render.

## State as a snapshot

```
<button onClick={() => {
        setNumber(number + 1);
        setNumber(number + 1);
        setNumber(number + 1);
      }}>+3</button>
```

Clicking on this button sets the number to 1 and not 3  
for the first setState function, number is 0 and then its increased to 1  
for the second setState function, number is still 0 because the `number` will only change to 1 on re-render and re-render is not triggered until the onClick functione executes completely.  
So number is always 0 for this snapshot of the state.

**React waits until all code in the event handlers has run before processing your state updates.**

This is whats happening :

```
<button onClick={() => {
  setNumber(0 + 1);
  setNumber(0 + 1);
  setNumber(0 + 1);
}}>+3</button>
```

**If you want to update the state multiple times before re render then use this method:**

`setNumber(n => n + 1)`

- React queues this function to be processed after all the other code in the event handler has run.
- During the next render, React goes through the queue and gives you the final updated state.

```
<button onClick={() => {
  setNumber(number + 5);
  setNumber(n => n + 1);
}}>
```

This will set the state to 6.

## Updating objects in State

`const [position, setPosition] = useState({ x: 0, y: 0 });`

It's possible to change the contents of this object like this : `position.x = 5;`.
This is called mutation.

But this should be avoided because react will not trigger the re render.

### Objects are not really nested

```
let obj = {
  name: 'Niki de Saint Phalle',
  artwork: {
    title: 'Blue Nana',
    city: 'Hamburg',
    image: 'https://i.imgur.com/Sd1AgUOm.jpg',
  }
};
```

The above object in reality looks like this:

```
let obj1 = {
  title: 'Blue Nana',
  city: 'Hamburg',
  image: 'https://i.imgur.com/Sd1AgUOm.jpg',
};

let obj2 = {
  name: 'Niki de Saint Phalle',
  artwork: obj1
};
```

## Updating arrays in State

- to push any item at the end of an array, similar to push()

```
setArtists( // Replace the state
  [ // with a new array
    ...artists, // that contains all the old items
    { id: nextId++, name: name } // and one new item at the end
  ]
);
```

- to push any item at the start of an array, similar to unshift()

```
setArtists([
  { id: nextId++, name: name },
  ...artists // Put old items at the end
]);
```

- filter can be used to delete an item from an array as it doesnt modify the original array

```
setArtists(
  artists.filter(a => a.id !== artist.id)
);
```

- transforming an array

```
const nextShapes = shapes.map(shape => {
  if (shape.type === 'square') {
    // No change
    return shape;
  } else {
    // Return a new circle 50px below
    return {
      ...shape,
      y: shape.y + 50,
    };
  }
});

// Re-render with the new array
setShapes(nextShapes);
```

- replacing items in an array

```
const nextCounters = counters.map((c, i) => {
  if (i === index) {
    // Increment the clicked counter
    return c + 1;
  } else {
    // The rest haven't changed
    return c;
  }
});

setCounters(nextCounters);
```

- inserting into an array(not at the start or end)

```
const insertAt = 1; // Could be any index
const nextArtists = [
  // Items before the insertion point:
  ...artists.slice(0, insertAt),
  // New item:
  { id: nextId++, name: name },
  // Items after the insertion point:
  ...artists.slice(insertAt)
];

setArtists(nextArtists);
setName('');
```

- reverse() and sort() mutates the original array so you shouldnt use them, instead create a copy of the array and then make changes to it.

```
const nextList = [...list];
nextList.reverse();
setList(nextList);
```

- here nextList and list points to the same object, so changing nextList also affects list

```
const nextList = [...list];
nextList[0].seen = true; // Problem: mutates list[0]
setList(nextList);
```

```
const myList = [
  { id: 0, title: 'Big Bellies', seen: false },
  { id: 1, title: 'Lunar Landscape', seen: false },
  { id: 2, title: 'Terracotta Army', seen: true },
];
const myNextList = [...myList];
const artwork = myNextList.find(a => a.id === artworkId);
artwork.seen = nextSeen; // Problem: mutates an existing item
setMyList(myNextList);
```

here line 3 mutates the original array, instead use map to change the items

```
setMyList(myList.map(artwork => {
  if (artwork.id === artworkId) {
    // Create a *new* object with changes
    return { ...artwork, seen: nextSeen };
  } else {
    // No changes
    return artwork;
  }
}));
```

# Managing State

## Reacting to input with state

1. Identify your component's different visual states

All the different states user might see:

- Empty: form has a disabled submit btn
- Typing: enabled submit btn
- Submitting: form is disabled and spinner is shown
- Success: Thank you message is shown instead of form
- Error: same as Typing state, but with an extra error msg

pass the visual state as a `status` prop to the custom component

2. Determine what triggers those state changes

- changing the text input should switch it from 'empty' state to 'typing' state
- clicking the submit button should switch it to 'submitting' state
- successful network response should switch it to 'success' state
- failed network response should switch it to 'error' state

3. Represent the state in memory with `useState`

4. Remove any non essential state variables

5. Connect the event handlers to set state

## Choosing the state structure

### **Group related state**

If some two state variables always change together, it might be a good idea to unify them into a single state variable.

`const [position, setPosition] = useState({ x: 0, y: 0 });`

### **Avoid contradictions in state**

If you have 2 states called `isSending` and `isSent`. Since `isSending` and `isSent` should never be `true` at the same time, it is better to replace them with one `status` state variable containing possible states: `typing`, `sending`, and `sent`.

### **Avoid redundant state**

If you can calculate some information from the component's props or its existing state variables during rendering, you should not put that information into that component's state.

Example of redundant state:

```
function Message({ messageColor }) {
  const [color, setColor] = useState(messageColor);
```

The problem is that if parent component passes a different value of `messageColor` later, then `color` state variable would not be updated. The state is only initialized during the first render.

“Mirroring” props into state only makes sense when you want to ignore all updates for a specific prop. By convention, start the prop name with initial or default to clarify that its new values are ignored:

```
function Message({ initialColor }) {
  // The `color` state variable holds the *first* value of `initialColor`.
  // Further changes to the `initialColor` prop are ignored.
  const [color, setColor] = useState(initialColor);
```

### **Avoid duplication in state**

The state used to be duplicated like this:

- items = [{ id: 0, title: 'pretzels'}, ...]
- selectedItem = {id: 0, title: 'pretzels'}

But after the change it’s like this:

- items = [{ id: 0, title: 'pretzels'}, ...]
- selectedId = 0

### **Avoid deeply nested state**

You can nest state as much as you like, but making it “flat” can solve numerous problems. It makes state easier to update, and it helps ensure you don’t have duplication in different parts of a nested object.

# Sharing State between components

1. Remove state from child components
2. Pass hardcoded data from the common parent.
3. Add state to the common parent.

## Controlled and Uncontrolled components

A component with some local state is called `uncontrolled`.

A component having information driven by props is called `controlled`.

# Preserving and Resetting state

React preserves a component's state for as long as its being rendered at its position in the UI tree.

```
{isFancy ? (
  <Counter isFancy={true} />
) : (
  <Counter isFancy={false} />
)}
```

whether `isFancy` is true or false, you always have a `<Counter/>`. Its the same component at the same position in the UI tree, thats why the state persists.

**If you want to preserve the state between re-renders, the structure of your tree needs to 'match up' from render to another. If the structure is different, the state gets destroyed because react destroys state when it removes a component from the tree.**

**Always declare component functions at the top level, and dont nest their definitions.**

To reset the state when the position of the components is the same in the UI tree:

1. Rendering a component in different positions

```
{isPlayerA &&
  <Counter person="Taylor" />
}
{!isPlayerA &&
  <Counter person="Sarah" />
}
```

2. Resetting state with a key: Specifying a key tells React to use the key itself as part of the position, instead of their order within the parent

```
{isPlayerA ? (
  <Counter key="Taylor" person="Taylor" />
) : (
  <Counter key="Sarah" person="Sarah" />
)}
```

**Remember that keys are not globally unique. They only specify the position within the parent.**

# Extracting state logic into a reducer

To reduce this complexity and keep all your logic in one easy-to-access place, you can move that state logic into a single function outside your component, called a “reducer”.

Migrating from `useState` to `useReducer`

1. Move from setting state to dispatching actions

```
function handleAddTask(text) {
  setTasks([
    ...tasks,
    {
      id: nextId++,
      text: text,
      done: false,
    },
  ]);
}
```

```
function handleAddTask(text) {
  dispatch({
    type: 'added',
    id: nextId++,
    text: text,
  });
}
```

- the object passed to dispatch is called an `action`

2. Write a reducer function

```
function tasksReducer(tasks, action) {
  switch (action.type) {
    case 'added': {
      return [
        ...tasks,
        {
          id: action.id,
          text: action.text,
          done: false,
        },
      ];
    }
    default: {
      throw Error('Unknown action: ' + action.type);
    }
  }
}
```

- `reducer` function is similar to the `reduce` function that's used on the arrays.

3. Use the reducer from your component.

```
import {useReducer} from 'react';
const [tasks, dispatch] = useReducer(taskReducer, initialTasks)
```

# Comparing useState and useReducer

- Code Size: try `useReducer` if there are a lot of event handlers that modify state in a similar way.
- Readability: if state updates are simple then `useState` is easier to read, otherwise use `useReducer`.
- Debugging: easier with `useReducer`
- Testing: Reducer because its a pure function

## Writing reducers well

- reducers should be pure
- each action should describe a single user interaction

# CONTEXT

1. Create the context

```
import {createContext} from 'react';

export const LevelContext = createContext(1);
```

2. Use the context

```
import { useContext } from 'react';
import { LevelContext } from './LevelContext.js';

export default function Heading({children}) {
  const level = useContext(LevelContext);
}
```

3. Provide the context

```
import {LevelContext} from './LevelContext.js';

export default function Section({ level, children }) {
  return (
    <section>
      <LevelContext.Provider value={level}>
        {children}
      </LevelContext.Provider>
    </section>
  )
}
```

## Reading from the component above

```
import {LevelContext} from './LevelContext.js';
import { useContext } from './LevelContext.js';

export default function Section({ children }) {
  const level = useContext(LevelContext);
  return (
    <section>
      <LevelContext.Provider value={level + 1}>
        {children}
      </LevelContext.Provider>
    </section>
  )
}
```

**in React, the only way to override some context coming from above is to wrap children into a context provider with a different value.**

## USE CASES FOR CONTEXT

- Theming
- Current account
- Routing
- Managing State
