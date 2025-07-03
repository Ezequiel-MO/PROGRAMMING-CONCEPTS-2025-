

# REDUX vs RTK (Redux ToolKit)

## REDUX EXAMPLE 

```typescript
//This example can be replicated in a nodejs playground, by kicking off a nodejs project, and installing 'redux' library
import redux from "redux"

//typing strings
const INCREMENT_ONE = 'INCREMENT_ONE'
const INCREMENT_MULTIPLE = 'INCREMENT_MULTIPLE'

//action creator - does not require a payload
function incrementOne(){
    return {
        type: INCREMENT_ONE,
        payload: 1
    }
}

//action creator with a payload
function incrementMultiple(qty){
    return {
        type: INCREMENT_MULTIPLE,
        payload: qty
    }
}

//initial state - is always an object, with at least one key:value pair
const initialState = {
    numberOfItems: 20
}

//Reducer are pure functions that accept previous state and actions, and return an updated state depending on the action type
const reducer = (state = initialState, action) =>{
    switch (action.type){
        case : INCREMENT_ONE : {
            return {
                ...state,
                numberOfItems: state.numberOfItems + 1
            }
        }
        case : INCREMENT_MULTIPLE : {
            return {
                ...state,
                numberOfItems: state.numberOfItems + action.payload
            }
        }
        default : 
            return state
    }
}

//Create a store to centralize state
const store = redux.createStore(reducer)

//Access state
//This console.log should give us the initial state since no transitions have been dispatched yet. 
//The state of numberOfItems will be 20
console.log('Initial State', store.getState())

//Listener that allows the app to subscribe to changes in state

store.subscribe(()=> console.log( 'updated state' , store.getState()))

//Dispatch to update state - using the action creator
store.dispatch(incrementOne())
//The state of numberOfItems will be 21
//I can dispatch the same action creator multiple times
store.dispatch(incrementOne())
//The state of numberOfItems will be 22
store.dispatch(incrementMultiple(3))
//The state of numberOfItems will be 25


//To unsubscribe the app from state updates, we need to call the subscribe method
const unsubscribe = store.subscribe(()=> console.log('updated state' , store.getState()))
unsubscribe()

//COMMENT : Manually subscribing and unsubscribing to the store is only required in REDUX.
//COMMENT : RTK simplifies this and manages subscribing and unsubscribing under the hood when using the useSelector and useDispatch custom hooks provided by the library


