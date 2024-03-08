## Modal

The Modal component is a basic way to present content above an enclosing view.

### Props

- inherits `View` Props
- visible: `true or false` : determines the visibility of modal
- onRequestClose: `function` : It is called when the user taps hardware back button on android or the menu button on IOS.
- animationType: `none, slide, fade` : controls modal animation, default is none
- onShow: `function` : The passed function will be called once the modal is shown
- transparent: `true or false` : If true renders the modal over a transparent background and fill the entire view.

## Alert

Launches an alert dialog with the specified title and message. By default an `Ok` button will be displayed, If we need we can provide a list of buttons with onPress callback.

## Networking in React Native

Imagine your React Native app as a visitor in a city. To get information or send things (like placing an order), it needs to talk to other places (like backend servers) on the internet. This communication is called networking.

### Here's a simplified breakdown:

- **Request:** Your app sends a message (request) to a server, specifying what it wants (data, action, etc.).
- **Response:** The server receives the request, processes it, and sends a reply (response) back to your app.
- **Data Flow:** The response might contain information (like product details) or confirmation of an action (like order success). Your app uses this data to update its content or perform further actions.

For this purpose we can use inbuilt `fetch` or third party networking library `axios`.

## GET Request

```js
    axios.get('https://jsonplaceholder.typicode.com/postss')
      .then(response => {
        console.log(response.data);
      }).catch(error => {
        console.log("Unable to access data");
      });

// fetch()
fetch('https://jsonplaceholder.typicode.com/posts')
  .then(response => response.json())    // one extra step
  .then(data => {
    console.log(data)
  })
  .catch(error => console.error(error));
```

## POST Request with Custom Header

```js
// axios

const url = "https://jsonplaceholder.typicode.com/posts";
const data = {
  title: "This is title",
  body: "This is body",
  userId: 1,
};
axios
  .post(url, data, {
    headers: {
      Accept: "application/json",
      "Content-Type": "application/json;charset=UTF-8",
    },
  })
  .then(({ data }) => {
    console.log(data);
  });

// fetch()

const url = "https://jsonplaceholder.typicode.com/posts";
const options = {
  method: "POST",
  headers: {
    Accept: "application/json",
    "Content-Type": "application/json;charset=UTF-8",
  },
  body: JSON.stringify({
    title: "This is title",
    body: "This is body",
    userId: 1,
  }),
};
fetch(url, options)
  .then((response) => response.json())
  .then((data) => {
    console.log(data);
  });
```

## Why Use Axios?

Notice that:
- To send data, fetch() uses the body property for a post request to send data to the endpoint, while Axios uses the data property
- The data in fetch() is transformed to a string using the JSON.stringify method
- Axios automatically transforms the data returned from the server, but with fetch() you have to call the response.json method to parse the data to a JavaScript object. More info on what the response.json method does can be found [here](https://developer.mozilla.org/en-US/docs/Web/API/Response/json).
- With Axios, the data response provided by the server can be accessed with in the [data object](https://github.com/axios/axios#response-schema), while for the fetch() method, the final data can be named any variable.

[Json Placeholder Typicode Guide](https://jsonplaceholder.typicode.com/guide/)

## Switch

- https://reactnative.dev/docs/switch

## Check Box

- https://github.com/react-native-checkbox/react-native-checkbox

## Radio Button 

- https://www.npmjs.com/package/react-native-radio-buttons-group

## Dropdown 

- https://www.npmjs.com/package/react-native-element-dropdown
