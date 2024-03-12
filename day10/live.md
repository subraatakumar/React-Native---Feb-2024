


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


