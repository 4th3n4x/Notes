

```
# unpack
apktool d app-release.apk
# repack (supprimer d'abord cert et manifest)
zip -r app-release.patch.apk *
# generer une clef
keytool -genkey -v -keystore key.jks -keyalg RSA -keysize
2048 -validity 10000 -alias hackthebox
arsigner -verbose -sigalg SHA1withRSA -digestalg SHA1 -
keystore key.jks app-release.patch.apk hackthebox
```