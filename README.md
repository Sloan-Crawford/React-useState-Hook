# React useState hook example and explanation

## ========== Notes ============

- Hooks can only be used inside function componentes, not class components
- hooks must always be called in the exact same order in every component render...
- (so not in if statements, functions, loops or nested in anything, only top level)

-----------useState------------
it is a function
what we pass as a param is what the default state is (in this case, 4)
it will always return an array of 2 values. [count, setCount]
the first param (count) will be the CURRENT STATE at every single iteration of our render function
the first param (setCount) is a function that allows us to UPDATE OUR STATE
then create a function (decrementCount) that runs setCount and passes in count - 1 (WRONG WAY!)
when we update the state, the component re-renders, just like with this useState hook
(CORRECT WAY!) pass in a function to setCount that takes in the previous state value
take prevCount and subtract 1 (works properly to subtract 2 if we call it twice)

```
import React, { useState } from "react";

function App() {

  const [count, setCount] = useState(4)

  function decrementCount() {
    setCount(prevCount => prevCount - 1)
  }

  function incrementCount() {
    setCount(prevCount => prevCount + 1)
  }

  return (
    <div>
      <button onClick={decrementCount}>-</button>
      <span>{count}</span>
      <button onClick={incrementCount}>+</button>
    </div>
  );
}

export default App;
```

when initial state passed as param for useState is complex and slow:
useState can always pass a function version of state that runs on only the first re-render:

```
const [count, setCount] = useState(() => {
  console.log('run function')
  return 4
})
```

useState works differently when working with objects:
it doesn't merge the setState value when working with an object (it overides it all instead)
we need to spread out the previous state first, then add the updated state/count:

```
const [state, setState] = useState({ count: 4, theme: 'blue' })
 const count = state.count
 const theme = state.theme

  function decrementCount() {
    setState(prevState => {
     return { ...prevState, count: prevState.count - 1 }
   })
  }
```

good practice to have multiple state hooks:
this example has both the first hook and the theme hook:

```
function App() {

  const [count, setCount] = useState(4)
  const [theme, setTheme] = useState('blue')

  function decrementCount() {
    setCount(prevCount => prevCount - 1)
    setTheme('red')
  }

  function incrementCount() {
    setCount(prevCount => prevCount + 1)
    setTheme('blue')
  }

  return (
    <div>
      <button onClick={decrementCount}>-</button>
      <span>{count}</span>
      <span> {theme}</span>
      <button onClick={incrementCount}>+</button>
    </div>
  );
}

export default App;
```

this uses two different hooks to manage two different types of state (no clash)
easier to manage!
