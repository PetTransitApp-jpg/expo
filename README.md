+PetTransit_App
│ App.js
│ package.json
│ ...
├── components
│     ├── AppointmentCard.js
│     └── PetUpdateCard.js
├── screens
│     ├── HomeScreen.js
│     ├── CreateAppointment.js
│     ├── ConfirmPickup.js
│     ├── TermsScreen.js
│     └── PaymentScreen.js
├── data
│     └── TermsText.js
└── services
      └── firebase.js
// services/firebase.js
import { initializeApp } from "firebase/app";
import { getFirestore } from "firebase/firestore";
import { getStorage } from "firebase/storage";

const firebaseConfig = {
  apiKey: "YOUR KEY",
  authDomain: "YOUR AUTH DOMAIN",
  projectId: "YOUR PROJECT ID",
  storageBucket: "YOUR BUCKET",
  messagingSenderId: "YOUR ID",
  appId: "YOUR APP ID"
};

const app = initializeApp(firebaseConfig);

export const db = getFirestore(app);
export const storage = getStorage(app);

import { View, Text, Button } from "react-native";

export default function HomeScreen({ navigation }) {
  return (
    <View style={{ padding: 20 }}>
      <Text style={{ fontSize: 28, fontWeight: "bold" }}>Pet Transit Service</Text>
      <Text>Your trusted pet transportation.</Text>

      <Button 
        title="Create Appointment" 
        onPress={() => navigation.navigate("CreateAppointment")} 
      />
      <Button 
        title="Terms & Agreement" 
        onPress={() => navigation.navigate("Terms")} 
      />
    </View>
  );
}import { useState } from "react";
import { View, Text, TextInput, Button } from "react-native";
import { db } from "../services/firebase";
import { addDoc, collection } from "firebase/firestore";

export default function CreateAppointment({ navigation }) {
  const [name, setName] = useState("");
  const [pet, setPet] = useState("");
  const [pickupTime, setPickupTime] = useState("");
  const [nailTrim, setNailTrim] = useState(false);

  const createAppt = async () => {
    await addDoc(collection(db, "appointments"), {
      name,
      pet,
      pickupTime,
      nailTrim,
      status: "pending"
    });
    navigation.navigate("Home");
  };

  return (
    <View style={{ padding: 20 }}>
      <Text>Create a Pet Transit Appointment</Text>

      <TextInput placeholder="Your Name" onChangeText={setName} />
      <TextInput placeholder="Pet Name" onChangeText={setPet} />
      <TextInput placeholder="Preferred Pickup Time" onChangeText={setPickupTime} />

      <Button title="Submit Appointment" onPress={createAppt} />
    </View>
  );
}import { View, Text, Button } from "react-native";
import { db } from "../services/firebase";
import { updateDoc, doc } from "firebase/firestore";

export default function ConfirmPickup({ route }) {
  const { appt } = route.params;

  const confirm = async () => {
    await updateDoc(doc(db, "appointments", appt.id), {
      status: "confirmed"
    });
  };

  return (
    <View style={{ padding: 20 }}>
      <Text>Confirm Appointment</Text>
      <Text>{appt.name}</Text>
      <Text>{appt.pet}</Text>
      <Text>{appt.pickupTime}</Text>

      <Button title="Confirm Pickup" onPress={confirm} />
    </View>
  );
}import * as ImagePicker from 'expo-image-picker';
import { ref, uploadBytes, getDownloadURL } from "firebase/storage";
import { storage } from "../services/firebase";

const pickImage = async () => {
  let result = await ImagePicker.launchCameraAsync();

  if (!result.canceled) {
    const response = await fetch(result.assets[0].uri);
    const blob = await response.blob();

    const storageRef = ref(storage, `updates/${Date.now()}.jpg`);
    await uploadBytes(storageRef, blob);
    const url = await getDownloadURL(storageRef);

    // store URL in Firestore...
  }
};npm install @stripe/stripe-react-native
export const terms = `
By using PetTransit, I acknowledge:
- I understand that PetTransit is not liable for injury, illness, escape, or accidents.
- I confirm that my pet is healthy and safe for transit.
- I authorize PetTransit to transport my pet.
- I release PetTransit from all liability related to transportation.
`;import { ScrollView, Text } from "react-native";
import { terms } from "../data/TermsText";

