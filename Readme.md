react-native-fbads [![npm version](https://badge.fury.io/js/react-native-fbads.svg)](https://badge.fury.io/js/react-native-fbads) <img src="https://david-dm.org/callstack-io/react-native-fbads.svg" /> <img src="https://david-dm.org/callstack-io/react-native-fbads/dev-status.svg?v=1" />
============

[![Facebook Ads](http://i.imgur.com/yH3s6rd.png)](https://developers.facebook.com/products/app-monetization)

**Facebook Audience SDK** integration for React Native, available on iOS and Android.

Features:
- [X] Native Ads
- [ ] Interstitial Ads
- [ ] Banner Ads

## Table of Contents

- [Installation](#installation)
  - [Install Javascript packages](#1-install-javascript-packages)
  - [Configure native projects](#2-configure-native-projects)
    - [iOS](#21-ios)
    - [Android](#22-android)
- [Usage](#usage)
   - [Native Ads](#native-ads)
      - [1. Creating AdsManager](#1-creating-adsmanager)
      - [2. Making ad component](#2-making-ad-component)
      - [3. Rendering an ad](#3-rendering-an-ad)
- [API](#api)
    - [NativeAdsManager](#nativeadsmanager)
      - [disableAutoRefresh](#disableautorefresh)
      - [setMediaCachePolicy](#setmediacachepolicy)
- [Running example](#running-example)
   - [Install dependencies](#1-install-dependencies)
   - [Start packager](#2-start-packager)
   - [Run it on iOS / Android](#3-run-it-on-ios--android)

## Installation

### 1. Install Javascript packages

Install JavaScript packages:

```bash
$ react-native install react-native-fbads
```

### 2. Configure native projects

The react-native-fbads has been automatically linked for you, the next step will be downloading and linking the native Facebook SDK for both platforms.

#### 2.1 iOS

Make sure you have the latest Xcode installed. Open the .xcodeproj in Xcode found in the ios subfolder from your project's root directory. Now, follow all the steps in the [Getting Started Guide for Facebook SDK](https://developers.facebook.com/docs/ios/getting-started) for iOS. Along with FBSDKCoreKit.framework, don't forget to import FBAudienceNetwork.framework.

Next, **follow steps 1 and 3** from the [Getting Started Guide for Facebook Audience](https://developers.facebook.com/docs/audience-network/getting-started). Once you have created the `placement id`, write it down and continue to next section.

#### 2.2. Android

If you are using [`react-native-fbsdk`](https://github.com/facebook/react-native-fbsdk) you can follow their installation instructions. Otherwise, please follow official [Getting Started Guide for Facebook SDK](https://developers.facebook.com/docs/android/getting-started).

## Usage

For detailed usage please check `examples` folder.

### Native Ads

Native Ad is a type of an ad that matches the form and function of your React Native interface.

<img src="https://cloud.githubusercontent.com/assets/2464966/18811079/52c99932-829e-11e6-9a3d-218569d71a6d.png" height="500" />

#### 1. Creating AdsManager

In order to start rendering your custom native ads within your app, you have to construct
a `NativeAdManager` that is responsible for caching and fetching ads as you request them.

```js
import { NativeAdsManager } from 'react-native-fbads';

const adsManager = new NativeAdsManager(placementId, numberOfAdsToRequest);
```

The constructor accepts two parameters:
- `placementId` - which is an unique identifier describing your ad units,
- `numberOfAdsToRequest` - which is a number of ads to request by ads manager at a time

#### 2. Making ad component

After creating `adsManager` instance, next step is to wrap an arbitrary component that you want to
use for rendering your custom advertises with a `withNativeAd` wrapper.

It's a higher order component that passes `nativeAd` via props to a wrapped component allowing
you to actually render an ad!

```js
class AdComponent extends React.Component {
  render() {
    return (
      <View>
        <Text>{nativeAd.description}</Text>
      </View>
    );
  }
}

export default withNativeAd(AdComponent);
```

For full details on the `nativeAd` object, please check flowtype definitions [here](https://github.com/callstack-io/react-native-fbads/blob/master/src/types.js)

#### 3. Rendering an ad

Finally, you can render your wrapped component from previous step and pass it `adsManager`
of your choice.

```js
class MainApp extends React.Component {
  render() {
    return (
      <View>
        <AdComponent adsManager={adsManager} />
      </View>
    );
  }
}
```

## API

### NativeAdsManager

Provides a mechanism to fetch a set of ads and then use them within your application. The native ads manager supports giving out as many ads as needed by cloning over the set of ads it got back from the server which can be useful for feed scenarios. It's a wrapper for [`FBNativeAdsManager`](https://developers.facebook.com/docs/reference/ios/current/class/FBNativeAdsManager/)

#### disableAutoRefresh

By default the native ads manager will refresh its ads periodically. This does not mean that any ads which are shown in the application's UI will be refreshed but simply that requesting next native ads to render may return new ads at different times. This method disables that functionality.

```js
adsManager.disableAutoRefresh();
```

#### setMediaCachePolicy

Sets the native ads manager caching policy. This controls which media from the native ads are cached before being displayed. The default is to not block on caching.

```js
adsManager.setMediaCachePolicy('none' | 'icon' | 'image' | 'all');
```

**Note:** This method is a noop on Android

## Running example

### 1. Install dependencies

```bash
$ npm install
```

### 2. Start packager

Because of the way example project is set up (custom packager arguments), you'll
have to start it explicitly before any other command

```bash
$ cd ./example && npm start
```

### 3. Run it on iOS / Android

```bash
$ cd ./example && npm run ios
$ cd ./examples && npm run android
```
