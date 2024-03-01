# Platform Specific Background Color

```js
import React from "react";
import { SafeAreaView, View, Text, StyleSheet, Platform } from "react-native";

const App = () => {
  return (
    <SafeAreaView style={styles.container}>
      <Text style={styles.text}>Hello World!</Text>
      <View style={{...styles.container, ...styles1.rowSpaceBetween}}>
        <Text style={{...styles.text1, flex:1}}>Text 1</Text><Text style={{...styles.text1, flex:2}}>Text 2</Text>
      </View>
    </SafeAreaView>
  );
};

export default App;


const styles = StyleSheet.create({
  
  container: {
    backgroundColor: Platform.select({
      ios: '#333399',
      android:'#333300',
      web:'#669999'
    }), 
    paddingTop: Platform.OS === 'android' ? 25 : 0,
    paddingBottom:10,
    paddingHorizontal:10,
  },
  text : {
    color:'#fff',
  },
  text1 : {
    overflow:'hidden', // required for IOS
    backgroundColor: '#fff',
  	borderRadius: 10,
    padding:5,
    marginHorizontal:10,
  }
})

const styles1 = StyleSheet.create({
  rowSpaceBetween : {
    flexDirection:'row',
    justifyContent: 'space-between'
  }
})  
```  

```js
import {
  View,
  Text,
  StyleSheet,
  Platform,
  Button,
  TouchableOpacity,
} from 'react-native';
import { useState } from 'react';

function App() {
  let names: string[];

  names = ['Ramesh', 'Suresh'];
  const [margin, setMargin] = useState(10);
  let bordertype = 'g';
  const handleButtonClick = () => {
    setMargin((prev) => prev + 10);
  };
  return (
    <View
      style={{
        marginTop: margin,
      }}>
      {names.map((name) => (
        <Text
          style={[
            { flex: 1 },
            styles.text,
            bordertype == 'green'
              ? styles.textBorderGreen
              : styles.textBorderYellow,
          ]}>
          {name}
        </Text>
      ))}

      <TouchableOpacity onPress={handleButtonClick}>
        <View style={{ backgroundColor: 'purple', height: 50, width: 50 }}>
          <Text>Button</Text>
        </View>
      </TouchableOpacity>
    </View>
  );
}

export default App;

const styles = StyleSheet.create({
  text: {
    backgroundColor: 'red',
    margin: 10,
    padding: 10,
    borderRadius: 10,
  },
  textBorderGreen: {
    borderWidth: 3,
    borderColor: 'green',
  },
  textBorderYellow: {
    borderWidth: 3,
    borderColor: 'yellow',
  },
});
```
