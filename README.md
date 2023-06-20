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
