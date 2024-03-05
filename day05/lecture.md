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

```js
import React, {useState} from 'react'
import {View, Text, StyleSheet, RefreshControl, FlatList} from 'react-native'
import  Constants  from 'expo-constants'

const App = () => {
  const [data, setData] = useState([])
  const [refreshing, setRefreshing] = React.useState(false);

  const onRefresh = () => {
    setRefreshing(true) ;
    setData(prev => [...prev, 'Task no: '+Math.ceil(Math.random()*100)]);
    setRefreshing(false);
  }
  return(
    <View style={styles.container}>
      <Text style={styles.heading}>List of Tasks</Text>
      <FlatList
      refreshControl={<RefreshControl 
          refreshing={refreshing} 
          onRefresh={onRefresh} 
          colors={['red']} 
          tintColor='green' />}
      data={data}
      renderItem={({item}) => <Text style={styles.item}>{item}</Text>}
      keyExtractor={(_, index) => index.toString() }
      numColumns={2}
      ItemSeparatorComponent={() => <View style={styles.separator}></View>}
      ListHeaderComponent={() => <Text style={{textAlign:'center'}}>This is header of FlatList</Text>}
      ListFooterComponent={() => <Text  style={{textAlign:'center'}}>This is footer of FlatList</Text>}
      ListEmptyComponent={() => <Text style={{textAlign:'center'}}>No Items in list</Text>}

      />
  </View>)
}

export default App;

const styles = StyleSheet.create({
  container :{
    marginTop: Constants.statusBarHeight
  },
  item : {
    flex:1,
    borderRadius:10,
    borderWidth:1,
    borderColor:'#006666',
    padding:5,
    margin:5,
  },
  heading : {
    fontSize: 25,
    fontWeight: 'bold',
    textAlign:'center'
  },
  separator:{
    flex:1,
    borderColor:'#006666',
    borderBottomWidth:2,
    margin:10
  }
})
```

## Section List in React Native

```js
import React from "react";
import { StyleSheet, Text, View, SafeAreaView, SectionList, StatusBar } from "react-native";

const DATA = [
  {
    category: "Mobile Phones",
    data: ["Galaxy s22", "Xiomi 12 Pro", "I-Phone 15"]
  },
  {
    category: "Refrigerators",
    data: ["Haier Single Door", "LG Double Door", "Godrej Single Door"]
  },
  {
    category: "Dresses",
    data: ["Lymio Multi Color", "Toplot Black Dress", "Keri Perry"]
  },
];

const App = () => (
  <SafeAreaView style={styles.container}>
    <SectionList
      sections={DATA}
      keyExtractor={(item, index) => item + index}
      renderItem={({ item }) => <View style={styles.item}>
                                  <Text style={styles.title}>{item}</Text>
                                </View>}
      renderSectionHeader={({ section: { category } }) => (
        <Text style={styles.header}>{category}</Text>
      )}
    />
  </SafeAreaView>
);

const styles = StyleSheet.create({
  container: {
    flex: 1,
    paddingTop: StatusBar.currentHeight,
    marginHorizontal: 16
  },
  item: {
    backgroundColor: "#006666",
    padding: 20,
    marginVertical: 8,
    flexDirection:'row'
  },
  header: {
    fontSize: 32,
    backgroundColor: "#fff"
  },
  title: {
    fontSize: 24
  }
});

export default App;
```

## SectionList with Columns

```js
import React from "react";
import { StyleSheet, Text, View, SafeAreaView, SectionList, StatusBar, FlatList } from "react-native";

const DATA = [
  {
    category: "Mobile Phones",
    data: [{list:["Galaxy s22", "Xiomi 12 Pro", "I-Phone 15"]}]
  },
  {
    category: "Refrigerators",
    data: [{list:["Haier Single Door", "LG Double Door", "Godrej Single Door"]}]
  },
  {
    category: "Dresses",
    data: [{list:["Lymio Multi Color", "Toplot Black Dress", "Keri Perry"]}]
  },
];

const App = () => (
  <SafeAreaView style={styles.container}>
    <SectionList
      sections={DATA}
      keyExtractor={(item, index) => item + index}
      renderItem={({ item }) => < FlatList horizontal data={item.list} renderItem={({item})=><Text style={styles.item}>{item}</Text>}/>}
      renderSectionHeader={({ section: { category } }) => (
        <Text style={styles.header}>{category}</Text>
      )}
    />
  </SafeAreaView>
);

const styles = StyleSheet.create({
  container: {
    flex: 1,
    paddingTop: StatusBar.currentHeight,
    marginHorizontal: 16
  },
  item: {
    backgroundColor: "#006666",
    padding: 20,
    marginVertical: 8,
    flexDirection:'row',
    margin:10
  },
  header: {
    fontSize: 32,
    backgroundColor: "#fff"
  }
});

export default App;
```

Note: Remember FlatList and SectionList both are PureComponents which means that it will not re-render if props remain shallow-equal.




