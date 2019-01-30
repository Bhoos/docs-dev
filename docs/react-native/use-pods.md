# Using CocoaPods
Always use cocoapods for dependency management in iOS. The `react-native link` also supports pods once its configured.

## Installing CocoaPods
Use `gem` a dependency manager for `Ruby`. On macs, `gem` is
preinstalled.

> `$ gem install cocoapods`

In some case you might need an elevated prevelages, in which case:

> `$ sudo gem install cocoapods`

## Setup react-native project
From within the `ios` folder of your react-native project. Execute
the following command.

> `$ pod init`

This command will create a new file named `Podfile`.

## Edit Podfile
### 1. Fix some issues
There should be a `target '<projectName>-tvOSTests' do` within
the main `target '<projectName>'` section. This will produce an
error while building, so remove that `tvOSTests` section from the
non `tvOS` target. This is most likely a configuration error in
react-native project setup and might be fixed later.

### 2. Add react-native dependency
The react-native project hasn't yet published it's Pods globally,
so we will have to link to react-native project via node_modules.
Include the following code segment in the `target '<projectName>'`
around the top:
```ruby
# Link to react-native
# React Native is not available on global repo yet
pod 'React', :path => '../node_modules/react-native', :subspecs => [
  'Core',
  'CxxBridge', # Include this for RN >= 0.47
  'DevSupport', # Include this to enable In-App Devmenu if RN >= 0.43
  'RCTText',
  'RCTNetwork',
  'RCTWebSocket', # needed for debugging
  'RCTActionSheet',
  'RCTAnimation',
  'RCTBlob',
  'RCTGeolocation',
  'RCTImage',
  'RCTLinkingIOS',
  'RCTSettings',
  'RCTText',
  'RCTVibration'
  # Add any other subspecs you want to use in your project
]

# Explicitly include Yoga if you are using RN >= 0.42.0
pod 'yoga', :path => '../node_modules/react-native/ReactCommon/yoga'

# Third party deps podspec link
pod 'DoubleConversion', :podspec => '../node_modules/react-native/third-party-podspecs/DoubleConversion.podspec'
pod 'glog', :podspec => '../node_modules/react-native/third-party-podspecs/glog.podspec'
pod 'Folly', :podspec => '../node_modules/react-native/third-party-podspecs/Folly.podspec'
```
This is how the dependency is managed via cocoapods. You might need
to some more subspecs in the `pod 'React'` section later
like for example to use **React ART** you will need to include **ART** subspec.

The `react-native` link command will also update this file to include
`pod specs` as per the library requirement.

### 3. Install Pods
From with the ios folder of your project execute:
> `$ pod install`

This command will take some time for the first time and will install
all the dependencies. All the depedencies are downloaded in the
`Pods` folder. You might want to include that folder in `.gitignore`.
And a lock file `Podfile.lock` is also created, do not add that in
`.gitignore`

### 4. Running app
You should now be able to run the ios app with:
> ` $ react-native run-ios`

## Notes
1. Execute `pod install` after adding any native code or native
   dependency and then build a new ios app.
2. If your are editing your ios project from `XCode` then
open the workspace rather than the project. CocoaPods creates
a `<project>.xcworkspace` which should be used while opening
the project.


