# Redux

https://redux.js.org/introduction/getting-started

## Flow

![](C:\ravi\Training\Notes\React\images\redux_flow.PNG)



**Types of State**

![](C:\ravi\Training\Notes\React\images\types_of_states.PNG)

### Sample Redux Code (for Node)

```js
//redux-basics.js
const redux = require("redux");
const createStore = redux.createStore;

const initialState = {
  counter: 0,
};

//Reducer
const rootReducer = (state = initialState, action) => {
    if(action.type === 'INC_COUNTER'){
        return {
            ...state,
            counter: state.counter + 1
        }
    }

    if(action.type === 'ADD_COUNTER'){
        return {
            ...state,
            counter: state.counter + action.value
        }
    }
  return state;
};

//Store
const store = createStore(rootReducer);
console.log(store.getState());

//Subscription
store.subscribe(() => {
    console.log('[Subscription]', store.getState());
})

//Dispatching Action
store.dispatch({
  type: "INC_COUNTER", /***/
});
store.dispatch({
  type: "ADD_COUNTER",
  value: 10
});

console.log(store.getState());

//To run
node redux-basics.js
```

We need a reducer to create a store, we can pass an initial state to the reducer while creating the reducer. 

Reducer takes `state` and `action` as parameters. Make sure you change the state **unmutated**, otherwise we will get undesired results.

We can susbcribe using the **subscribe** method, when the store is updated the subscriptions are notified.

We call action you using **dispatch** method, it takes object as input, we need to make sure we pass the `type` as part of the object as it is mandatory. `type` will be type of the action which we want to call, other than that we can pass any information(values or a complete payload). 



## Redux in React

### Store creation

The store should be created right before our application or when our application starts, so the index.js file is a great place, this is where we mount our app component to the dom, so creating the store here also makes a lot of sense.

```react
//reducer.js
const initialState = {
    counter: 0
}

const reducer = (state = initialState, action) => {
    if (action.type === "INCREMENT") {
    return {
      counter: state.counter + 1,
    };
  }
  if (action.type === "DECREMENT") {
    return {
      counter: state.counter - 1,
    };
  }
  if (action.type === "ADD") {
    return {
      counter: state.counter + action.val,
    };
  }
  if (action.type === "SUBTRACT") {
    return {
      counter: state.counter - action.val,
    };
  }
    return state;
}

export default reducer;

//index.js
import {createStore} from 'redux';
import reducer from './store/reducer';

const store = createStore(reducer);
```



### Connecting React to Redux

Install the `react-redux` package.

```powershell
npm install --save react-redux
#or
yarn add react-redux
```

```react
//index.js
//Wrap the app Provider tag and pass the store
import {createStore} from 'redux';
import reducer from './store/reducer';
import {Provider} from 'react-redux'; //***

const store = createStore(reducer);

ReactDOM.render(<Provider store={store}><App /></Provider>, document.getElementById('root')); //***
```

**Provider** is a helper component which allows us to kind of inject our store into the react components. A special property expected by the component of course. It's named `store` and here, I passed a store created with createStore, which in our case is stored in that store constant.

Consuming in the containers

```react
//Counter.js
import React, { Component } from 'react';
import {connect} from 'react-redux';

class Counter extends Component {
    render () {
        return (
           <div>
                <CounterOutput value={this.props.ctr} /> <!-- *** -->
                <CounterControl label="Increment" clicked={this.props.onIncrementCounter} /> <!--***-->
                <CounterControl label="Decrement" clicked={this.props.onDecrementCounter}  />
                <CounterControl label="Add 5" clicked={() => this.props.onAddCounter(5)}  />
                <CounterControl label="Subtract 5" clicked={() => this.props.onSubtractCounter( 5 )}  />
            </div>
        );
    }
}

const mapStateToProps = (state) => { //*** Mapping the react states with container props
    return {
        ctr: state.counter
    }
}
const mapDispatchToProps = (dispatch) => {
    return {
        onIncrementCounter: () => dispatch({type: 'INCREMENT'}),
        onDecrementCounter: () => dispatch({type: 'DECREMENT'}),
        onAddCounter: (value) => dispatch({type: 'ADD', val: value}),
        onSubtractCounter: (value) => dispatch({type: 'SUBTRACT', val: value}),
    }
}
export default connect(mapStateToProps, mapDispatchToProps)(Counter); //***
```

