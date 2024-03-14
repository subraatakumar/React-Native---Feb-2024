# Track phone tilting horizontally and vertically

```js
import React, { useState, useEffect } from 'react';
import { Text, View, StyleSheet } from 'react-native';
import { Accelerometer } from 'expo-sensors';

const App = () => {
  const [tilt, setTilt] = useState('Center');
  const [standing, setStanding] = useState("Sleeping")

  useEffect(() => {
    const subscription = Accelerometer.addListener(data => {
      const { x, y } = data;

      // Adjust sensitivity based on your needs
      const threshold = 0.3; // Adjust this value to determine tilt sensitivity

      if (x > threshold) {
        setTilt('Right');
      } else if (x < -threshold) {
        setTilt('Left');
      } else {
        setTilt('Center');
      }
      if(y > threshold){
        setStanding("Standing")
      } else if (y < -threshold) {
        setStanding("Standing Head Down")
      }else{
        setStanding("Sleeping")
      }
    });

    return () => subscription.remove();
  }, []);

  return (
    <View style={styles.container}>
      <Text style={styles.text}>{tilt}</Text>
      <Text style={styles.text}>{standing}</Text>
    </View>
  );
};

const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
  },
  text: {
    fontSize: 32,
    fontWeight: 'bold',
  },
});

export default App;
```

- For basic tilt detection (left or right), x and y axes are sufficient.
- The z-axis plays a vital role in applications that require more sophisticated motion detection or 3D orientation tracking.

# Basic Camera App

```js
import { Camera, CameraType } from 'expo-camera';
import { useState } from 'react';
import {
  Button,
  StyleSheet,
  Text,
  TouchableOpacity,
  View,
  Image,
} from 'react-native';

export default function App() {
  const [type, setType] = useState(CameraType.back);
  const [photos, setPhotos] = useState([{ uri: '' }]);
  const [cameraRef, setCameraRef] = useState(null);
  const [permission, requestPermission] = Camera.useCameraPermissions();

  if (!permission) {
    // Camera permissions are still loading
    return <View />;
  }

  if (!permission.granted) {
    // Camera permissions are not granted yet
    return (
      <View style={styles.container}>
        <Text style={{ textAlign: 'center' }}>
          We need your permission to show the camera
        </Text>
        <Button onPress={requestPermission} title="grant permission" />
      </View>
    );
  }

  function toggleCameraType() {
    setType((current) =>
      current === CameraType.back ? CameraType.front : CameraType.back
    );
  }

  return (
    <View style={styles.container}>
      <Camera
        style={styles.camera}
        type={type}
        ref={(ref) => {
          setCameraRef(ref);
        }}>
        <View style={styles.buttonContainer}>
          <TouchableOpacity style={styles.button} onPress={toggleCameraType}>
            <Text style={styles.text}>Flip Camera</Text>
          </TouchableOpacity>
        </View>
        <View style={styles.buttonContainer}>
          <TouchableOpacity
            style={styles.button}
            onPress={async () => {
              if (cameraRef) {
                let photo = await cameraRef.takePictureAsync();
                setPhotos((prev) => [...prev, photo]);
              }
            }}>
            <Text style={styles.text}>Take Photo</Text>
          </TouchableOpacity>
        </View>
        <View style={{flexDirection:'row', margin:20}}>
          {photos.map((photo) =>
            photo?.uri ? (
              <Image
                source={{ uri: photo?.uri }}
                style={{ width: 50, height: 50, margin:2 }}
              />
            ) : (
              <></>
            )
          )}
        </View>
      </Camera>
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
  },
  camera: {
    flex: 1,
  },
  buttonContainer: {
    flex: 1,
    flexDirection: 'row',
    backgroundColor: 'transparent',
    margin: 64,
  },
  button: {
    flex: 1,
    alignSelf: 'flex-end',
    alignItems: 'center',
  },
  text: {
    fontSize: 24,
    fontWeight: 'bold',
    color: 'white',
  },
});
```

# Barcode Reader App

```js
import { Camera, CameraType } from 'expo-camera';
import { useState } from 'react';
import {
  Button,
  StyleSheet,
  Text,
  TouchableOpacity,
  View,
  Image,
} from 'react-native';

export default function App() {
  const [type, setType] = useState(CameraType.back);
  const [barcode, setBarcode] = useState(null);
  const [cameraRef, setCameraRef] = useState(null);
  const [permission, requestPermission] = Camera.useCameraPermissions();

  if (!permission) {
    // Camera permissions are still loading
    return <View />;
  }

  if (!permission.granted) {
    // Camera permissions are not granted yet
    return (
      <View style={styles.container}>
        <Text style={{ textAlign: 'center' }}>
          We need your permission to show the camera
        </Text>
        <Button onPress={requestPermission} title="grant permission" />
      </View>
    );
  }

  function toggleCameraType() {
    setType((current) =>
      current === CameraType.back ? CameraType.front : CameraType.back
    );
  }

  return (
    <View style={styles.container}>
      {!barcode ? (
        <Camera
          style={styles.camera}
          type={type}
          ref={(ref) => {
            setCameraRef(ref);
          }}
          onBarCodeScanned={(barcode) => {
            setBarcode(barcode);
          }}>
          <View style={styles.buttonContainer}>
            <TouchableOpacity style={styles.button} onPress={toggleCameraType}>
              <Text style={styles.text}>Flip Camera</Text>
            </TouchableOpacity>
          </View>
        </Camera>
      ) : (
        <View style={{justifyContent:'center'}}>
          <Text>Scanned Data: {barcode?.data}</Text>
          <TouchableOpacity onPress={() => setBarcode(null)} style={{backgroundColor:'red', borderRadius:10, padding:10, margin:20}}>
            <Text style={{textAlign:'center', color:'#FFF'}}>Scan Again</Text>
          </TouchableOpacity>
        </View>
      )}
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
  },
  camera: {
    flex: 1,
  },
  buttonContainer: {
    flex: 1,
    flexDirection: 'row',
    backgroundColor: 'transparent',
    margin: 64,
  },
  button: {
    flex: 1,
    alignSelf: 'flex-end',
    alignItems: 'center',
  },
  text: {
    fontSize: 24,
    fontWeight: 'bold',
    color: 'white',
  },
});
```
