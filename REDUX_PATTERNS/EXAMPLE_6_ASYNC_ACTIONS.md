
# REDUX vs RTK (Redux ToolKit)

## REDUX EXAMPLE - MIDDLEWARE - ASYNC ACTIONS

```typescript
import redux from 'redux'


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

// Create redux store
const store = redux.createStore(reducer)

```