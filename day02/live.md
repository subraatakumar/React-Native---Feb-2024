## Custom Button Example
```js
import {
  View,
  TouchableOpacity,
  Text,
  Pressable,
  StyleSheet,
} from 'react-native';
import { useState } from 'react';
const App = () => {
  const [backgroundColor, setBackgroundColor] = useState('red');
  const handleClick = () => {
    console.log('Button is clicked');
  };

  return (
    <View>
      <CustomButton
        pressedColor="cyan"
        btnColor="magenta"
        handleClick={handleClick}
      />
      <CustomButton border handleClick={handleClick} />
      <CustomButton
        pressedColor="magenta"
        btnColor="darkgrey"
        handleClick={handleClick}
      />
    </View>
  );
};

export default App;

const CustomButton = (props) => {
  const { border, handleClick, pressedColor, btnColor } = props;
  return (
    <Pressable
      style={({ pressed }) => [
        styles.btn,
        {
          backgroundColor: pressed
            ? pressedColor || 'red'
            : btnColor || 'green',
        },
        border ? styles.borderYellow : {},
      ]}
      onPress={handleClick}>
      <Text>Ok</Text>
    </Pressable>
  );
};

const styles = StyleSheet.create({
  btn: {
    padding: 10,
    borderRadius: 20,
    alignItems: 'center',
  },
  borderYellow: {
    borderWidth: 2,
    borderColor: 'yellow',
  },
});
```
