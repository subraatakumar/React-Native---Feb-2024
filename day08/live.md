# Modal Example

```js
import React, {useState} from 'react';
import {Alert, Modal, StyleSheet, Text, Pressable, View} from 'react-native';

const App = () => {
  const [modalVisible, setModalVisible] = useState(false);
  return (
    <View style={styles.centeredView}>
      <Modal
        animationType="none"
        transparent={true}
        visible={modalVisible}
>
        <View style={styles.modalContainer}>
          <View style={styles.modalView}>
            <Text style={styles.modalText}>Hello World!</Text>
             <Text style={styles.modalText}>Sample Modal Screen!</Text>
            <Pressable
              style={[styles.button, styles.buttonClose]}
              onPress={() => setModalVisible(!modalVisible)}>
              <Text style={styles.textStyle}>Hide Modal</Text>
            </Pressable>
          </View>
        </View>
      </Modal>
     
      <Pressable
        style={[styles.button, styles.buttonOpen]}
        onPress={() => setModalVisible(true)}>
        <Text style={styles.textStyle}>Show Modal</Text>
      </Pressable>
      <Pressable
        style={[styles.button, styles.buttonOpen,{marginTop:20}]}
        onPress={() => console.log("Second button clicked")}>
        <Text style={styles.textStyle}>Second Button</Text>
      </Pressable>
    </View>
  );
};

const styles = StyleSheet.create({
  centeredView: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
    marginTop: 22,
    backgroundColor:'green'
  },
  modalContainer:{
    flex:1,
    backgroundColor:"rgba(0,0,0,0.5)",
    alignItems:'center',
    justifyContent:'center'
  },
  modalView: {
    width:"80%",
    backgroundColor: 'white',
    borderRadius: 20,
    padding: 35,
    alignItems: 'center',
    shadowColor: '#000',
    shadowOffset: {
      width: 0,
      height: 2,
    },
    shadowOpacity: 0.25,
    shadowRadius: 4,
    elevation: 5,
  },
  button: {
    borderRadius: 20,
    padding: 10,
    elevation: 2,
  },
  buttonOpen: {
    backgroundColor: '#F194FF',
  },
  buttonClose: {
    backgroundColor: '#2196F3',
  },
  textStyle: {
    color: 'white',
    fontWeight: 'bold',
    textAlign: 'center',
  },
  modalText: {
    marginBottom: 15,
    textAlign: 'center',
  },
});

export default App;
```

- To dismis modal on back pressed on android device please use `onRequestClose` prop.

```js
        onRequestClose={() => {
          Alert.alert('Modal has been closed.');
          setModalVisible(!modalVisible);
        }}
```

## Alert Example
```js
import * as React from 'react';
import {
  Text,
  View,
  Image,
  TextInput,
  Platform,
  StatusBar,
  FlatList,
  TouchableOpacity,
  Alert
} from 'react-native';
import { NavigationContainer } from '@react-navigation/native';
import { createBottomTabNavigator } from '@react-navigation/bottom-tabs';
import { createStackNavigator } from '@react-navigation/stack';

import FontAwesome from '@expo/vector-icons/FontAwesome';

const Colors = {
  primary: '#044a2c',
  primaryLight: '#148052',
  primaryLigher: '#3cbd86',
};

function HomeScreen() {
  return (
    <View style={{ flex: 1 }}>
      <Text>Home Screen</Text>
    </View>
  );
}

const CustomHeader = () => {
  return (
    <View
      style={{
        flexDirection: 'row',
        justifyContent: 'space-between',
        backgroundColor: Colors.primary,
        padding: 10,
        gap: 10,
      }}>
      <View style={{ width: 50 }}>
        <Image
          source={{
            uri: 'https://cdn.pixabay.com/photo/2022/11/29/00/17/witch-7623438_1280.jpg',
          }}
          style={{ width: 50, height: 50, borderRadius: 25 }}
        />
      </View>
      <View
        style={{
          flex: 1,
          padding: 10,
          backgroundColor: '#e8e8e8',
          borderRadius: 10,
          flexDirection: 'row',
          justifyContent: 'space-between',
        }}>
        <FontAwesome name="search" color={'rgba(0,0,0,0.1)'} size={24} />
        <TextInput placeholder="search" style={{ flex: 1, marginLeft: 10 }} />
      </View>
      <View style={{ width: 50, backgroundColor: Colors.primaryLight }}></View>
    </View>
  );
};

function SettingsScreen() {
  return (
    <View style={{ flex: 1, justifyContent: 'center', alignItems: 'center' }}>
      <Text>Settings!</Text>
    </View>
  );
}

function NotificationScreen({ navigation }) {
  const notification = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

  const handleNotificationClick = (data) => {
    Alert.alert("Redirecting","Do you want to see the notification details?", [
      {
        text: "Ok",
        onPress: () => redirectToNotification(data)
      },{
        text: "Cancel",
        style: 'cancel',
      }
    ])
  };

  const redirectToNotification = (data) => {
    navigation.navigate('Notification',{data:data});
  };
  const renderItem = ({ item }) => {
    return (
      <TouchableOpacity onPress={() => handleNotificationClick(item)}>
        <View
          style={{
            padding: 15,
            margin: 10,
            backgroundColor: Colors.primaryLigher,
          }}>
          <Text>Notification {item}</Text>
        </View>
      </TouchableOpacity>
    );
  };
  return (
    <View style={{ flex: 1 }}>
      <Text>Notifications</Text>
      <FlatList data={notification} renderItem={renderItem} />
    </View>
  );
}

const Notification = (props) => {
  return (
    <View>
      <Text>Notification Screen</Text>
      <Text>
        Data Received:{' '}
        {props?.route?.params?.data ? props?.route?.params?.data : ''}
      </Text>
    </View>
  );
};

const Tab = createBottomTabNavigator();

function MyTabs() {
  return (
    <Tab.Navigator
      screenOptions={{
        header: () => <CustomHeader />,
        tabBarStyle: { backgroundColor: Colors.primary },
        tabBarInactiveTintColor: Colors.primaryLight,
        tabBarActiveTintColor: '#FFFFFF',
      }}>
      <Tab.Screen name="Home" component={HomeScreen} />
      <Tab.Screen name="Settings" component={SettingsScreen} />
      <Tab.Screen name="AllNotification" component={NotificationScreen} />
    </Tab.Navigator>
  );
}

const MyStack = createStackNavigator();

const MyStackNavigation = () => {
  return (
    <MyStack.Navigator>
      <MyStack.Screen
        name="Home"
        component={MyTabs}
        options={{ headerShown: false }}
      />
      <MyStack.Screen name="Notification" component={Notification} />
    </MyStack.Navigator>
  );
};

export default function App() {
  return (
    <NavigationContainer>
      <View style={{ marginTop: Platform.OS == 'android' ? 25 : 0, flex: 1 }}>
        <StatusBar backgroundColor={Colors.primary} hidden={false} />
        <MyStackNavigation />
      </View>
    </NavigationContainer>
  );
}
```
