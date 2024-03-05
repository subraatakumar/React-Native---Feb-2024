# Image in React Native

A React component for displaying different types of images, including network images, static resources, temporary local images, and images from local disk, such as the camera roll.

```js
import React from 'react'
import { View, Image, StyleSheet } from 'react-native'
import snackIcon from './assets/snack-icon.png'

const App = () => {
  return(<View>
    <Image source={{uri:"https://cdn.pixabay.com/photo/2021/11/22/05/02/dalmatian-6815838_960_720.jpg"}} style={styles.roundImage}/>
    <Image source={require('./assets/snack-icon.png')} style={styles.squareImage}/>
    <Image source={snackIcon} style={styles.squareImage}/>
  </View>)
}

export default App;

const styles = StyleSheet.create({
  squareImage : {
    width:100,
    height:100,
    borderRadius:10  
  },
  roundImage: {
    width:100,
    height:100,
    borderRadius:'50%'  
  }
})

```

### Image Props

- source: specifies source of the image
- resizeMode: Determines how to resize the image when the frame doesn't match the raw image dimensions.
    Value of this prop can be any one of the following. The default is `cover`
  - cover: Scale the image uniformly (maintain the image's aspect ratio) so that both dimensions (width and height) of the image will be equal to or larger than the corresponding dimension of the view (minus padding). at least one dimension of the scaled image will be equal to the corresponding dimension of the view (minus padding). 
  - contain: Scale the image uniformly (maintain the image's aspect ratio) so that both dimensions (width and height) of the image will be equal to or less than the corresponding dimension of the view (minus padding).
  - stretch: Fit the width and height of image to mentioned width and height, This may change the aspect ratio.
  - repeat: Repeat the image to cover the frame of the view. The image will keep its size and aspect ratio, unless it is larger than the view, in which case it will be scaled down uniformly so that it is contained in the view.
  - center: Center the image in the view along both dimensions. If the image is larger than the view, scale it down uniformly so that it is contained in the view.

# [ImageBackground](https://reactnative.dev/docs/imagebackground)

# [Linking](https://reactnative.dev/docs/linking)

Linking gives you a general interface to interact with both incoming and outgoing app links.

# FlatList in React Native

In React Native, you can use the FlatList component to render a long list of data. It renders only the items shown on the screen in a scrolling list and not all the data at once. To render a scrollable list of items using FlatList , you need to pass the required data prop to the component. The other props are self explanatory.

- data: array of items
- renderItem: a function that returns a JSX for a single item.
- keyExtractor: extracts key from data. But remember that it should be a string.
- numColumns: number of columns
- horizontal: show horizontal list
- itemSeparatorComponent:
- listHeaderComponent:
- listFooterComponent:
- listEmptyComponent: 

## Section List in React Native

A performant interface for rendering sectioned lists, supporting the most handy features:

- Fully cross-platform.
- Configurable viewability callbacks.
- List header support.
- List footer support.
- Item separator support.
- Section header support.
- Section separator support.
- Heterogeneous data and item rendering support.
- Pull to Refresh.
- Scroll loading.


Note: Remember FlatList and SectionList both are PureComponents which means that it will not re-render if props remain shallow-equal.




