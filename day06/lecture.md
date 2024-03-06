## React Native Architecture

When we look at the react native architecture we find two different divisions often interact with each other :

- native world (Android or IOS)
- JavaScript world

Communication between these two worlds happens via React Native Bridge. React Native parses the bunch of commands coming from the React world into a JSON array, serializes it as a string, and then transfers it to the native world via that bridge.

To maintain consistency across all platforms, React Native implements the actual layout by converting React-based display styles (eg, flex) to the relative position values where each element is to be structured and then finally passes it over the UI layer of the native world. Broadly speaking, the current React Native architecture is based on 3 major threads-

- `Main/Native/UI thread` — where all the UI elements are rendered and native code is executed.
- `Layout thread/Shadow Thread` — this is a background thread where the actual layout is calculated. As mentioned, it recasts the flexbox layout with the help of Facebook’s layout engine called Yoga.
- `JavaScript thread` — this thread is responsible for executing and compiling all the JavaScript-related code or business logic.

![](https://firebasestorage.googleapis.com/v0/b/mymasai-school.appspot.com/o/project_files%2Fold_architecture.webp?alt=media&token=968b1e3f-a715-447f-b9a7-e3dc152c0aea)

## The drawbacks of this architecture

- In the current architecture, communication between threads occurs through the bridge so it leads to a slow transfer rate and unnecessary copying of data.
- Since the communication happens asynchronously, one major drawback of this asynchronous approach is that it doesn’t execute the events in real-time instead it schedules the action. Let’s imagine a scenario where we implemented a Flatlist and when we start scrolling, each time an event is triggered and is scheduled for its execution. So, when dealing with a large set of list items and scrolling rapidly one can easily notice a white glitch before actual rendering happens. This happens because the UI layer of that native world hasn’t received any layout information by the time scrolling is completed. So this scrolling effect needs to take place synchronously to achieve the desired result which is not possible in the current architecture.

# The new architecture

Starting from version 0.68, React Native provides the New Architecture, which offers developers new capabilities for building highly performant and responsive apps.

![](https://firebasestorage.googleapis.com/v0/b/mymasai-school.appspot.com/o/project_files%2Fnew_architecture.webp?alt=media&token=cff47435-c289-4bb3-b89e-fe0aa99e70e2)

The New Architecture dropped the concept of The Bridge in favor of another communication mechanism: the JavaScript Interface (JSI). The JSI is an interface that allows a JavaScript object to hold a reference to a C++ and viceversa.

Once an object has a reference to the other one, it can directly invoke methods on it. So, for example, a C++ object can now ask a JavaScript object to execute a method in the JavaScript world and viceversa.

This idea allowed to unlock several benefits:

- `Synchronous execution:` it is now possibile to execute synchronously those functions that should not have been asynchronous in the first place.
- `Concurrency:` it is possible from JavaScript to invoke functions that are executed on different thread.
- `Lower overhead:` the New Architecture don't have to serialize/deserialize the data anymore, therefore there are no serialization taxes to pay.
- `Code sharing:` by introducing C++, it is now possible to abstract all the platform agnostic code and to share it with ease between the platforms.
- `Type safety:` to make sure that JS can properly invoke methods on C++ objects and viceversa, a layer of code automatically generated has been added. The code is generated starting from some JS specification that must be typed through Flow or TypeScript.

# React Navigation

Navigating through screens in a React Native app can be tricky. Initially, React Native had its own way of handling this, but it was later replaced by a library called react-navigation.

The challenges come from the fact that iOS and Android handle navigation differently. iOS uses view controllers, and Android uses activities. These are like behind-the-scenes tools that work differently and look different to users. React Native navigation libraries try to make things look right on each platform while giving developers a consistent way to work with it using JavaScript.

Another problem is that the special tools for navigating on mobile don't match up neatly with the regular things developers use in React Native, like View, Text, and Image. It's not always clear how to connect these tools to JavaScript.

Unlike web apps, where going to a new URL takes you to a new page, mobile apps remember where you've been. You can go back to previous screens, and you might even see the same screen more than once.

Because of these challenges, there isn't one perfect way to do navigation in React Native. That's why it was taken out of the main React Native package.

## Implementation of React Navigation

The `react-navigation` documentation explains how to install the required dependencies, based on your React Native setup.

After that, we need to do 5 things:

- Set up a <NavigationContainer> from @react-navigation/native
- Create a navigator:
  - createStackNavigator from @react-navigation/stack
  - createBottomTabNavigator from @react-navigation/bottom-tabs
  - createDrawerNavigator from @react-navigation/drawer
- Define the screens in our app
- Wrap all your screens in navigator
- Wrap the navigator in navigation container

## Stack navigator

In React Native, stack navigation refers to a navigation pattern where screens are organized in a stack-like structure. This means that screens are placed on top of each other, and you navigate through them by adding or removing screens from the stack.

The most common use case for stack navigation is handling the flow of screens in a way that resembles a "stack" or a pile of cards. When you navigate to a new screen, it gets added to the top of the stack. If you go back, the top screen is removed, revealing the screen beneath it. This mimics the behavior of a physical stack of objects.

Stack navigation is well-suited for scenarios where you have a linear flow of screens, such as moving from a login screen to a dashboard and then to a details screen. It follows a Last In, First Out (LIFO) approach, where the last screen added to the stack is the first one to be removed when navigating back.

```js
import { NavigationContainer } from '@react-navigation/native'
import { createStackNavigator } from '@react-navigation/stack'

const Root = createStackNavigator()

const Screen1 = ({ navigation, route }) => {
  return <Text>Screen1</Text>
}

const Screen2 = ({ navigation, route }) => {
  return <Text>Screen2</Text>
}

const Screen3 = ({ navigation, route }) => {
  return <Text>Screen3</Text>
}

const App = () => {
  return (
    <NavigationContainer>
      <Root.Navigator>
        <Root.Screen name="Screen1" component={Screen1} />
        <Root.Screen name="Screen2" component={Screen2} />
        <Root.Screen name="Screen3" component={Screen3} />
      </Root.Navigator>
    </NavigationContainer>
  )
}
```
## Bottom Tab Navigator

In React Native, a bottom tab navigator is a navigation pattern that typically uses a tab bar located at the bottom of the screen to allow users to switch between different screens or tabs within an app. Each tab represents a specific section or functionality of the application.

Key features of a bottom tab navigator include:

- **Tab Bar:** The tabs are visually represented by a bar at the bottom of the screen, displaying icons or labels for each tab. Users can tap on these tabs to navigate to the associated screens.

- **Persistent UI:** The tab bar usually remains visible at the bottom of the screen, providing a persistent and easily accessible way for users to move between different sections of the app.

- **Independent Content:** Each tab typically corresponds to an independent view or screen. When users switch tabs, they navigate to a different part of the app, and the content on the screen changes accordingly.

Bottom tab navigation is commonly used in mobile applications where there are distinct and separate sections that users frequently access. Examples of tabs might include Home, Explore, Notifications, and Profile.


