# Task List part 1

- User should have the ability to add task to the list

```js
npx react-native init SqlExample
cd SqlExample
npx install-expo-modules@latest
yarn add expo-sqlite
yarn add @expo/vector-icons
```

```js
import {openDatabase} from 'expo-sqlite';
import React, {useEffect, useState} from 'react';
import {
  StyleSheet,
  Text,
  TextInput,
  TouchableOpacity,
  View,
} from 'react-native';
import AntDesign from '@expo/vector-icons/AntDesign';

const color = 'rgb(20,20,20)';

const db = openDatabase('subrat.db');

// db.transaction(txn => {
//   txn.executeSql(query, params, successfully(), error())
// })

export function ExecuteSql(db, query, params = []) {
  return new Promise((resolve, reject) => {
    db.transaction(txn => {
      txn.executeSql(
        query,
        params,
        (tx, res) => resolve(res),
        e => reject(e),
      );
    });
  });
}

export default function App() {
  const [todos, setTodos] = useState([]);
  const [val, setVal] = useState('');

  const pullDataFromTable = () => {
    ExecuteSql(db, 'SELECT * FROM todotable').then(res => {
      //console.log('Res', res);
      const {
        rows: {_array},
      } = res;
      //console.log('rows:', _array);
      if (_array) setTodos(_array);
    });
  };

  useEffect(() => {
    ExecuteSql(
      db,
      `CREATE TABLE IF NOT EXISTS todotable(id INTEGER PRIMARY KEY AUTOINCREMENT, task VARCHAR(20), status INTEGER(1))`,
    )
      .then(t => console.log(t))
      .catch(e => console.log('Error creating table'));
    pullDataFromTable();
  }, []);

  const addTask = () => {
    ExecuteSql(db, 'INSERT INTO todotable (task, status) VALUES (?,?)', [
      val,
      0,
    ])
      .then(async res => {
        console.log(res, `Inserted :${res.insertId}`);
        let data = await ExecuteSql(
          db,
          `SELECT * FROM todotable WHERE id=${res.insertId}`,
        );
        setTodos(prev => [...prev, data.rows.item(0)]);
        setVal('');
      })
      .catch(e => {
        console.log('Error:', e);
      });
    //setTodos(prev => [...prev,{id: Math.random() , task:val, status:0}])
  };

  return (
    <View style={styles.container}>
      <Text style={styles.title}>SQLite Task List</Text>
      <View style={styles.inputContainer}>
        <TextInput
          style={[styles.input, styles.border]}
          value={val}
          onChangeText={t => setVal(t)}
        />
        <TouchableOpacity
          style={[styles.caret, styles.border]}
          onPress={addTask}>
          <AntDesign name="caretright" color={color} size={32} />
        </TouchableOpacity>
      </View>
      {todos.map(todo => (
        <SingleTask todo={todo} key={todo.id} />
      ))}
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#fff',
    marginTop: 25,
  },
  title: {
    fontSize: 25,
    textAlign: 'center',
  },
  border: {
    borderWidth: 1,
    borderColor: color,
    borderRadius: 5,
  },
  todo: {
    padding: 5,
    margin: 5,
  },
  input: {
    flex: 1,
    margin: 5,
    padding: 3,
  },
  inputContainer: {
    flexDirection: 'row',
    alignItems: 'center',
  },
  caret: {
    marginRight: 5,
  },
});

const SingleTask = ({todo}) => {
  return (
    <View>
      <Text style={[styles.todo, styles.border]}>{todo.task}</Text>
    </View>
  );
};
```    



# Persisted Redux Toolkit

```js
// counterSlice.js
import { createSlice } from "@reduxjs/toolkit";

const initialState = {
  value: 0,
};

export const counterSlice = createSlice({
  name: "counter",
  initialState,
  reducers: {
    increment: (state) => {
      // Redux Toolkit allows us to write "mutating" logic in reducers. It
      // doesn't actually mutate the state because it uses the Immer library,
      // which detects changes to a "draft state" and produces a brand new
      // immutable state based off those changes
      state.value += 1;
    },
    decrement: (state) => {
      state.value -= 1;
    },
    incrementByAmount: (state, action) => {
      state.value += action.payload;
    },
  },
});

// Action creators are generated for each case reducer function
export const { increment, decrement, incrementByAmount } = counterSlice.actions;

export default counterSlice.reducer;
```

```js
// store.js
import { combineReducers, configureStore } from "@reduxjs/toolkit";
import AsyncStorage from "@react-native-async-storage/async-storage";
import { persistStore, persistReducer } from "redux-persist";
import counterSlice from "./counterSlice";

const reducers = combineReducers({
  counter: counterSlice,
});

const persistConfig = {
  key: "root",
  storage: AsyncStorage,
  blacklist: ["dimen"],
  version: 1,
};

const persistedReducer = persistReducer(persistConfig, reducers);

export const store = configureStore({
  reducer: persistedReducer,
  middleware: (getDefaultMiddleware) =>
    getDefaultMiddleware({
      serializableCheck: false,
    }),
});

export const persistor = persistStore(store);
```

```js
// index.js
/**
 * @format
 */

import { AppRegistry } from "react-native";
import App from "./App1";
import { name as appName } from "./app.json";
import { store, persistor } from "./src/redux/store";
import { Provider } from "react-redux";
import { PersistGate } from "redux-persist/integration/react";

const Main = () => {
  return (
    <Provider store={store}>
      <PersistGate loading={null} persistor={persistor}>
        <App />
      </PersistGate>
    </Provider>
  );
};
AppRegistry.registerComponent(appName, () => Main);
```

```js
// App.js
import React from 'react';
import {useSelector, useDispatch} from 'react-redux';
import {increment, decrement} from './src/redux/counterSlice';
import {Button, View, Text, TouchableOpacity} from 'react-native';

function App1() {
  const {value} = useSelector(state => state.counter);
  const dispatch = useDispatch();

  return (
    <View>
      <View style={{marginTop: 40}}>
        <Button onPress={() => dispatch(increment())} title="Increment " />
        <Text style={{fontSize: 25, textAlign: 'center'}}>{value}</Text>
        <Button onPress={() => dispatch(decrement())} title="Decrement " />
      </View>
    </View>
  );
}

export default App1;
```


