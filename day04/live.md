## Creating Custom TextInput using Vector Icons

```ts
// CustomInput.tsx
import React, { useState } from 'react';
import { TextInput, TextInputProps, View, Text, TouchableOpacity, StyleSheet } from 'react-native';
import { MaterialIcons } from '@expo/vector-icons'; // You may need to install the @expo/vector-icons package

interface CustomInputProps extends TextInputProps {
  type?: 'default' | 'email' | 'password';
  label?: string;
}

const CustomInput: React.FC<CustomInputProps> = ({ type = 'default', label, ...rest }) => {
  const [isPasswordVisible, setIsPasswordVisible] = useState(false);

  const handleTogglePasswordVisibility = () => {
    setIsPasswordVisible((prev) => !prev);
  };

  return (
    <View style={{}}>
      {label && <Text>{label}</Text>}
      <View  style={[styles.input, {flexDirection:'row'}]}>
      <TextInput
        secureTextEntry={type === 'password' && !isPasswordVisible}
        keyboardType={type === 'email' ? 'email-address' : 'default'}
        style={{flex:1}}
      />
      {type === 'password' && (
        <TouchableOpacity onPress={handleTogglePasswordVisibility}>
          <MaterialIcons
            name={isPasswordVisible ? 'visibility-off' : 'visibility'}
            size={24}
            color="black"
          />
        </TouchableOpacity>
      )}</View>
    </View>
  );
};

export default CustomInput;

const styles = StyleSheet.create({
    input: {
    height: 40,
    borderColor: 'gray',
    borderWidth: 1,
    marginBottom: 10,
    paddingHorizontal: 10,
    alignItems:'center',
    borderRadius:10
  },
})
```

### Use CustomInput with in a Component

```ts

import { View, Text, StyleSheet, Button } from 'react-native';
import CustomInput from './CustomInput';

const MyForm: React.FC = () => {
  const handleSubmit = () => {
    // Handle form submission logic here
  };


  return (
    <View style={styles.container}>
      <Text style={styles.title}>Login Form</Text>

      <CustomInput type="email" label="Email" placeholder="Enter your email" style={styles.input} />

        <CustomInput
          type="password"
          label="Password"
          placeholder="Enter your password"
          style={styles.passwordInput}
        />

      <Button onPress={handleSubmit} title="Submit" />
    </View>
  );
};

const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
    paddingHorizontal: 20,
  },
  title: {
    fontSize: 24,
    fontWeight: 'bold',
    marginBottom: 20,
    textAlign: 'center',
  },
});

export default MyForm;
```

## Formik Basic Example

```js
// JavaScript example
import React from 'react';
import { View, TextInput, Button, Text } from 'react-native';
import { Formik } from 'formik';

const MyForm = () => {
  return (
    <Formik
      initialValues={{ email: '', password: '' }}
      onSubmit={(values) => {
        // Handle form submission logic here
        console.log(values);
      }}
      validate={(values) => {
        // Implement your form validation logic here
        const errors = {};
        if (!values.email) {
          errors.email = 'Required';
        }
        // Add more validation rules as needed
        return errors;
      }}
    >
      {({ handleChange, handleBlur, handleSubmit, values, errors }) => (
        <View>
          <TextInput
            onChangeText={handleChange('email')}
            onBlur={handleBlur('email')}
            value={values.email}
            placeholder="Email"
          />
          <Text>{errors.email}</Text>

          <TextInput
            onChangeText={handleChange('password')}
            onBlur={handleBlur('password')}
            value={values.password}
            placeholder="Password"
            secureTextEntry
          />
          <Text>{errors.password}</Text>

          <Button onPress={handleSubmit} title="Submit" />
        </View>
      )}
    </Formik>
  );
};

export default MyForm;
```

```ts
// Typescript version of same example
import React from 'react';
import { View, TextInput, Button, Text } from 'react-native';
import { Formik, FormikHelpers } from 'formik';

interface FormValues {
  email: string;
  password: string;
}

const MyForm: React.FC = () => {
  const initialValues: FormValues = { email: '', password: '' };

  const onSubmit = (values: FormValues, { setSubmitting }: FormikHelpers<FormValues>) => {
    // Handle form submission logic here
    console.log(values);
    setSubmitting(false); // You can set this to false when the submission is complete
  };

  const validate = (values: FormValues) => {
    // Implement your form validation logic here
    const errors: Partial<FormValues> = {};
    if (!values.email) {
      errors.email = 'Required';
    }
    // Add more validation rules as needed
    return errors;
  };

  return (
    <Formik
      initialValues={initialValues}
      onSubmit={onSubmit}
      validate={validate}
    >
      {({ handleChange, handleBlur, handleSubmit, values, errors }) => (
        <View>
          <TextInput
            onChangeText={handleChange('email')}
            onBlur={handleBlur('email')}
            value={values.email}
            placeholder="Email"
          />
          <Text>{errors.email}</Text>

          <TextInput
            onChangeText={handleChange('password')}
            onBlur={handleBlur('password')}
            value={values.password}
            placeholder="Password"
            secureTextEntry
          />
          <Text>{errors.password}</Text>

          <Button onPress={handleSubmit} title="Submit" />
        </View>
      )}
    </Formik>
  );
};

export default MyForm;
```
In this example, Formik wraps the form, and the TextInput components are connected to the form state using the provided handleChange and handleBlur functions. The form submission is handled by the onSubmit prop, and the validate prop is used for form validation.

