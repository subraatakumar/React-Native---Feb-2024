## React Native Vector Icons

In React Native, "React Native Vector Icons" refers to a library that provides a set of customizable vector icons for use in your React Native applications. Vector icons are scalable and look sharp on various screen resolutions, making them a popular choice for displaying icons in mobile apps.

The "React Native Vector Icons" library makes it easy to include and use popular icon sets such as FontAwesome, MaterialIcons, Entypo, and many more in your React Native projects. These icons can be customized in terms of size, color, and other properties.

- https://www.npmjs.com/package/react-native-vector-icons can be used in bare react native projects
- https://www.npmjs.com/package/@expo/vector-icons can be used in expo managed projects

## Formik 

Formik is a popular form management library in the React ecosystem, and it can be used with React Native as well. Its primary purpose is to simplify the process of building and handling forms in React applications by providing a set of utilities and components.

In the context of React Native, Formik allows you to manage forms in a way that is similar to how you would do it in React. It helps you handle form state, form submission, form validation, and other related tasks in a more organized and efficient manner.

Key features and concepts of Formik include:

- **Form State Management:** Formik helps manage the state of your forms, making it easy to keep track of input values, form submission status, and error messages.

- **Form Validation:** It provides a straightforward way to define and handle form validation. You can define validation rules for each form field, and Formik will take care of displaying error messages and managing the overall validity of the form.

- **Form Submission:** Formik streamlines the process of handling form submissions. It allows you to define submission functions and manage the submission status, making it easier to interact with APIs or perform other actions upon form submission.

- **Field Components:** Formik provides a set of field components that you can use to connect your form fields with the Formik context. These components simplify the process of handling user input and updating the form state.

## Yup

Yup is a JavaScript library for schema validation. It is often used in conjunction with form libraries like Formik to handle validation logic in React applications. Yup allows you to define a schema for your data and then validate whether the data conforms to that schema.

Here are some key features and use cases for Yup:

- **Declarative Schema Definition:** Yup provides a simple and declarative API for defining validation schemas. You can specify the shape of your data, the types of the values, and any validation rules that should be applied.

- **Schema Validation:** Once you've defined your schema, Yup can be used to validate data against that schema. It checks whether the data conforms to the specified rules and constraints, and it returns error messages for any validation failures.

- **Chaining and Composition:** Yup supports chaining and composition of validation rules. You can chain multiple validation methods together to create complex validation logic. This makes it easy to express a wide range of validation scenarios.

- **Async Validation:** Yup supports asynchronous validation. This is useful when you need to perform validation that involves asynchronous operations, such as fetching data from a server.

- **Integration with Formik:** Yup is commonly used with Formik to handle form validation in React applications. Formik allows you to manage form state, and Yup is used to define the validation schema for the form fields.

## ScrollView

The ScrollView is a generic scrolling container that can contain multiple components and views. By default it is a vertical scrollable container but you can make it horizontal by specifying horizontal property.

Let us modify our previous program and wrap our list of items with in a scrollview.

### Example of vertical list of items

```js
    <ScrollView>
    {data.map(item => (<Text style={styles.item}>{item}</Text>))}
    {data.length == 0 && <Text style={{textAlign:'center'}}>No Items in list</Text>}
    </ScrollView>
```

### Example of horizntal list of items

```js
    <ScrollView horizontal>
    {data.map(item => (<Text style={styles.item}>{item}</Text>))}
    {data.length == 0 && <Text style={{textAlign:'center'}}>No Items in list</Text>}
    </ScrollView>
```

Note : The ScrollView works best to present a small number of things of a limited size. All the elements and views of a ScrollView are rendered, even if they are not currently shown on the screen. If you have a long list of items which cannot fit on the screen, you should use a FlatList instead.

Before moving to FlatList let us find out, How to attach a pull to refresh to our ScrollView.

## Pull to Refresh

`RefreshControl` component is used inside a ScrollView to provide pull to refresh functionality.

most used props
- onRefresh: Function to be executed when the ScrollView is pulled down.
- refreshing : when true displays spinner. So at the beginning of `onRefresh` method set it true then at the end set it false.
- colors: on Android sets the spinner color
- tintColor: on IOS sets the spinner color

```js
import React, {useState} from 'react'
import {View, Text, StyleSheet, RefreshControl, ScrollView} from 'react-native'
import  Constants  from 'expo-constants'

const App = () => {
  const [data, setData] = useState([])
  const [refreshing, setRefreshing] = React.useState(false);

  const onRefresh = () => {
    setRefreshing(true) ;
    setData(prev => [...prev, 'item '+Math.ceil(Math.random()*100)]);
    setRefreshing(false);
  }
  return(
    <View style={styles.container}>
      <Text style={styles.heading}>List of Items</Text>
      <ScrollView
    refreshControl={<RefreshControl refreshing={refreshing} onRefresh={onRefresh} colors={['red']} tintColor='green' />}
  >
    {data.map(item => (<Text style={styles.item}>{item}</Text>))}
    {data.length == 0 && <Text style={{textAlign:'center'}}>No Items in list</Text>}
    </ScrollView>
  </View>)
}

export default App;

const styles = StyleSheet.create({
  container :{
    marginTop: Constants.statusBarHeight
  },
  item : {
    borderRadius:10,
    borderWidth:1,
    padding:5,
    margin:5,
  },
  heading : {
    fontSize: 25,
    fontWeight: 'bold',
    textAlign:'center'
  }
})
```

As this task ends quickly so you may not see the spinner properly. While getting data from server you may see it properly. To test it now letus create a function which intentionaly delay the process, so that we can see the spinner.

```js
  const wait = (timeout) => {
    return new Promise(resolve => setTimeout(resolve, timeout));
  }

  const onRefresh = () => {
    setRefreshing(true) ;
    setData(prev => [...prev, 'item '+Math.ceil(Math.random()*100)]);
    wait(2000).then(() => setRefreshing(false));
  }
```

## [Linking](https://reactnative.dev/docs/linking)

Linking gives you a general interface to interact with both incoming and outgoing app links.
