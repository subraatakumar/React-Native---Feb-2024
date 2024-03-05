

# FlatList Example

```js
import React from 'react';
import {
  SafeAreaView,
  View,
  FlatList,
  StyleSheet,
  Text,
  StatusBar,
} from 'react-native';

const DATA = [
  {
    id: 'bd7acbea-c1b1-46c2-aed5-3ad53abb28ba',
    title: 'First Item',
  },
  {
    id: '3ac68afc-c605-48d3-a4f8-fbd91aa97f63',
    title: 'Second Item',
  },
  {
    id: '58694a0f-3da1-471f-bd96-145571e29d72',
    title: 'Third Item',
  },
  {
    id: '58694a0f-3da1-471f-bd96-145571e29d72',
    title: 'Fourth Item',
  },
  {
    id: '58694a0f-3da1-471f-bd96-145571e29d72',
    title: 'Fifth Item',
  },
  {
    id: '58694a0f-3da1-471f-bd96-145571e29d72',
    title: 'Sixth Item',
  },
  {
    id: '58694a0f-3da1-471f-bd96-145571e29d72',
    title: 'Seventh Item',
  },
  {
    id: '58694a0f-3da1-471f-bd96-145571e29d72',
    title: 'Eighth Item',
  },
  {
    id: '58694a0f-3da1-471f-bd96-145571e29d72',
    title: 'Nineth Item',
  },
  {
    id: '58694a0f-3da1-471f-bd96-145571e29d72',
    title: 'Tenth Item',
  },
  {
    id: '58694a0f-3da1-471f-bd96-145571e29d72',
    title: 'Eleventh Item',
  },
  {
    id: '58694a0f-3da1-471f-bd96-145571e29d72',
    title: 'Tweleveth Item',
  },
  {
    id: '58694a0f-3da1-471f-bd96-145571e29d72',
    title: 'Thirteenth Item',
  },
  {
    id: '58694a0f-3da1-471f-bd96-145571e29d72',
    title: 'Fourteenth Item',
  },
];

const Data1 = [];

const Item = ({ title }) => (
  <View style={styles.item}>
    <Text style={styles.title}>{title}</Text>
  </View>
);

const App = () => {
  return (
    <SafeAreaView style={styles.container}>
      <FlatList
        data={DATA}
        numColumns={3}
        ItemSeparatorComponent={() => (
          <View
            style={{
              height: 10,
              width: '100%',
              backgroundColor: 'green',
            }}></View>
        )}
        ListHeaderComponent={() => (
          <View
            style={{
              width: '100%',
              padding: 30,
              backgroundColor: 'green',
            }}>
            <Text style={{ color: '#FFFFFF', fontSize: 45, textAlign:'center' }}>
              This is Header
            </Text>
          </View>
        )}
        ListEmptyComponent={() => (
          <View
            style={{
              width: '100%',
              padding: 30
            }}>
            <Text style={{ color: 'red', fontSize: 45, textAlign:'center' }}>
              List is Empty 
            </Text>
          </View>
        )}
        renderItem={({ item }) => <Item title={item.title} />}
        keyExtractor={(item) => item.id}
      />
    </SafeAreaView>
  );
};

const styles = StyleSheet.create({
  container: {
    flex: 1,
    marginTop: StatusBar.currentHeight || 0,
  },
  item: {
    flex:1,
    backgroundColor: '#f9c2ff',
    padding: 10,
    marginVertical: 8,
    marginHorizontal: 16,
  },
  title: {
    fontSize: 15,
  },
});

export default App;
```
