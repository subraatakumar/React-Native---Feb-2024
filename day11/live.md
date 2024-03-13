```js
import React, { useEffect, useState } from "react";
import { useSelector, useDispatch } from "react-redux";
import { increment, decrement } from "./src/redux/counterSlice";
import { Button, View, Text, TouchableOpacity, TextInput } from "react-native";
import auth from "@react-native-firebase/auth";

function App1() {
  const [isLoggedIn, setIsLoggedIn] = useState(false);
  const [hasAccount, setHasAccount] = useState(false);
  const [email, setEmail] = useState("");
  const [password, setPassword] = useState("");
  const { value } = useSelector((state) => state.counter);
  const dispatch = useDispatch();

  useEffect(() => {
    // auth().onAuthStateChanged((user) => {
    //   if (user) {
    //     setIsLoggedIn(true);
    //   }
    // });

    const user = auth().currentUser;
    if (user) {
      setIsLoggedIn(true);
    } else {
      setIsLoggedIn(false);
    }
  }, []);
  const handleLogin = () => {
    console.log("handleLogin", email, password);
    auth()
      .signInWithEmailAndPassword(email, password)
      .then(() => {
        setIsLoggedIn(true);
        setEmail("");
        setPassword("");
      });
  };

  const handleSignup = () => {
    console.log("handleSignup", email, password);

    auth()
      .createUserWithEmailAndPassword(email, password)
      .then(() => {
        console.log("User account created & signed in!");
        setIsLoggedIn(true);
      })
      .catch((error) => {
        if (error.code === "auth/email-already-in-use") {
          console.log("That email address is already in use!");
        }

        if (error.code === "auth/invalid-email") {
          console.log("That email address is invalid!");
        }

        console.error(error);
      });
  };
  return (
    <View>
      {isLoggedIn ? (
        <View style={{ marginTop: 40 }}>
          <Button onPress={() => dispatch(increment())} title="Increment " />
          <Text style={{ fontSize: 25, textAlign: "center" }}>{value}</Text>
          <Button onPress={() => dispatch(decrement())} title="Decrement " />

          <TouchableOpacity>
            <Text
              onPress={() => {
                auth()
                  .signOut()
                  .then(() => setIsLoggedIn(false));
              }}
              style={{ textAlign: "center", marginTop: 50 }}
            >
              Logout
            </Text>
          </TouchableOpacity>
        </View>
      ) : (
        <View>
          <TextInput
            style={{ borderRadius: 10, borderWidth: 1, margin: 10 }}
            placeholder="Email"
            value={email}
            onChangeText={setEmail}
          />
          <TextInput
            style={{ borderRadius: 10, borderWidth: 1, margin: 10 }}
            placeholder="Password"
            value={password}
            onChangeText={setPassword}
            secureTextEntry
          />
          <View style={{ margin: 10 }}>
            {hasAccount ? (
              <Button title="Login" onPress={handleLogin} />
            ) : (
              <Button title="Signup" onPress={handleSignup} />
            )}
          </View>
          <TouchableOpacity onPress={() => setHasAccount((prev) => !prev)}>
            {hasAccount ? (
              <Text style={{ textAlign: "center" }}>
                Don't have account? Create One.
              </Text>
            ) : (
              <Text style={{ textAlign: "center" }}>has account, login?</Text>
            )}
          </TouchableOpacity>
          <Text style={{ color: "red", fontSize: 49 }}>
            Please login to use counter app
          </Text>
        </View>
      )}
    </View>
  );
}

export default App1;

```
