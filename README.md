# react-native-activity-result

A simple React Native native module for invoking `Activity.startActivityForResult`, `Activity.setResult`, and `Activity.finish` to help with implementing app-to-app communication.

## Getting started

`$ npm install react-native-activity-result --save`

### Mostly automatic installation

`$ react-native link react-native-activity-result`

### Manual installation

#### Android

1. Open up `android/app/src/main/java/[...]/MainActivity.java`
  - Add `import com.microsoft.ActivityResultPackage;` to the imports at the top of the file
  - Add `new ActivityResultPackage()` to the list returned by the `getPackages()` method
2. Append the following lines to `android/settings.gradle`:
  	```
  	include ':react-native-activity-result'
  	project(':react-native-activity-result').projectDir = new File(rootProject.projectDir, 	'../node_modules/react-native-activity-result/android')
  	```
3. Insert the following lines inside the dependencies block in `android/app/build.gradle`:
  	```
      compile project(':react-native-activity-result')
  	```

## Usage
```javascript
import ActivityResult from 'react-native-activity-result';

// Check if an app is installed to handle an intent
const activity = await ActivityResult.resolveActivity('com.example.INTENT');
if (!activity) {
	console.warn('Please install the othe app.');
} else {
	console.log(`Activity will be handled by ${activity.package}`);
}

// Start an activity for a result
let uniqueId = 0;
let args = /* ... */;
const response = await ActivityResult.startActivityForResult(++uniqueId, 'com.example.INTENT', args);
if (response.resultCode !== ActivityResult.OK) {
  throw new Error('Invalid result from activity.');
} else {
  console.log('Got the following response: ' + response.data);
}

// Finish an activity with a result
ActivityResult.finish(ActivityResult.OK, 'com.Example.INTENT', args);
```
