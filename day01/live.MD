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
