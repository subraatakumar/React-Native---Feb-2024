# Sign-in and Sign-up with Enhanced Features

## Objective:

Develop a React Native app that provides user authentication through sign-in and sign-up functionalities.

## Requirements:

### Sign-in Page:

- Sign-in with Email:
  - Implement a form with email address and password input fields.
  - Validate email format using a regular expression or a library.
  - Display an error message:
    - if the email or password is incorrect.
    - if the email or password is empty.
    - if the password does not contain 1 lowercase letter, 1 uppercase letter, 1 number, 1 special character
    - if the password length is below 8 characters

- Sign-in with Google:
  - Integrate Google Sign-in using the @react-native-google-signin/google-signin library.
  - Retrieve user information like display name, email, and profile picture upon successful sign-in.
  - Store this user information securely in your app's state management solution (e.g., Redux, Context API).

- Forgot Password:
  - Provide a clear link or button that navigates to a password reset screen (optional implementation for this assignment).

- Don't have an account:
  - navigates to the sign-up page.

### Sign-up Page:

- Sign-up with Email:
  - Implement a form with display name, email address, password, and profile picture input fields.
  - Enforce password validation rules (e.g., minimum length, character requirements as given in signin page).
  - Display a success message upon successful sign-up or indicate any errors encountered.
  - Consider offering optional fields like phone number or date of birth (depending on your app's requirements).

- Sign-up with Google:
  - Enable Google Sign-up using the same library as in sign-in for consistency.
- Already have an account:
  - navigates to the sign-in page.

### UI Design:

**Responsiveness:** Ensure the app's layout adapts to different screen sizes and device orientations.
**Accessibility:** Adhere to accessibility guidelines (e.g., WCAG) by providing clear labels, sufficient contrast, and support for assistive technologies(optional for this assignment).
**Theme:** Create a visually appealing and user-friendly theme that aligns with your app's branding (optional for this assignment).

### State Management:

Choose a state management solution (Redux, Context API, etc.) to maintain user information and authentication state throughout the app.

### Bonus Challenges:

- **Social Login:** Implement sign-in/up with other social media providers (Facebook, Twitter).
- **Password Reset:** Design a functional password reset flow (email verification, new password input).
- **Image Upload:** Allow users to upload their profile pictures from their device's storage.

