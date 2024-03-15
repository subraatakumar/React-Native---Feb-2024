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