In this TypeScript version, I've defined an interface FormValues to represent the shape of the form values. The onSubmit function now takes FormValues as its first argument, and the validate function returns Partial<FormValues> for the errors.

Make sure you have the @types/react-native package installed, or create a typings.d.ts file with the following content to make TypeScript aware of the FormikHelpers type:

```ts
// typings.d.ts
import { FormikHelpers } from 'formik';

declare module 'formik' {
  export interface FormikHelpers<Values> extends FormikActions<Values> {}
}
```
This ensures that TypeScript recognizes FormikHelpers correctly in the context of Formik in React Native.

## Basic Yup example

```ts
import * as Yup from 'yup';

const schema = Yup.object().shape({
  name: Yup.string().required('Name is required'),
  age: Yup.number().positive('Age must be a positive number').integer('Age must be an integer'),
  email: Yup.string().email('Invalid email address').required('Email is required'),
});

// Example usage
const data: {
  name: string;
  age: number;
  email: string;
} = {
  name: 'Subrata Das',
  age: 25,
  email: 'subrata@example.com',
};

schema.validate(data)
  .then(validData => console.log(validData))
  .catch(errors => console.error(errors));
```

In this example, the Yup schema defines rules for a data object with properties name, age, and email. The validate method is then used to check whether the provided data adheres to the defined schema. If there are validation errors, it returns an array of error messages. If the data is valid, it returns the validated data.

## Formik + Yup

```ts
import React from "react";
import { View, TextInput, Button, Text, StyleSheet } from "react-native";
import { Formik, Field, FieldProps } from "formik";
import * as Yup from "yup";

// Define validation schema using Yup
const validationSchema = Yup.object().shape({
  name: Yup.string().required("Name is required"),
  email: Yup.string()
    .email("Invalid email address")
    .required("Email is required"),
  password: Yup.string()
    // .min(6, "Password must be at least 6 characters")
    .required("Password is required")
    .matches(
      /^(?=.*[a-z])(?=.*[A-Z])(?=.*\W).{8,}$/,
      "Password must be at least 8 characters and contain one lowercase letter, one uppercase letter, and one special character",
    ),
});

// Define form initial values
const initialValues = {
  name: "",
  email: "",
  password: "",
};

// Form component
const MyForm: React.FC = () => {
  const handleSubmit = (values: typeof initialValues) => {
    // Handle form submission logic here
    console.log(values);
  };

  return (
    <Formik
      initialValues={initialValues}
      onSubmit={handleSubmit}
      validationSchema={validationSchema}
    >
      {({ handleChange, handleBlur, handleSubmit, values, errors }) => (
        <View style={styles.container}>
          <Field
            name="name"
            render={({ field }: FieldProps<(typeof initialValues)["name"]>) => (
              <>
                <TextInput
                  style={styles.input}
                  onChangeText={handleChange("name")}
                  onBlur={handleBlur("name")}
                  value={values.name}
                  placeholder="Name"
                />
                <Text style={styles.errText}>{errors.name}</Text>
              </>
            )}
          />

          <Field
            name="email"
            render={({
              field,
            }: FieldProps<(typeof initialValues)["email"]>) => (
              <>
                <TextInput
                  style={styles.input}
                  onChangeText={handleChange("email")}
                  onBlur={handleBlur("email")}
                  value={values.email}
                  placeholder="Email"
                />
                <Text style={styles.errText}>{errors.email}</Text>
              </>
            )}
          />

          <Field
            name="password"
            render={({
              field,
            }: FieldProps<(typeof initialValues)["password"]>) => (
              <>
                <TextInput
                  style={styles.input}
                  onChangeText={handleChange("password")}
                  onBlur={handleBlur("password")}
                  value={values.password}
                  placeholder="Password"
                  secureTextEntry
                />
                <Text style={[styles.errText, { marginBottom: 20 }]}>
                  {errors.password}
                </Text>
              </>
            )}
          />

          <Button onPress={handleSubmit} title="Submit" />
        </View>
      )}
    </Formik>
  );
};

export default MyForm;

const styles = StyleSheet.create({
  container: {
    flex: 1,
    padding: "10%",
    alignSelf: "center",
    justifySelf: "center",
  },
  input: {
    height: 40,
    marginVertical: 12,
    borderWidth: 1,
    padding: 10,
    borderRadius: 5,
  },
  errText: {
    color: "red",
  },
});

```