**connect** is a function which returns a higher order component, which takes then a component as input. Since it  returns a function, we then execute the result of connect of this function execution.

`mapStateToProps` user defined, it actually stores a function which expects the **state** stored in redux as the input and returns a javascript object which is a map of prop names and slices of the state stored in redux. Here we define the required **states** to be manage by the container. This information will passed to **connect**. 

`mapDispatchToProps` user defined, similar to mapStateToProps but here we map the **actions**. This is how we use the **dispatch** to call the actions of redux.

### Updating Objects Immutably

```react
//reducer.js
const initialState = {
  counter: 0,
  results: [],
};

const reducer = (state = initialState, action) => {
  switch (action.type) {
    case "INCREMENT":
      return {
        ...state,
        counter: state.counter + 1,
      };
    case "STORE_RESULT":
      return {
        ...state,
        results: state.results.concat({ id: new Date(), value: state.counter }), //***
      };
    case "DELETE_RESULT":
      return {
        ...state,
        results: state.results.filter((result) => result.id !== action.resultElId), //***
      };
  }
  return state;
};

export default reducer;
```

`Array.concat` will create a new array so it can be used to add immutably.

 `Array.filter` will create a new array so it can be used to remove immutably.

#### Updating Nested Objects

The key to updating nested data is **that \*every\* level of nesting must be copied and updated appropriately**. This is often a difficult concept for those learning Redux, and there are some specific problems that frequently occur when trying to update nested objects. These lead to accidental direct mutation, and should be avoided.

https://redux.js.org/recipes/structuring-reducers/immutable-update-patterns/

#### Common Mistake #1: New variables that point to the same objects

Defining a new variable does *not* create a new actual object - it only creates another reference to the same object. An example of this error would be:

```react
function updateNestedState(state, action) {
    let nestedState = state.nestedState;
    // ERROR: this directly modifies the existing object reference - don't do this!
    nestedState.nestedField = action.data;

    return {
        ...state,
        nestedState
    };
}
```

This function does correctly return a shallow copy of the top-level state object, but because the `nestedState` variable was still pointing at the existing object, the state was directly mutated.

#### Common Mistake #2: Only making a shallow copy of one level

Another common version of this error looks like this:

```react
function updateNestedState(state, action) {
    // Problem: this only does a shallow copy!
    let newState = {...state};

    // ERROR: nestedState is still the same object!
    newState.nestedState.nestedField = action.data;

    return newState;
}
```

Doing a shallow copy of the top level is *not* sufficient - the `nestedState` object should be copied as well.

#### Correct Approach: Copying All Levels of Nested Data

Unfortunately, the process of correctly applying immutable updates to deeply nested state can easily become verbose and hard to read. Here's what an example of updating `state.first.second[someId].fourth` might look like:

```react
function updateVeryNestedField(state, action) {
    return {
        ...state,
        first : {
            ...state.first,
            second : {
                ...state.first.second,
                [action.someId] : {
                    ...state.first.second[action.someId],
                    fourth : action.someValue
                }
            }
        }
    }
}
```

Obviously, each layer of nesting makes this harder to read, and gives more chances to make mistakes. This is one of several reasons why you are encouraged to keep your state flattened, and compose reducers as much as possible.

#### Inserting and Removing Items in Arrays

Normally, a Javascript array's contents are modified using mutative functions like `push`, `unshift`, and `splice`. Since we don't want to mutate state directly in reducers, those should normally be avoided. Because of that, you might see "insert" or "remove" behavior written like this:

```react
function insertItem(array, action) {
    return [
        ...array.slice(0, action.index),
        action.item,
        ...array.slice(action.index)
    ]
}

function removeItem(array, action) {
    return [
        ...array.slice(0, action.index),
        ...array.slice(action.index + 1)
    ];
}
```

However, remember that the key is that the *original in-memory reference* is not modified. **As long as we make a copy first, we can safely mutate the copy**. Note that this is true for both arrays and objects, but nested values still must be updated using the same rules.