export default function TermsScreen() {
  return (
    <ScrollView style={{ padding: 20 }}>
      <Text style={{ fontSize: 22, fontWeight: "bold" }}>Terms & Agreement</Text>
      <Text>{terms}</Text>
    </ScrollView>
  );
}
<p align="center">
  <a href="https://expo.dev/">
    <img alt="Expo logo" height="128" src="./.github/resources/banner.png">
    <h1 align="center">Expo</h1>
  </a>
</p>

<p align="center">
   <a aria-label="SDK version" href="https://www.npmjs.com/package/expo" target="_blank">
    <img alt="Expo SDK version" src="https://img.shields.io/npm/v/expo.svg?style=flat-square&label=SDK&labelColor=000000&color=4630EB" />
  </a>
  <a aria-label="Chat or ask a question" href="https://chat.expo.dev" target="_blank">
    <img alt="Chat or ask a question" src="https://img.shields.io/discord/695411232856997968.svg?style=flat-square&labelColor=000000&color=4630EB&logo=discord&logoColor=FFFFFF&label=Chat%20with%20us" />
  </a>
  <a aria-label="Expo is free to use" href="https://github.com/expo/expo/blob/main/LICENSE" target="_blank">
    <img alt="License: MIT" src="https://img.shields.io/badge/License-MIT-success.svg?style=flat-square&color=33CC12" target="_blank" />
  </a>
  <a aria-label="expo downloads" href="http://www.npmtrends.com/expo" target="_blank">
    <img alt="Downloads" src="https://img.shields.io/npm/dm/expo.svg?style=flat-square&labelColor=gray&color=33CC12&label=Downloads" />
  </a>
</p>

<p align="center">
  <a aria-label="try expo with snack" href="https://snack.expo.dev"><b>Try Expo in the Browser</b></a>
&ensp;•&ensp;
  <a aria-label="expo documentation" href="https://docs.expo.dev">Read the Documentation</a>
&ensp;•&ensp;
  <a aria-label="expo documentation" href="https://expo.dev/blog">Learn more on our blog</a>
&ensp;•&ensp;
  <a aria-label="expo documentation" href="https://expo.canny.io/feature-requests">Request a feature</a>
</p>

<h6 align="center">Follow us on</h6>
<p align="center">
  <a aria-label="Follow @expo on X" href="https://x.com/intent/follow?screen_name=expo" target="_blank">
    <img alt="Expo on X" src="https://img.shields.io/badge/X-000000?style=for-the-badge&logo=x&logoColor=white" target="_blank" />
  </a>&nbsp;
  <a aria-label="Follow @expo on GitHub" href="https://github.com/expo" target="_blank">
    <img alt="Expo on GitHub" src="https://img.shields.io/badge/GitHub-222222?style=for-the-badge&logo=github&logoColor=white" target="_blank" />
  </a>&nbsp;
  <a aria-label="Follow @expo on Reddit" href="https://www.reddit.com/r/expo/" target="_blank">
    <img alt="Expo on Reddit" src="https://img.shields.io/badge/Reddit-FF4500?style=for-the-badge&logo=reddit&logoColor=white" target="_blank" />
  </a>&nbsp;
  <a aria-label="Follow @expo on Bluesky" href="https://bsky.app/profile/expo.dev" target="_blank">
    <img alt="Expo on Bluesky" src="https://img.shields.io/badge/Bluesky-1DA1F2?style=for-the-badge&logo=bluesky&logoColor=white" target="_blank" />
  </a>&nbsp;
  <a aria-label="Follow @expo on LinkedIn" href="https://www.linkedin.com/company/expo-dev" target="_blank">
    <img alt="Expo on LinkedIn" src="https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white" target="_blank" />
  </a>
</p>

## Introduction

Expo is an open-source platform for making universal native apps that run on Android, iOS, and the web. It includes a universal runtime and libraries that let you build native apps by writing React and JavaScript.

