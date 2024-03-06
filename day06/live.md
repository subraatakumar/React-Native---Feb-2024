## Basic Stack Screen Example

```js
// App.js
import { View } from 'react-native';
import { NavigationContainer } from '@react-navigation/native';
import MyStack from './MyStack';
const App = () => {
  return (
    <NavigationContainer>
      <MyStack />
    </NavigationContainer>
  );
};

export default App;
```

```js
// MyStack.js
import { createStackNavigator } from '@react-navigation/stack';
import { Text, TouchableOpacity, View, StyleSheet, ImageBackground, SafeAreaView } from 'react-native';

const Stack = createStackNavigator();

const Screen1 = ({ navigation }) => {
  return (
    <View>
      <Text>Screen One</Text>
      <TouchableOpacity
        onPress={() => navigation.navigate('Notifications')}
        style={styles.btn}>
        <Text style={styles.btnText}>Goto Notifications</Text>
      </TouchableOpacity>
    </View>
  );
};

const Screen2 = () => {
  return <Text>Screen Two</Text>;
};

const Screen3 = ({ navigation }) => {
  return (
    <ImageBackground source={{uri:"https://cdn.pixabay.com/photo/2017/12/13/16/40/background-3017167_1280.jpg"}} style={{flex:1}}>
      <Text>Screen Three</Text>
      <TouchableOpacity
        onPress={() => navigation.navigate('Settings')}
        style={styles.btn}>
        <Text style={styles.btnText}>Goto Settings</Text>
      </TouchableOpacity>
    </ImageBackground>
  );
};

const Screen4 = ({ navigation }) => {
  return (
    <ImageBackground source={{uri:"https://cdn.pixabay.com/photo/2020/06/20/16/20/orange-5321552_1280.jpg"}} style={{flex:1}}>
    <SafeAreaView style={{flex:1, marginTop:60, justifyContent:'center', alignItems:'center'}}>
      <Text style={{fontSize:25, color:'red', fontWeight:'bold', textAlign:'center'}}>Login Screen</Text>
      <TouchableOpacity
        onPress={() => navigation.replace('Home')}
        style={styles.btn}>
        <Text style={styles.btnText}>Login</Text>
      </TouchableOpacity>
      </SafeAreaView>
    </ImageBackground>
  );
};

const MyStack = () => {
  return (
    <Stack.Navigator>
      <Stack.Screen name="Login" component={Screen4} options={{headerShown: false}}/>
      <Stack.Screen name="Home" component={Screen1} />
      <Stack.Screen name="Settings" component={Screen2} />
      <Stack.Screen name="Notifications" component={Screen3} />
    </Stack.Navigator>
  );
};

export default MyStack;

const styles = StyleSheet.create({
  btn: {
    padding: 10,
    margin: 40,
    borderRadius: 40,
    backgroundColor: '#4287f5',
    width:150,
  },
  btnText: {
    color: '#FFFFFF',
    textAlign: 'center',
  },
});
```

## Bottom Tab Navigation

```js
import * as React from 'react';
import { Text, View, ImageBackground, TouchableOpacity, StyleSheet } from 'react-native';
import { NavigationContainer } from '@react-navigation/native';
import { createBottomTabNavigator } from '@react-navigation/bottom-tabs';
import { AntDesign } from '@expo/vector-icons';

function HomeScreen() {
  return (
    <View style={{ flex: 1, justifyContent: 'center', alignItems: 'center' }}>
      <Text>Home!</Text>
    </View>
  );
}

function SettingsScreen() {
  return (
    <View style={{ flex: 1, justifyContent: 'center', alignItems: 'center' }}>
      <Text>Settings!</Text>
    </View>
  );
}

const Screen3 = () => {
  return (
    <ImageBackground source={{uri:"https://cdn.pixabay.com/photo/2017/12/13/16/40/background-3017167_1280.jpg"}} style={{flex:1}}>
      <Text>Screen Three</Text>
    </ImageBackground>
  );
};

const Tab = createBottomTabNavigator();

function MyTabs() {
  return (
    <Tab.Navigator screenOptions={{
      tabBarActiveTintColor: 'red',
      tabBarInactiveTintColor: 'green'
    }}>
      <Tab.Screen name="Home" component={HomeScreen} options={{tabBarShowLabel: false, tabBarIcon: ({color, size}) => <AntDesign name="home" size={size} color={color} />}} />
      <Tab.Screen name="Settings" component={SettingsScreen}  options={{tabBarShowLabel: false, tabBarIcon: ({color, size}) => <AntDesign name="setting" size={size} color={color} />}}/>
      <Tab.Screen name="Screen3" component={Screen3} options={{tabBarShowLabel: false, tabBarIcon: ({color, size}) => <AntDesign name="linechart" size={size} color={color}/>}} />
    </Tab.Navigator>
  );
}

export default function App() {
  return (
    <NavigationContainer>
      <MyTabs />
    </NavigationContainer>
  );
}
```
