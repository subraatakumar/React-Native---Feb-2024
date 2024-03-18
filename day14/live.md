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

# using Animated API

```js
import React, { useRef } from 'react';
import { View, TouchableOpacity, Text, Animated } from 'react-native';

const App = () => {
  const br = useRef(new Animated.Value(50)).current;
  return (
    <View>
      <Animated.View
        style={{
          width: 100,
          height: 100,
          borderRadius: br,
          backgroundColor: 'green',
        }}></Animated.View>

      <TouchableOpacity
        onPress={() => {
          Animated.spring(br, { toValue: br._value == 0 ? 50 : 0 }).start();
        }}>
        <Text>Change</Text>
      </TouchableOpacity>
    </View>
  );
};

export default App;
```