This repository includes the Expo SDK, Modules API, Go app, CLI, Router, documentation, and various other supporting tools. [Expo Application Services (EAS)](https://expo.dev/eas) is a platform of hosted services that are deeply integrated with Expo open source tools. EAS helps you build, ship, and iterate on your app as an individual or a team.

Read the [Expo Community Guidelines](https://expo.dev/guidelines) before interacting in the repository. Thank you for helping keep the Expo community open and welcoming!

## Table of contents

- [📚 Documentation](#-documentation)
- [🗺 Project Layout](#-project-layout)
- [🏅 Badges](#-badges)
- [👏 Contributing](#-contributing)
- [❓ FAQ](#-faq)
- [💙 The Team](#-the-team)
- [License](#license)

## 📚 Documentation

<p>Learn about building and deploying universal apps <a aria-label="expo documentation" href="https://docs.expo.dev">in our official docs!</a></p>

- [Getting Started](https://docs.expo.dev/)
- [API Reference](https://docs.expo.dev/versions/latest/)
- [Using Custom Native Modules](https://docs.expo.dev/workflow/customizing/)

## 🗺 Project Layout

- [`packages`](/packages) All the source code for Expo modules, if you want to edit a library or just see how it works this is where you'll find it.
- [`apps`](/apps) This is where you can find Expo projects which are linked to the development modules. You'll do most of your testing in here.
- [`apps/expo-go`](/apps/expo-go) This is where you can find the source code for Expo Go.
- [`apps/expo-go/ios/Exponent.xcworkspace`](/apps/expo-go/ios) is the Xcode workspace. When developing iOS, always open this instead of `Exponent.xcodeproj` because the workspace also loads the CocoaPods dependencies.
- [`docs`](/docs) The source code for **https://docs.expo.dev**
- [`templates`](/templates) The template projects you get when you run `npx create-expo-app`
- [`react-native-lab`](/react-native-lab) This is our fork of `react-native` used to build Expo Go.
- [`guides`](/guides) In-depth tutorials for advanced topics like contributing to the client.
- [`tools`](/tools) contain build and configuration tools.
- [`template-files`](/template-files) contains templates for files that require private keys. They are populated using the keys in `template-files/keys.json`.
- [`template-files/ios/dependencies.json`](/template-files/ios/dependencies.json) specifies the CocoaPods dependencies of the app.

## 🏅 Badges

Let everyone know your app can be run instantly in the _Expo Go_ app!
<br/>

[![runs with Expo Go](https://img.shields.io/badge/Runs%20with%20Expo%20Go-000.svg?style=flat-square&logo=EXPO&labelColor=f3f3f3&logoColor=000)](https://expo.dev/client)

[![runs with Expo Go](https://img.shields.io/badge/Runs%20with%20Expo%20Go-4630EB.svg?style=flat-square&logo=EXPO&labelColor=f3f3f3&logoColor=000)](https://expo.dev/client)

```md
[![runs with Expo Go](https://img.shields.io/badge/Runs%20with%20Expo%20Go-000.svg?style=flat-square&logo=EXPO&labelColor=f3f3f3&logoColor=000)](https://expo.dev/client)

[![runs with Expo Go](https://img.shields.io/badge/Runs%20with%20Expo%20Go-4630EB.svg?style=flat-square&logo=EXPO&labelColor=f3f3f3&logoColor=000)](https://expo.dev/client)
```

## 👏 Contributing

If you like Expo and want to help make it better then check out our [contributing guide](/CONTRIBUTING.md)! Check out the [CLI package](https://github.com/expo/expo/tree/main/packages/%40expo/cli) to work on the Expo CLI.

## ❓ FAQ

If you have questions about Expo and want answers, then check out our [Frequently Asked Questions](https://docs.expo.dev/faq/)!

If you still have questions you can ask them on our [Discord and Forums](https://chat.expo.dev) or X [@expo](https://x.com/expo).

## 💙 The Team

Curious about who makes Expo? Here are our [team members](https://expo.dev/about)!

## License

The Expo source code is made available under the [MIT license](LICENSE). Some of the dependencies are licensed differently, with the BSD license, for example.

<img alt="Star the Expo repo on GitHub to support the project" src="https://user-images.githubusercontent.com/9664363/185428788-d762fd5d-97b3-4f59-8db7-f72405be9677.gif" width="50%">
