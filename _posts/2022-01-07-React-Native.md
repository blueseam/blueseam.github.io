---
layout: post
title:  "React Native Study"
categories: [ blueseam, tutorial ]
image: assets/images/1.png
---

# React Native Notes

1. Install Homebrew

Homebrew installs the stuff you need that Apple(or Linux) didn't

```
blueseam@MacBookPro ~ % /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

blueseam@MacBookPro ~ % brew update
```

2. Install nodejs(mpm)

```
blueseam@MacBookPro ~ % brew install node

blueseam@MacBookPro ~ % node -v
v17.3.0

blueseam@MacBookPro ~ % npm -v
8.3.0
```

3. Install Expo CLI(Developer Tools)

Expo is a set of tools built around React Native

```
blueseam@MacBookPro ~ % npm install -g expo-cli

blueseam@MacBookPro works % expo init HelloWorld
WARNING: expo-cli has not yet been tested against Node.js v17.3.0.
If you encounter any issues, please report them to https://github.com/expo/expo-cli/issues

expo-cli supports following Node.js versions:
* >=12.13.0 <13.0.0 (Maintenance LTS)
* >=14.0.0 <15.0.0 (Active LTS)
* >=15.0.0 <17.0.0 (Current Release)

✔ Choose a template: › tabs (TypeScript)   several example screens and tabs using react-navigation and TypeScript
✔ Downloaded and extracted project files.
📦 Using npm to install packages.
✔ Installed JavaScript dependencies.

✅ Your project is ready!

To run your project, navigate to the directory and run one of the following npm commands.

- cd HelloWorld
- npm start # you can open iOS, Android, or web from here, or run them directly with the commands below.
- npm run android
- npm run ios
- npm run web

blueseam@MacBookPro works % cd HelloWorld

blueseam@MacBookPro HelloWorld % npm start
```

4. Install React Native CLI(Developer Tools)

```
blueseam@MacBookPro ~ % brew tap AdoptOpenJDK/openjsk

blueseam@MacBookPro ~ % brew search jdk

blueseam@MacBookPro ~ % brew install adoptopenjdk

blueseam@MacBookPro ~ % java --version
openjdk 16.0.1 2021-04-20
OpenJDK Runtime Environment AdoptOpenJDK-16.0.1+9 (build 16.0.1+9)
OpenJDK 64-Bit Server VM AdoptOpenJDK-16.0.1+9 (build 16.0.1+9, mixed mode, sharing)


blueseam@MacBookPro ~ % npm install -g react-native-cli

added 85 packages, and audited 86 packages in 3s

40 packages are looking for funding
  run `npm fund` for details

found 0 vulnerabilities


blueseam@MacBookPro ~ % npx react-native --version
react-native-cli: 2.0.1



blueseam@MacBookPro ~ % sudo gem install cocoapods

blueseam@MacBookPro ~ % pod --version
1.11.2

```


5. Android Studio install

```
https://developer.android.com/studio
```

Setting up ADV & SDK Manager
![Android Studio](/assets/images/h3.png)

4. XCode install

https://developer.apple.com/forums/thread/678469

I ran into this same problem and resolved it in the following steps.
) Open Xcode

) go to preferences

) go to locations tab

) select an Xcode version in the select labeled "Command Line Tools"

