# dbangel

## First Build

```
cordova create cordova com.dbangel.beta "DB Angel"
rm -Rf ./cordova/www/*
cp -f ./build/config.xml ./cordova/config.xml
cd cordova && cordova platform add android && cd -
cordova plugin install
grunt dist
cd cordova && cordova build --release && cd -
cp cordova/platforms/android/app/build/outputs/apk/release/app-release-unsigned.apk build/tmp/app-release-unsigned.apk
jarsigner -verbose -sigalg SHA1withRSA -digestalg SHA1 -keystore build/dbangel.keystore build/tmp/app-release-unsigned.apk dbangel
# dbangel
rm -f dbangel.apk
zipalign -v 4 build/tmp/app-release-unsigned.apk dbangel.apk
```

## Rebuild

```
cordova build --release
cp platforms/android/app/build/outputs/apk/release/app-release-unsigned.apk tmp/app-release-unsigned.apk
jarsigner -verbose -sigalg SHA1withRSA -digestalg SHA1 -keystore dbangel.keystore tmp/app-release-unsigned.apk dbangel
# dbangel
rm -f tmp/dbangel.apk
zipalign -v 4 tmp/app-release-unsigned.apk tmp/dbangel.apk
```

## Creating Keystore

Create only if required, or if file `build/dbangel.keystore` not there.

```
keytool -genkey -v -keystore build/dbangel.keystore -alias dbangel -keyalg RSA -keysize 2048 -validity 10000
# dbangel
```