This means that we could also write the insert and remove functions like this:

```react
function insertItem(array, action) {
    let newArray = array.slice();
    newArray.splice(action.index, 0, action.item);
    return newArray;
}

function removeItem(array, action) {
    let newArray = array.slice();
    newArray.splice(action.index, 1);
    return newArray;
}
```

The remove function could also be implemented as:

```react
function removeItem(array, action) {
    return array.filter( (item, index) => index !== action.index);
}
```

#### Updating an Item in an Array

Updating one item in an array can be accomplished by using `Array.map`, returning a new value for the item we want to update, and returning the existing values for all other items:

```react
function updateObjectInArray(array, action) {
    return array.map( (item, index) => {
        if(index !== action.index) {
            // This isn't the item we care about - keep it as-is
            return item;
        }

        // Otherwise, this is the one we want - return an updated value
        return {
            ...item,
            ...action.item
        };    
    });
}
```

#### Immutable Update Utility Libraries

Because writing immutable update code can become tedious, there are a number of utility libraries that try to abstract out the process. These libraries vary in APIs and usage, but all try to provide a shorter and more succinct way of writing these updates. Some, like [dot-prop-immutable](https://github.com/debitoor/dot-prop-immutable), take string paths for commands:

```react
state = dotProp.set(state, `todos.${index}.complete`, true)
```

Others, like [immutability-helper](https://github.com/kolodny/immutability-helper) (a fork of the now-deprecated React Immutability Helpers addon), use nested values and helper functions:

```react
var collection = [1, 2, {a: [12, 17, 15]}];
var newCollection = update(collection, {2: {a: {$splice: [[1, 1, 13, 14]]}}});
```

They can provide a useful alternative to writing manual immutable update logic.



## Combining Reducers

We only have one reducer with redux and this is the case, all actions in the end get funneled through one reducer but redux, the package gives us a utility method we can use to combine multiple reducers into one, so that we still follow the pattern of having only one reducer behind the scenes.

We can split the reducers by feature.

```react
//reducers
//reducer/counter.js
const initialState = {
    counter: 0, //*** counter related
};
const counterReducer = (state = initialState, action) => {
  switch (action.type) {
    case "INCREMENT":
      return {
        ...state,
        counter: state.counter + 1,
      };
    //... 
  }
  return state;
};

export default counterReducer;

//reducer/result.js
const initialState = {
  results: [],
};

const resultReducer = (state = initialState, action) => {
  switch (action.type) {
    case "STORE_RESULT":
      return {
        ...state,
        results: state.results.concat({ id: new Date(), value: action.result }), //***
      };
    //...
  }
  return state;
};

export default resultReducer;
  
```

We can only access the local state of reducer and we will not be able to access the global state. So in the `STORE_RESULT` action we can't access `counter` as previously. Now we need to pass this value as part of the action.

While creating the store, now we will use the both the reducers.

```react
import {createStore, combineReducers} from 'redux';
import {Provider} from 'react-redux';
import counterReducer from './store/reducers/counter';
import resultReducer from './store/reducers/result';

const rootReducer = combineReducers({ //***
    ctr: counterReducer,
    res: resultReducer
});

const store = createStore(rootReducer);
ReactDOM.render(<Provider store={store}><App /></Provider>, document.getElementById('root'));
```

**combineReducers** is a function which takes an javascript object mapping our reducers to different slices of our state as input and merges everything into one state and one reducer for us.

So now while accessing in the containers we need use throught mapper name which we gave while combining the reducers to access their respective states.

```react
const mapStateToProps = (state) => {
    return {
        ctr: state.ctr.counter, //***
        storedResults: state.res.results //***
    }
}
```



## Middleware

We may add middleware in between action being dispatched and it reaching the reducer. Middleware basically is a term used for functions or the code general you hook into a process which then gets executed as part of that process without stopping it.

We can make use of Middleware if we want to execute asynchronus code or log.

![](C:\ravi\Training\Notes\React\images\redux_with_middleware_flow.PNG)



```react
import { createStore, combineReducers, applyMiddleware } from "redux";

const rootReducer = combineReducers({
  ctr: counterReducer,
  res: resultReducer,
});

const logger = (store) => { //***
  return (next) => {
    return (action) => {
      console.log("[Middleware]", action);
      const result = next(action);
      console.log("[Middleware] next state", store.getState());
      return result;
    };
  };
};

const store = createStore(rootReducer, applyMiddleware(logger)); //***
```

We can pass any number of middleware in **applyMiddleware**. We should make sure we return the action in the middleware.

#### Action Creators

An action creator is just a function which returns an action or which creates an action. We can make asynchronus calls before call the action in action creators.

We mostly need this for asynchronus call, it is the only place where we will make async call. For synchronus calls we can directly call the dispatch.

Use the action creators to get the data and pass it on to reducers. Reducers will be responsible for any data manipulation.

```react
//store/action/action.js
export const INCREMENT = 'INCREMENT';
export const DECREMENT = 'DECREMENT';

export const increment = () => {
    //here goes the asynchronus code if any
    return {
        type: INCREMENT
    };
};

//Counter.js
import * as actionCreators from '../../store/actions/actions';
const mapDispatchToProps = (dispatch) => {
    return {
        onIncrementCounter: () => dispatch(actionCreators.increment()),//***
        onDecrementCounter: () => dispatch({type: 'DECREMENT'}),
    }
}
export default connect(mapStateToProps, mapDispatchToProps)(Counter);

```



#### Redux Thunk

Redux Thunk is a library which allows you to mimic the async behavior. Middleware extends the store's abilities, and lets you write async logic that interacts with the store. 

Thunks are the recommended middleware for basic Redux side effects logic, including complex synchronous logic that needs access to the store, and simple async logic like AJAX requests

```react
//index.js
import { createStore, combineReducers, applyMiddleware, compose  } from "redux";
import thunk from 'redux-thunk';

const store = createStore(rootReducer, composeEnhancers(applyMiddleware(logger, thunk))); //***

//actions.js
const saveResult = res => {
    return {
        type: STORE_RESULT,
        result: res
    };
}
export const storeResult = (res) => {
    //async
    return (dispatch) => {
        setTimeout(() => {
          dispatch(saveResult(res));
        }, 2000);
      };
};



//Working example of action creators making call to server
export const fetchIngredientsFailed = () => {
  return {
    type: actionTypes.FETCH_INGREDIENTS_FAILED,
  };
};

export const setIngredients = (ingredients) => {
  return {
    type: actionTypes.SET_INGREDIENTS,
    ingredients: ingredients,
  };
};

export const initIngredients = () => {
  return (dispatch) => {
    axios
      .get("/ingredients.json")
      .then((response) => {
        dispatch(setIngredients(response.data));
      })
      .catch((error) => {
        dispatch(fetchIngredientsFailed());
      });
  };
};
```

We can make use **getState** in dispatch to access the previous state.

```react
export const storeResult = (res) => {
    //async
    return (dispatch, getState) => { //***
        const { counter } = getState(); //***
        setTimeout(() => {
          dispatch(saveResult(res));
        }, 2000);
      };
};
```



#### Restructuring actions

We can move the actions to separate files based on the feature. It is good for a large applications but it might be a overkill for smaller apps.

We can have single index file inside actions to expose all the actions. We can make use of **es6** feature.

```react
//index.js
// just the exports, it is the centralized exports for all the actions 
export {
    increment,
    decrement,
    add,
    subtract
} from './counter.js';

export {
    storeResult,
    deleteResult
} from './result.js';
```



## Debugging

We can install a chrome extension for debugging redux application.

https://chrome.google.com/webstore/detail/redux-devtools/lmhkpmbekcpmknklioeibfkpmmfibljd

We will need somethings to be configured in the app before we start using the debugger, so follow the git read me to set it up.

https://github.com/zalmoxisus/redux-devtools-extension

```react
import { createStore, combineReducers, applyMiddleware, compose  } from "redux";

const composeEnhancers = window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__ || compose; //***
const store = createStore(rootReducer, composeEnhancers(applyMiddleware(logger))); //***
```

