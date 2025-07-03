

# REDUX vs RTK (Redux ToolKit)

## REDUX EXAMPLE - IMMER LIBRARY

```typescript
//In this example, we will reproduce a case in which the state is deeply nested
import redux from "redux"
import {produce} from "immer"

//typing strings
const UPDATE_STREET = 'UPDATE_STREET'

//action creator - does not require a payload
const updateStreet = (street) => {
    return {
        type: UPDATE_STREET,
        payload : street
    }
}


//initial state - is always an object, with at least one key:value pair
const initialState = {
    name : "Hotel Radisson Copenhaguen,
    address : {
        street : "Lagenshtten 3",
        city: "Copenhaguen",
        country: "Denmark"
    }
}

//Reducer are pure functions that accept previous state and actions, and return an updated state depending on the action type
const reducer = (state = initialState, action) =>{
    switch (action.type){
        case : UPDATE_STREET : {
           /*  return {
                ...state,
                address: {
                    ...state.address,
                    street : action.payload
                }
            }
        } */
       return produce(state, (draft)=> draft.address.street = action.payload)
       
        default : 
            return state
    }
}

//Create a store to centralize state
const store = redux.createStore(reducer)

//Access state
//This console.log should give us the initial state since no transitions have been dispatched yet. 
console.log('Initial State', store.getState())

//Listener that allows the app to subscribe to changes in state

store.subscribe(()=> console.log( 'updated state' , store.getState()))

//Dispatch to update state - using the action creator
store.dispatch(updateStreet("Doner strasse 4"))



//To unsubscribe the app from state updates, we need to call the subscribe method
const unsubscribe = store.subscribe(()=> console.log('updated state' , store.getState()))
unsubscribe()

//COMMENT : Manually subscribing and unsubscribing to the store is only required in REDUX.
//COMMENT : RTK simplifies this and manages subscribing and unsubscribing under the hood when using the useSelector and useDispatch custom hooks provided by the library
```