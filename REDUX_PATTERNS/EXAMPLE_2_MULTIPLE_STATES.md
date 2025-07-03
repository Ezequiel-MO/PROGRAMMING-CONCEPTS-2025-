
# REDUX vs RTK (Redux ToolKit)

## REDUX EXAMPLE - MULTIPLE REDUCERS

```typescript
//Let's create an example, where we need to handle 2 different states : for example : in a shop, we may need to keep track of shoes and hoodies
import redux from 'redux'
const createStore = redux.createStore

//let's type the actions
const SHOES_ORDERED = 'SHOES_ORDERED'
const SHOES_RESTOKED = 'SHOES_RESTOKED'
const HOODIES_ORDERED = 'HOODIES_ORDERED'
const HOODIES_RESTOCKED = 'HOODIES_RESTOKED'

//Now, let's type the action creators
function orderShoes(){
    return {
        type: SHOES_ORDERED,
        payload: 1
    }
}


function restokeShoes(qty=1){
    return {
        type: SHOES_RESTOKED,
        payload: qty
    }
}

function orderHoodies(){
    return {
        type: HOODIES_ORDERED,
        payload: 1
    }
}

function restokeHoodies(qty=1){
    return {
        type: HOODIES_RESTOKED,
        payload: qty
    }
}

// Initial state object
const initialState = {
    numOfShoes: 10,
    numOfHoodies: 20
}

// Reducer
const reducer = (state= initialState, action) => {
    switch (action.type){
        case SHOES_ORDERED:
            return {
                ...state,
                numOfShoes: state.numOfShoes -1
            }
        case SHOES_RESTOKED:
            return {
                ...state,
                numOfShoes: state.numOfShoes + action.payload
            }
        case HOODIES_ORDERED:
            return {
                ...state,
                numOfHoodies: state.numOfHoodies. -1
            }
        case SHOES_RESTOKED:
            return {
                ...state,
                numOfHoodies: state.numOfHoodies + action.payload
            }
        default:
            return state
    }
}

const store = createStore(reducer)

store.dispatch(orderShoes())
store.dispatch(restokeShoes(3))
store.dispatch(orderHoodies())
store.dispatch(restokeHoodies(3))

const unsubscribe = store.subscribe(()=> console.log('Update state', store.getState())

```