
## App with touchable Button

```js
import { Text, View, TouchableOpacity, StyleSheet } from "react-native";

const App = () => {
  const x = [
    "ramesh",
    "suersh",
    "mahesh",
    "ganesh",
    "yukti",
    "sukhi",
    "thiland",
    "one",
    "two",
    "three",
    "four",
    "five"
  ];
  const clickedItems = []

  return (
    <View>
    <View style={{ flexDirection: "row", flexWrap:"wrap", marginTop:30,  }}>
      {x.map((y) => (
        <TouchableOpacity onPress={()=> clickedItems.push(y)}>
        <Text style={{ borderRadius: 10, borderWidth: 0.5, padding: 5, margin:5 }}>
          {y}
        </Text>
        </TouchableOpacity>
      ))}

    </View>
      <View style={{alignItems:'center' }}>
      <TouchableOpacity onPress={()=>console.log(clickedItems)} style={styles.btn}>
        <Text style={styles.btnText}>Show All The Clicked Items</Text>
      </TouchableOpacity>
      </View>
    </View>
  );
};

export default App;

const styles= StyleSheet.create({
  btn:{
    borderRadius:5,
    borderWidth:0.5,
    backgroundColor:'#f1f1f1',
    padding:5,
    width:200

  },
  btnText: {
    textAlign:'center'
  }
})

```

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
