# Useing Firebase on React Native and Expo project

### Create a Firebase Project

- goto `https://console.firebase.google.com/u/0/`. Login through your google account
- click on `add project`, enter your project name and click continue.
- deselect `Enable Google Analytics for this project`, and click on `create project`.
- It will take some time to create the project, Then `Your new project is ready` message will be displayed with a `continue` button. Click on that.
- On left side of dashboard, click on `Project Overview`. Then click on android icon, which says `add an app to get started`.
- Write android package name. This you will get from `/android/app/src/main/AndroidManifest.xml` with in your project.
- Write an optional app nick name, and click on `register app` button.
- Download `google-services.json`, and move to `/android/app/` directory.
- Then follow the steps given in `Add Firebase SDK`.

## [Prerequisites](https://rnfirebase.io/#prerequisites)

## [Installation for react native project](https://rnfirebase.io/#installation-for-react-native-cli-projects)

## [use it on Expo](https://rnfirebase.io/#expo)

# [Use Firebase Authentication](https://rnfirebase.io/auth/usage)


## Firebase Console Setup

- Goto `All Products` -> `Authentication` -> `Sign In Method` , then choose the methods. For this project select `google` and `email and password`.
- As you have already created project now add `SHA1` key to it. `Project settings` -> `Add Fingerprint` -> paste `SHA1` -> `SAVE`.
- If you have not generated `SHA1`, then generate it by `cd android && ./gradlew signingReport`.
- Download this new `google-services.json` and paste in `android/app` folder.


## [Email and Password Signin](https://rnfirebase.io/auth/usage#emailpassword-sign-in)

## [Google Signin](https://rnfirebase.io/auth/social-auth#google)

## [Sign Out](https://rnfirebase.io/auth/usage#signing-out)



