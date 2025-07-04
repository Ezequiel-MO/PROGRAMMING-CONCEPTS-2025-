
# REDUX vs RTK (Redux ToolKit)

## REACT REDUX TOOLKIT EXAMPLE

   - Using Redux has some concerns
     * It required too much boilerplate code : actions, action objects, action creators, switch statements in the reducer
     * Many packages need to be installed to work with redux : Redux-thunk, Immer, Redux-devtools, etc...
     There was a need to improve the developer experience for Redux

## SAME EXAMPLE BUT DEVELOPED WITH RTK INSTEAD OF PLAIN REDUX
     * RTK is opinionated on the folder structure of our application

### DIFFERENCE IN BEHAVIOUR
    * In the case of redux, for example "cakeReducer" can respond to actions related to cakes, but it can also modify the state by receiving an action related to icecream
    for example : 

    ```typescript
    const icecreamReducer = (state= initialIceCreamState, action) => {
    switch (action.type){
        case ICECREAM_ORDERED:
            return {
                ...state,
                numberOfIceCreams: state.numberOfIceCreams -1
            }
        case ICECREAM_RESTOKED:
            return {
                ...state,
                numberOfIceCreams: state.numberOfIceCreams + action.payload
            }
        // we can add a 3rd case , - in which the action updates the numberOf
        case CAKE_ORDER:
            return {
                ...state,
                numberOfIcecreams: state.numberOficeCreams - 1
            }
        default:
            return state
    }
}
    ```

    So, this would be the case of the shoopkeeper giving away 1 ice-cream each time a customer orders a cake.

    * In the case of RTK - If we want a particular slice to update its state obeying actions outside of the specified slice reducers, then we need to add "EXTRA REDUCERS"
    
    COMMENT : This use case is very common when multiple slices need to update their state in response to a single action

### FOLDERS

#### index.js

```typescript
import store from "./app/store"
import {cakeActions} from "./features/cake/cakeSlice"
import {icecreamActions} from "./features/icecream/icecreamSlice"

const unsubscribe = store.subscribe(()=>{})

store.dispatch(cakeActions.ordered())
store.dispatch(cakeActions.ordered())
store.dispatch(cakeActions.restoked())

store.dispatch(icecreamActions.ordered())
store.dispatch(icecreamActions.ordered())
store.dispatch(icecreamActions.restoked()

unsubscribe()
```

#### app

##### store.js

```typescript
import {configureStore} from '@reduxjs/toolkit'
import reduxLogger from 'redux-logger'
import cakeReducer from "./features/cake/cakeSlice"
import icecreamReducer from "./features/icecream/icecreamSlice"

const logger = reduxLogger.createLogger()

const store = configureStore({
    // In RTK it is not necessary to combine the reducers in a combineReducers function. You can pass them directly to the reducer key.
    reducer: {
        cake: cakeReducer,
        icecream: icecreamReducer
    }, 
    middleware : (getDefaultMiddleware) => getDefaultMiddleware().concat(logger)
})

export default store
```

#### features

##### cake

###### cakeSlice.js

```typescript
import {createSlice} from '@reduxjs/toolkit'

const initialState = {
    numOfCakes: 10
}

// createSlice uses Immer under the hood - that is why
const cakeSlice = createSlice({
    name: 'cake',
    initialState,
    //now, ordered and restoked will be the actions creators. They are named automatically
    reducers: {
        ordered: (state)=> {
            state.numOfCakes--
        },
        restocked: (state,action) =>{
            state.numOfCakes += action.payload
        }
    }
})
```

###### iceCreamSlice.js

```typescript
import {createSlice} from '@reduxjs/toolkit'

const initialState = {
    numOfIcecreams: 20
}

// createSlice uses Immer under the hood - that is why
const icecreamSlice = createSlice({
    name: 'icecream',
    initialState,
    //now, ordered and restoked will be the actions creators. They are named automatically
    reducers: {
        ordered: (state)=> {
            state.numOfIcecreams--
        },
        restocked: (state,action) =>{
            state.numOfIcecreams += action.payload
        }
    },
    // action of cakes working in the icecreamSlice
    // Aproach 1
    /* extraReducers : {
        ['cake/ordered'] : (state) =>{
            state.numberOfIcecreams--
        }
    } */ 
   // Aproach 2 - recommended
   extraReducers : (builder)=>{
    builder.addCase(cakeActions.ordered, state =>{
            state.numberOfIcecreams--
        })
   }
})

export default icecreamSlice.reducer
export icecreamSlice.actions

```

