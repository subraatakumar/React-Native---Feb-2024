## Advanced Bottom Tab Navigator with Custom Header, Icon and Style

```js

import {
  Text,
  View,
  ImageBackground,
  TouchableOpacity,
  StyleSheet,
  Image,
  TextInput,
  SafeAreaView,
  Platform,
  StatusBar,
} from 'react-native';
import { NavigationContainer } from '@react-navigation/native';
import { createBottomTabNavigator } from '@react-navigation/bottom-tabs';
import MaterialCommunityIcons from '@expo/vector-icons/MaterialCommunityIcons';
import FontAwesome from '@expo/vector-icons/FontAwesome';
const Colors = {
  primary: '#44b361',
  primaryLight: '#cdfad9',
};
function HomeScreen() {
  return (
    <SafeAreaView
      style={{
        flex: 1,
        justifyContent: 'center',
        alignItems: 'center',
        marginTop: 40,
      }}>
      <Text>Home!</Text>
    </SafeAreaView>
  );
}

const HomeHeader = () => {
  return (
    <View style={styles.headerContainer}>
      <TouchableOpacity onPress={() => navigation.navigate('Profile')}>
        <Image
          source={{
            uri: 'https://cdn.pixabay.com/photo/2016/03/31/19/58/avatar-1295431_960_720.png',
          }} // Replace with your image path
          style={styles.profileIcon}
        />
      </TouchableOpacity>
      <TextInput
        style={styles.textInput}
        placeholder="Enter text here"
        // Add other input props as needed: onChangeText, value, etc.
      />
      <TouchableOpacity onPress={() => navigation.navigate('Chat')}>
        <MaterialCommunityIcons
          name="message"
          size={24}
          style={styles.messageIcon}
        />
      </TouchableOpacity>
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

const Screen3 = () => {
  return (
    <ImageBackground
      source={{
        uri: 'https://cdn.pixabay.com/photo/2017/12/13/16/40/background-3017167_1280.jpg',
      }}
      style={{ flex: 1 }}>
      <Text>Screen Three</Text>
    </ImageBackground>
  );
};

const Tab = createBottomTabNavigator();

function MyTabs() {
  return (
    <View style={{ flex: 1, marginTop: Platform.OS == 'android' ? 25 : 0 }}>
      <Tab.Navigator
        screenOptions={{
          header: () => <HomeHeader />,
          tabBarStyle: { backgroundColor: Colors.primary, padding:10 },
          tabBarActiveTintColor: 'white',
          tabBarInactiveTintColor: 'green',
        }}>
        <Tab.Screen
          name="Home"
          component={HomeScreen}
          options={{
            tabBarIcon: ({ color, size }) => (
              <FontAwesome name="home" color={color} size={size} />
            ),
          }}
        />
        <Tab.Screen
          name="Anouncements"
          component={SettingsScreen}
          options={{
            tabBarIcon: ({ color, size }) => (
              <FontAwesome name="bullhorn" color={color} size={size} />
            ),
          }}
        />
        <Tab.Screen name="Screen3" component={Screen3}  options={{
          tabBarLabel:"Friends",
            tabBarIcon: ({ color, size }) => (
              <FontAwesome name="users" color={color} size={size} />
            ),
          }}/>
      </Tab.Navigator>
    </View>
  );
}

export default function App() {
  
  return (
    <NavigationContainer>
      <StatusBar backgroundColor={Colors.primary} barStyle="light-content" />
      <MyTabs />
    </NavigationContainer>
  );
}

const styles = StyleSheet.create({
  profileIcon: {
    width: 40,
    height: 40,
    borderRadius: 20,
  },
  textInput: {
    flexGrow: 1,
    marginHorizontal: 10,
    padding: 8,
    borderRadius: 5,
    backgroundColor: '#f0f0f0',
  },
  messageIcon: {
    color: '#000',
  },
  headerContainer: {
    flexGrow: 1,
    flexDirection: 'row',
    padding: 10,
    alignItems: 'center',
    justifyContent: 'space-between',
    paddingHorizontal: 10,
    backgroundColor: Colors.primary,
  },
});
```
## Stack and Bottom Tab Combined
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
    navigation.navigate('Notification',{data: data});
  };
  const renderItem = ({ item }) => {
    return (
      <TouchableOpacity onPress={()=>handleNotificationClick(item)}>
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

  return <View>
    <Text>Notification Screen</Text>
    <Text>Data Received: {props?.route?.params?.data ? route?.params?.data : ""}</Text>
  </View>;
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
      <MyStack.Screen name="Home" component={MyTabs} options={{headerShown:false}}/>
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
