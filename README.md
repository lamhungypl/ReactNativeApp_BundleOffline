# ReactNativeApp_BundleOffline
build app without bundler js server

# create assets folder in the current project
 `$ mkdir android/app/src/main/assets`

# create bundle script
`$ react-native bundle --platform android --dev false --entry-file index.js --bundle-output android/app/src/main/assets/index.android.bundle --assets-dest android/app/src/main/res/`

# execute command to run android to create debug apk
`$ react-native run-android`

# Or change to android folder
`$ cd android`

# build debug apk
`$ ./gradlew assembleDebug`

# build release/unsigned apk
`$ ./gradlew assembleRelease`



**#React Native Android Duplicate file error when generating apk**

Custom node_modules/react-native/react.gradle to solve the Duplicate file error perfectly. Add following code into currentBundleTask's creation block (after doFirst block)

`doLast {
    def moveFunc = { resSuffix ->
   
        File originalDir = file("${resourcesDir}/drawable-${resSuffix}");
   
        if (originalDir.exists()) {
        
            File destDir = file("$buildDir/../src/main/res/drawable-${resSuffix}");
            
            ant.move(file: originalDir, tofile: destDir);
       }
    }    
    moveFunc.curry("ldpi").call()    
    moveFunc.curry("mdpi").call()    
    moveFunc.curry("hdpi").call()    
    moveFunc.curry("xhdpi").call()    
    moveFunc.curry("xxhdpi").call()    
    moveFunc.curry("xxxhdpi").call()
}`

You can create script to do it automatically.

reate `android-react-gradle-fix` file
Create script `android-release-gradle-fix.js` file
Update `package.json` file:

`"scripts": { "postinstall": "node ./android-release-gradle-fix.js" },`

That's it! Run npm install to get awesome.

Note: If you run `npm install` on ci like jenkins, you may get `error: postinstall: cannot run in wd %s %s (wd=%s) node` => just use `npm install --unsafe-perm` instead
