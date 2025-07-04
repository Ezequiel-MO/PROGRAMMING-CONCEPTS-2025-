# REDUX vs RTK (Redux ToolKit)

## REDUX TOOLKIT WITH ASYNC FUNCTIONS


#### index.js

```typescript
import store from "./store"
import {fetchUsers} from "./features/user/userSlice"


cons unsubscribe = store.subscribe(()=> console.log('Updated state' , store.getState()))

store.dispatch(fetchUsers())

```



#### store

```typescript
import {configureStore} from "@reduxjs/toolkit"
import cakeReducer from "./features/cake/cakeSlice"
import icecreamReducer from "./features/icecream/icecreamSlice"
import userReducer from "./features/user/userSlice"

const store = configureStore({
    reducer: {
        cake: cakeReducer,
        icecream: icecreamReducer,
        user: userReducer
    }
})


export default store
 
```

#### features
##### user
###### userSlice.js

```typescript
import {createSlice, createAsyncThunk} from "@reduxjs/toolkit"
import axios from 'axios'

// State
const initialState = {
    loading : false,
    users: [],
    error : ""
}

// Generates pending, fulfilled and rejected action types
const fetchUsers = createAsyncThunk('user/fetchUsers', ()=> {
    return axios.get('https://jsonplaceholder.typicode.com/users').
    then((response)=>response.data.map((user)=> user.id))
})

const userSlice = createSlice({
    name: 'user',
    initialState,
    extraReducers: (builder=>{
        builder.addCase(fetchUsers.pending, state =>{
            state.loading = true
        })
        builder.addCase(fetchUsers.fulfilled, state =>{
            state.loading = false
            state.users = action.payload
            state.error = ""

        })
        builder.addCase(fetchUsers.rejected, state => {
            state.loading = false
            state.users = []
            state.error = action.error.message
        })
    })
})

export userSlice.reducer
export userSlice.fetchUsers

```