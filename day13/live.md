## Current Location

```js
import React, { useState, useEffect } from 'react';
import { View, Text } from 'react-native';
import * as Location from 'expo-location';

const CurrentLocationExample = () => {
  const [location, setLocation] = useState(null);
  const [errorMsg, setErrorMsg] = useState(null);

  useEffect(() => {
    (async () => {
      let { status } = await Location.requestForegroundPermissionsAsync();
      if (status !== 'granted') {
        setErrorMsg('Permission to access location was denied');
        return;
      }

      let location = await Location.getCurrentPositionAsync({});
      setLocation(location);
    })();
  }, []);

  return (
    <View style={{ flex: 1, justifyContent: 'center', alignItems: 'center' }}>
      {errorMsg && <Text>{errorMsg}</Text>}
      {location && (
        <Text>
          Latitude: {location.coords.latitude}, Longitude: {location.coords.longitude}
        </Text>
      )}
    </View>
  );
};

export default CurrentLocationExample;
```
## GeoCoding Example

```js
import React, { useState, useEffect } from 'react';
import { View, Text } from 'react-native';
import * as Location from 'expo-location';

const GeocodingExample = () => {
  const [address, setAddress] = useState(null);

  useEffect(() => {
    (async () => {
      let location = await Location.reverseGeocodeAsync({ latitude: 37.78825, longitude: -122.4324 });
      setAddress(location[0]);
    })();
  }, []);

  return (
    <View style={{ flex: 1, justifyContent: 'center', alignItems: 'center' }}>
      {address && (
        <Text>
          {address.name}, {address.street}, {address.city}, {address.region} {address.postalCode}, {address.country}
        </Text>
      )}
    </View>
  );
};

export default GeocodingExample;
```

## Geo Fencing

```js
import React, { useEffect } from 'react';
import { View, Text } from 'react-native';
import * as Location from 'expo-location';

const GeofencingExample = () => {
  useEffect(() => {
    (async () => {
      const geofence = await Location.startGeofencingAsync('myGeofence', [
        {
          latitude: 37.78825,
          longitude: -122.4324,
          radius: 100,
        },
      ]);
      console.log('Geofencing started:', geofence);
    })();
  }, []);

  return (
    <View style={{ flex: 1, justifyContent: 'center', alignItems: 'center' }}>
      <Text>Geofencing started</Text>
    </View>
  );
};

export default GeofencingExample;
```

## Background Location Updates:

```js
import React, { useEffect } from 'react';
import { View, Text } from 'react-native';
import * as Location from 'expo-location';

const BackgroundLocationExample = () => {
  useEffect(() => {
    (async () => {
      let { status } = await Location.requestBackgroundPermissionsAsync();
      if (status === 'granted') {
        Location.startLocationUpdatesAsync('myBackgroundTask', {
          accuracy: Location.Accuracy.Balanced,
        });
      }
    })();
  }, []);

  return (
    <View style={{ flex: 1, justifyContent: 'center', alignItems: 'center' }}>
      <Text>Background location updates started</Text>
    </View>
  );
};

export default BackgroundLocationExample;
```

# Maps

MapView allows developers to display maps from various providers such as Google Maps, Apple Maps, and OpenStreetMap. Developers can choose the desired map provider and customize the appearance of the map.

## Displaying Maps

```js
import React from 'react';
import { StyleSheet, View } from 'react-native';
import MapView from 'react-native-maps';

const MapExample = () => {
  return (
    <View style={styles.container}>
      <MapView
        style={styles.map}
        initialRegion={{
          latitude: 37.78825,
          longitude: -122.4324,
          latitudeDelta: 0.0922,
          longitudeDelta: 0.0421,
        }}
      />
    </View>
  );
};

const styles = StyleSheet.create({
  container: {
    flex: 1,
  },
  map: {
    flex: 1,
  },
});

export default MapExample;
```

## Specifying Custom Provider

```js
import React from 'react';
import { StyleSheet, View } from 'react-native';
import MapView, { PROVIDER_OSMDROID } from 'react-native-maps';

const OpenStreetMapExample = () => {
  return (
    <View style={styles.container}>
      <MapView
        style={styles.map}
        provider={PROVIDER_OSMDROID} // Specify OpenStreetMap as the map provider
        initialRegion={{
          latitude: 37.78825,
          longitude: -122.4324,
          latitudeDelta: 0.0922,
          longitudeDelta: 0.0421,
        }}
      />
    </View>
  );
};

const styles = StyleSheet.create({
  container: {
    flex: 1,
  },
  map: {
    flex: 1,
  },
});

export default OpenStreetMapExample;
```

## Displaying Markers

```js
import React from 'react';
import { StyleSheet, View } from 'react-native';
import MapView, { Marker } from 'react-native-maps';

const MapWithMarkersExample = () => {
  return (
    <View style={styles.container}>
      <MapView
        style={styles.map}
        initialRegion={{
          latitude: 37.78825,
          longitude: -122.4324,
          latitudeDelta: 0.0922,
          longitudeDelta: 0.0421,
        }}
      >
        <Marker
          coordinate={{ latitude: 37.78825, longitude: -122.4324 }}
          title="Marker Title"
          description="Marker Description"
        />
      </MapView>
    </View>
  );
};

const styles = StyleSheet.create({
  container: {
    flex: 1,
  },
  map: {
    flex: 1,
  },
});

export default MapWithMarkersExample;
```

## Displaying Polylines

MapView allows developers to draw polylines on the map to represent routes, boundaries, or any other lines of interest.

```js
import React from 'react';
import { StyleSheet, View } from 'react-native';
import MapView, { Polyline } from 'react-native-maps';

const MapWithPolylineExample = () => {
  const coordinates = [
    { latitude: 37.8025259, longitude: -122.4351431 },
    { latitude: 37.7896386, longitude: -122.421646 },
    { latitude: 37.7665248, longitude: -122.4161628 },
    { latitude: 37.7734153, longitude: -122.4577787 },
    { latitude: 37.7948605, longitude: -122.4596065 },
  ];

  return (
    <View style={styles.container}>
      <MapView
        style={styles.map}
        initialRegion={{
          latitude: 37.78825,
          longitude: -122.4324,
          latitudeDelta: 0.0922,
          longitudeDelta: 0.0421,
        }}
      >
        <Polyline
          coordinates={coordinates}
          strokeColor="#000" // Line color
          strokeWidth={2} // Line width
        />
      </MapView>
    </View>
  );
};

const styles = StyleSheet.create({
  container: {
    flex: 1,
  },
  map: {
    flex: 1,
  },
});

export default MapWithPolylineExample;
```

## Events Handling

MapView provides event handling capabilities, allowing developers to respond to user interactions such as taps, long presses, and map movements.

```js
import React from 'react';
import { StyleSheet, View, Alert } from 'react-native';
import MapView from 'react-native-maps';

const MapWithEventsExample = () => {
  const handleMapPress = (event) => {
    const { latitude, longitude } = event.nativeEvent.coordinate;
    Alert.alert(`Map Pressed at: ${latitude}, ${longitude}`);
  };

  return (
    <View style={styles.container}>
      <MapView
        style={styles.map}
        onPress={handleMapPress}
        initialRegion={{
          latitude: 37.78825,
          longitude: -122.4324,
          latitudeDelta: 0.0922,
          longitudeDelta: 0.0421,
        }}
      />
    </View>
  );
};

const styles = StyleSheet.create({
  container: {
    flex: 1,
  },
  map: {
    flex: 1,
  },
});

export default MapWithEventsExample;
```




