---
title: Flutter Tutorial - Development Environment Setup
description: For iOS and Android
date: 2021-10-12

categories:
  - frontend

tags: 
  - flutter
  - dart
  - mobile

# resources:
#   - name: featured-image
#     src: featured-image.png
---


I have dabbled with [Dart](https://dart.dev/) for some toy backend experiment when it first came out years ago, even before ES6 era. I really liked how it was typed and offer very nice IDE integrations, as well as its Stream support. 

With [Flutter](https://flutter.dev/) overtaking [React Native](https://reactnative.dev/) on Google Trends, I decided to dive head first and try to make a cross platform toy app that can run on iOS, Android and Web.

{{<google_trends keywords="/g/11f03_rzbg, /g/11h03gfxy9" range="5-y">}}

## Overview

This series of articles will walk you through my journey from setting up the development environment to various aspects of building a non-trivial app. Each  step of the way, I will document my findings in small, bite-sized articles for easy consumptions.

In this article, I will cover setting up the development environment for both iOS and Android development. I'm assuming you are using the latest MacOS.

This series of articles is broken down as the following:

* Development Environment Setup ( this article )
* Customizing Splash Screen and Logo
* Login Page with Firebase Auth
* Profile Page

## Install XCode

Install [XCode from the App Store](https://apps.apple.com/us/app/xcode/id497799835?mt=12) if you haven't done so.

Next, you will need to install the XCode CommandLine Tools by running the following command in your terminal:

```bash
xcode-select --install
```

## Install HomeBrew

[HomeBrew](https://brew.sh/) is a package manager for MacOS. The installation is very simple, please go to https://brew.sh/ and follow the instructions there.

Read the notes after installation and make sure your `.zshrc` or `.bash_profile` is properly configured. It's also helpful to run `brew doctor` to verify brew is installed correctly.

## Install Flutter

The easiest way to install Flutter on macOS is probably by using HomeBrew.

```bash
brew install --cask flutter # install flutter
```

## Install Android Studio

In order to compile Flutter App for Android, we need to install Android Studio. Again, this can be done with HomeBrew

```bash
brew install --cask android-studio
```

## Install Java

Android Studio needs Java 8 to compile. We are going to use OpenJDK. [Temurin](https://adoptium.net/) is the successor to [AdoptOpenJDK](https://github.com/AdoptOpenJDK/homebrew-openjdk) that provides prebuilt OpenJDK binaries, and is the recommended way to install OpenJDK by HomeBrew.

```bash
brew install --cask temurin8
```

## Install Android SDK, CommandLineTools

Open Android Studio in the Apps folder and you will encounter the initial setup process. Select the standard setup, and complete the initial setup. By default this will download the Android SDK and Android Emulators.

Once the initial setup is done. You need to open the `Preferences` menu, and navigate to: `Appearance And Behavior` -> `System Settings` -> `Android SDK`.

* In `SDK Platforms` tab, make sure the latest Android version ( 12 ) is checked.
* In `SDK Tools` tab, make sure you have the the following checked:
  * Android SDK Build-Tools 31
  * Android SDK Command-line Tools (latest)
  * Android Simulator
  * Android SDK Platform-Tools

Finally, click Ok button and wait for installation to complete.

## Sanity Check

Now it's time to see if we have setup everything correctly. In a terminal run:

```bash
flutter doctor
```

You should have everything green.

## Create A New App

Now we have flutter setup, we can start by generating a skeleton app for our project. 

```bash
flutter create software_unicorns
```

This will create a new project called `software_unicorns` . Change the project name to your liking.

## Run The New App on iOS Simulator

First open the iOS Simulator by either type in `Simulator` in Spotlight Search ( Cmd + Space ) or running `open -a Simulator` in terminal.

Now, in the project directory, run the command below.

```bash
flutter run # runs the new app
```

Wait for it to compile, it might take a while. After compilation is done, your iOS simulator should have the skeleton app running. 

## Run The New App on Android Simulator

First open Android Studio, then open the AVD Manager (Android Virtual Device Manager). You should have a default virtual device setup during installation.

Now, in the project directory, run the command below.

```bash
flutter run # runs the new app
```

Similarly as with iOS, it takes a long time to compile on first run. But once it's done, your app should show up on the simulated Android device.

## What's next?

With the development environment setup, we can finally start building our app. But before that, let's customize our Splash Screen and Logo for both iOS and Android, and move onto Creating a Sign In Screen.
