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

## Gesture Handler Basic Example

```js
import { Dimensions, View, Text, StyleSheet, Image } from 'react-native';
import {
  Gesture,
  GestureDetector,
  GestureHandlerRootView,
} from 'react-native-gesture-handler';
import Animated, {
  interpolate,
  useAnimatedStyle,
  useSharedValue,
  withSpring,
} from 'react-native-reanimated';

const { width: SCREEN_WIDTH } = Dimensions.get('window');

const App = () => {
  return (
    <View style={{ flex: 1, marginTop: 50 }}>
      <BottomSheet style={{ height: 152, marginVertical: 10 }}>
        <Image
          source={{
            uri: 'https://cdn.pixabay.com/photo/2023/03/05/12/41/mountains-7831286_960_720.jpg',
          }}
          style={{ width: '100%', height: 150 }}
        />
      </BottomSheet>
      <BottomSheet />
      <BottomSheet />
      <BottomSheet />
    </View>
  );
};

export default App;

const BottomSheet = (props) => {
  const translateX = useSharedValue(0);
  const prevPosition = useSharedValue({ x: 0 });

  const gesture = Gesture.Pan()
    .onStart(() => {
      prevPosition.value = { x: translateX.value };
    })
    .onUpdate((event) => {
      translateX.value = Math.max(
        event.translationX + prevPosition.value.x,
        -SCREEN_WIDTH
      );
    })
    .onEnd(() => {
      translateX.value = withSpring(0, { damping: 40 });
    });

  const btmSheetAnimStyle = useAnimatedStyle(() => {
    return {
      transform: [{ translateX: translateX.value }],
    };
  });

  return (
    <GestureHandlerRootView style={{ flex: 1 }}>
      <GestureDetector gesture={gesture}>
        <Animated.View
          style={[styles.btmSheet, btmSheetAnimStyle, props.style]}>
          {props.children}
        </Animated.View>
      </GestureDetector>
    </GestureHandlerRootView>
  );
};

const styles = StyleSheet.create({
  btmSheet: {
    height: 50,
    width: '100%',
    backgroundColor: 'red',
    borderRadius: 5,
    borderWidth: 0.5,
  },
});
```

## Reanimated Bottom Sheet

```js
// BottomSheet.js
import React, { useEffect } from 'react';
import { Dimensions, View, Text, StyleSheet } from 'react-native';
import { Gesture, GestureDetector,GestureHandlerRootView } from 'react-native-gesture-handler';
import Animated, {
  interpolate,
  useAnimatedStyle,
  useSharedValue,
  withSpring,
} from 'react-native-reanimated';

const { height: SCREEN_HEIGHT, width } = Dimensions.get('window');

const BottomSheet = () => {
  const translateY = useSharedValue(0);
  const prevPosition = useSharedValue({ y: 0 });

  const gesture = Gesture.Pan()
    .onStart(() => {
      prevPosition.value = { y: translateY.value };
    })
    .onUpdate((event) => {
      translateY.value = Math.max(
        event.translationY + prevPosition.value.y,
        -SCREEN_HEIGHT
      );
    })
    .onEnd(() => {
      if (translateY.value > -SCREEN_HEIGHT / 2) {
        translateY.value = withSpring(-35, { damping: 50 });
      } else if (translateY.value < -SCREEN_HEIGHT / 3) {
        translateY.value = withSpring(-SCREEN_HEIGHT, { damping: 50 });
      }
    });

  useEffect(() => {
    translateY.value = withSpring(-35, { damping: 50 });
  }, []);
  const btmSheetAnimStyle = useAnimatedStyle(() => {
    return {
      transform: [{ translateY: translateY.value }],
      borderRadius: interpolate(
        translateY.value,
        [-SCREEN_HEIGHT, -SCREEN_HEIGHT + 50, 0],
        [2, 25, 25]
      ),
    };
  });

  return (
    <GestureHandlerRootView style={{ flex: 1, backgroundColor:'blue' }}>
      <GestureDetector gesture={gesture}>
        <Animated.View style={[styles.btmSheet, btmSheetAnimStyle]}>
          <View style={styles.line}></View>
          <Text>BottomSheet</Text>
        </Animated.View>
      </GestureDetector>
    </GestureHandlerRootView>
  );
};

export default BottomSheet;

const styles = StyleSheet.create({
  btmSheet: {
    height: SCREEN_HEIGHT,
    width: '100%',
    backgroundColor: '#FFF',
    top: SCREEN_HEIGHT,
    borderRadius: 25,
  },
  line: {
    width: 150,
    height: 2,
    backgroundColor: 'grey',
    alignSelf: 'center',
    marginVertical: 15,
    borderRadius: 2,
  },
});
```
