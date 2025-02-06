
# Pentest Android

## Décompilation d'un APK

```bash
apktool d app-release.apk
```

## Repackaging de l'APK (supprimer d'abord les fichiers cert et manifest)

```bash
zip -r app-release.patch.apk *
```

## Génération d'une Clé de Signature

```bash
keytool -genkey -v -keystore key.jks -keyalg RSA -keysize 2048 -validity 10000 -alias hackthebox
```

## Signature de l'APK Modifié

```bash
jarsigner -verbose -sigalg SHA1withRSA -digestalg SHA1 -keystore key.jks app-release.patch.apk hackthebox
```