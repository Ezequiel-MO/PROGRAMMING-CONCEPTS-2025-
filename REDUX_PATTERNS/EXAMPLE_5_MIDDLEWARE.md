
# REDUX vs RTK (Redux ToolKit)

## REDUX EXAMPLE - MIDDLEWARE - LOGGER

```typescript
//Let's create an example, where we need to handle 2 different states : for example : in a shop, we may need to keep track of shoes and hoodies
import redux from 'redux'
import reduxLogger from 'redux-logger'
const createStore = redux.createStore
const combineReducers = redux.combineReducers
const applyMiddleware = redux.applyMiddleware

const logger = reduxLogger.createLogger()


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
const initialShoesState= {
    numOfShoes: 10
}

const initialHoodiesState = {
    numOfHoodies: 20
}

// Reducer 1
const shoesReducer = (state= initialShoesState, action) => {
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
        default:
            return state
    }
}
// Reducer 2
const hoodiesReducer = (state= initialHoodiesState, action) => {
    switch (action.type){
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

//Combining reducers
const rootReducer = combineReducers({
    shoes: shoesReducer,
    hoodies: hoodiesReducer
})

//create the store
const store = createStore(rootReducer, applyMiddleware(logger))

store.dispatch(orderShoes())
store.dispatch(restokeShoes(3))
store.dispatch(orderHoodies())
store.dispatch(restokeHoodies(3))

const unsubscribe = store.subscribe(()=> {})

```