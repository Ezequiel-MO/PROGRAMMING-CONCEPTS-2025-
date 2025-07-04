
# REDUX vs RTK (Redux ToolKit)

## REDUX EXAMPLE - MIDDLEWARE - ASYNC ACTIONS

```typescript
import redux from 'redux'
import axios from 'axios'
import thunk from 'redux-thunk'


// State
const initialState = {
    loading : false,
    users: [],
    error : ""
}

// Typed Actions

const FETCH_USERS_REQUESTED = 'FETCH_USERS_REQUESTED'
const FETCH_USERS_SUCCEEDED = 'FETCH_USERS_SUCCEEDED'
const FETCH_USERS_FAILED = 'FETCH_USERS_FAILED'

// Action creators

const fetchUsersRequest = () ={
    type: FETCH_USERS_REQUESTED
}

const fetchUsersSuccess = (users) =>{
    type: FETCH_USERS_SUCCEEDED,
    payload: users
}

const fetchUsersFailure = (error) =>{
    return {
        type: FETCH_USERS_FAILED,
        payload : error
    }
}

//reducer function

const reducer = (state = initialState, action) => {
    switch(action.type){
        case FETCH_USERS_REQUESTED: 
        return {
            ...state,
            loading: true
        }
        case FETCH_USERS_SUCCEEDED:
            return {
                loading: false,
                users: action.payload
                error: ''
            }
        case FETCH_USERS_FAILED: 
            return {
                loading: false,
                users: [],
                error: action.payload
            }
    }
}

//Create an asynchronous action creator. 
//The async action creator is allowed to have side effects - does not have to be pure
//The async action creator returns another function instead of an action object like the regular ones
//The async action creator can dispatch regular actions inside the function
const fetchUsers = () => {
    return function(dispatch){
        dispatch(fetchUsersRequest())
        axios.get('https://jsonplaceholder.typicode.com/users').then((response)=>{
        const users = response.data.map((user)=> user.id)
        dispatch(fetchUsersSuccess(users))
        }).catch(error){
            //error.message
            dispatch(fetchUsersFailure(error.message))
        }
    }
}

// Create redux store
const store = redux.createStore(reducer, redux.applyMiddleware(thunk))
store.subscribe(()=>console.log(store.getState()))
store.dispatch(fetchUsers())

```