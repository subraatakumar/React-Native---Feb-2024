# Square to Circle Without using Animate API

```js
import React, { useState } from 'react';
import { View, TouchableOpacity, Text } from 'react-native';

const App = () => {
  const [isCircle, setIsCircle] = useState(false)
  return (
    <View>
    {isCircle ? 
      <View
        style={{
          width: 100,
          height: 100,
          borderRadius: 50,
          backgroundColor: 'green',
        }}></View>:
      <View
        style={{
          width: 100,
          height: 100,
          borderRadius: 0,
          backgroundColor: 'green',
        }}></View>}
      <TouchableOpacity onPress={() => {setIsCircle(prev => !prev)}}>
        <Text>Change</Text>
      </TouchableOpacity>
    </View>
  );
};

export default App;

```
