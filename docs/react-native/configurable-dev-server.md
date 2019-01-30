# Configurable Dev Server
While working with react-native projects on android, it's easy to
switch one of the co-workers development server code by changing
the IP address on `Dev Settings`. This is not a case with iOS where
the development server IP address is hard coded into the app. This
problem could be addressed by using `Settings.bundle` which allows
us to provide certain configuration parameters within the
**Settings** app.

## Settings.bundle
### 1. Open iOS project in XCode

### 2. Add Settings.bundle
Use `Add New File...` to add `Settings.bundle` from resource section

### 3. Setup input field
Edit the `Root.plist` file within `Settings.bundle` and add/edit
`Text Field` and give it a proper `Identifier` and `Title`.
For our example sake, it would be `server`. Provide a `Default Value`
of `localhost:8081`.

### 4. Update AppDelegate
Open `AppDelegate.m` file and edit the debug section code which sets jsCodeLocation to look like:
```ObjectiveC
#ifdef DEBUG
  NSUserDefaults *defaults = [NSUserDefaults standardUserDefaults];
  NSString *server = [defaults stringForKey:@"server"];
  if (server == NULL) {
    server = @"localhost:8081";
  }
  NSString *url = [NSString stringWithFormat:@"http://%@/index.bundle?platform=ios&dev=true&minify=false", server];
  jsCodeLocation = [NSURL URLWithString:url];
#else
  jsCodeLocation = [[NSBundle mainBundle] URLForResource:@"main" withExtension:@"jsbundle"];
#endif
```

## Avoid in Release
We would only want this configuration to appear in development mode
only. In the release mode, we would want to exclude this entire
section. This should be done in the `Build Phases`.

We want to include the `Settings.bundle` conditionally. So remove
`Settings.bundle` from `Copy Bundle Resources`, then add
`New Run Script Phase` and use the following shell script:
```sh
if [ "${CONFIGURATION}" == "Debug" ]; then
  cp -r "${PROJECT_DIR}/Settings.bundle" "${BUILT_PRODUCTS_DIR}/${PRODUCT_NAME}.app/Settings.bundle"
fi
```
