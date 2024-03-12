# SQL Lite Storage on React Native

React Native SQLite is a library that implements a self-contained, serverless, zero-configuration SQL database engine. SQLite is the most widely deployed SQL database engine in the world. The source code for SQLite is in the public domain.

## Installation

```js
npm install --save react-native-sqlite-storage
```

## Use Database of SQLite

```js
import {openDatabase} from 'react-native-sqlite-storage';

...

export const db = openDatabase({name: 'mydatabase1.db'},
  () => { },
  error => {
    console.log("ERROR: " + error);
  });
```


## Basic CRUD operation on SQLite

We can use a custom ExecuteSql function which will accept db, query string and optional parameters.

```js
export function ExecuteSql(db, query: string, params: any[] = []) {
  return new Promise((resolve, reject) => {
    db.transaction((txn) => {
      txn.executeSql(
        query,
        params,
        (tx, res) => resolve(res),
        (e) => reject(e)
      );
    });
  });
}
```

### Create Table

```js
  // Create Table
  async CreateTodoTable() {
    let Table = await ExecuteSql(
            db,
            `CREATE TABLE todotable(id INTEGER PRIMARY KEY AUTOINCREMENT, task VARCHAR(20), status INTEGER(1))`,
          );
    console.log(Table);
  }
```

### Delete Table

```js
const deleteTable = async (tblName) => {
  await ExecuteSql(db, `DROP TABLE IF EXISTS ${tblName}`);
};
```

### Insert data into table

```js
ExecuteSql(db, "INSERT INTO todotable (task, status) VALUES (?,?)", [
  "new Task",
  0,
])
  .then(async (res) => {
    dbSuccess(res, `Inserted :${res.insertId}`);
    let data = await ExecuteSql(
      db,
      `SELECT * FROM ${tblName} WHERE id=${res.insertId}`
    );
    setTodos((prev) => [...prev, data.rows.item(0)]);
  })
  .catch((e) => {
    dbError(e);
  });
```

### Update Data

```js
await ExecuteSql(db, `UPDATE ${tblName} SET task= ? WHERE id=?`, [
  todoText,
  id,
]);
```

### Delete Data

```js
await ExecuteSql(db, `DELETE FROM ${tblName} WHERE id=${todo.id}`);
```

[Practice](https://www.sql-practice.com/)

# [Redux Toolkit](https://redux-toolkit.js.org/tutorials/quick-start)

- Install Redux Toolkit and React-Redux

```js
npm install @reduxjs/toolkit react-redux
```

- Create a Redux Store

```js
// app/store.js
import { configureStore } from '@reduxjs/toolkit'

export const store = configureStore({
  reducer: {},
})
```

- Provide the Redux Store to React

```js
index.js
import React from 'react'
import ReactDOM from 'react-dom'
import './index.css'
import App from './App'
import { store } from './app/store'
import { Provider } from 'react-redux'

ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById('root')
)
```

- Create a Redux State Slice

```js
features/counter/counterSlice.js
import { createSlice } from '@reduxjs/toolkit'

const initialState = {
  value: 0,
}

export const counterSlice = createSlice({
  name: 'counter',
  initialState,
  reducers: {
    increment: (state) => {
      // Redux Toolkit allows us to write "mutating" logic in reducers. It
      // doesn't actually mutate the state because it uses the Immer library,
      // which detects changes to a "draft state" and produces a brand new
      // immutable state based off those changes
      state.value += 1
    },
    decrement: (state) => {
      state.value -= 1
    },
    incrementByAmount: (state, action) => {
      state.value += action.payload
    },
  },
})

// Action creators are generated for each case reducer function
export const { increment, decrement, incrementByAmount } = counterSlice.actions

export default counterSlice.reducer
```

- Add Slice Reducers to the Store

```js
// app/store.js
import { configureStore } from '@reduxjs/toolkit'
import counterReducer from '../features/counter/counterSlice'

export const store = configureStore({
  reducer: {
    counter: counterReducer,
  },
})
```

- Use Redux State and Actions in React Components

```js
import React from 'react'
import { useSelector, useDispatch } from 'react-redux'
import { decrement, increment } from './counterSlice'

export function Counter() {
  const count = useSelector((state) => state.counter.value)
  const dispatch = useDispatch()

  return (
    <div>
      <div>
        <button
          aria-label="Increment value"
          onClick={() => dispatch(increment())}
        >
          Increment
        </button>
        <span>{count}</span>
        <button
          aria-label="Decrement value"
          onClick={() => dispatch(decrement())}
        >
          Decrement
        </button>
      </div>
    </div>
  )
}
```

Now, any time you click the "Increment" and "Decrement" buttons:

- The corresponding Redux action will be dispatched to the store
- The counter slice reducer will see the actions and update its state
- The <Counter> component will see the new state value from the store and re-render itself with the new data.

## SUMMARY
- Create a Redux store with configureStore
  - configureStore accepts a reducer function as a named argument
  - configureStore automatically sets up the store with good default settings
- Provide the Redux store to the React application components
  - Put a React-Redux <Provider> component around your <App />
  - Pass the Redux store as <Provider store={store}>
- Create a Redux "slice" reducer with createSlice
  - Call createSlice with a string name, an initial state, and named reducer functions
  - Reducer functions may "mutate" the state using Immer
  - Export the generated slice reducer and action creators
- Use the React-Redux useSelector/useDispatch hooks in React components
  - Read data from the store with the useSelector hook
  - Get the dispatch function with the useDispatch hook, and dispatch actions as needed

# Persist Redux Store

```js
yarn add @react-native-async-storage/async-storage redux-persist
```

```js
// app/store.js
import { combineReducers, configureStore } from '@reduxjs/toolkit'
import AsyncStorage from '@react-native-async-storage/async-storage';
import {persistStore, persistReducer} from 'redux-persist';
import counterReducer from '../features/counter/counterSlice'

const persistConfig = {
  key: 'root',
  storage: AsyncStorage,
  blacklist: ['dimen'],
  version: 1,
};

const reducers = combineReducers({
  counter: counterReducer,
});

const persistedReducer = persistReducer(persistConfig, reducers);

export const store = configureStore({
  reducer: persistedReducer,
})

export const persistor = persistStore(store);
```


```js
// index.js
import {AppRegistry} from 'react-native';
import App from './App';
import {name as appName} from './app.json';
import {Provider as StoreProvider} from 'react-redux';
import {store, persistor} from './src/redux/store';
import {PersistGate} from 'redux-persist/integration/react';

const Main = () => {
  return (
    <StoreProvider store={store}>
      <PersistGate loading={null} persistor={persistor}>
        <App />
      </PersistGate>
    </StoreProvider>
  );
};

AppRegistry.registerComponent(appName, () => Main);
```
