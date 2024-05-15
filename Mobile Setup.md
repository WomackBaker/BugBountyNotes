
### 1. Install Android Studio


### 2. Add ADB to path
- Copy the location of the ADB path (ex: C:\Users\Gebruiker\Downloads\platform-tools_r30.0.5- windows) 
- Press the windows + r key 
- Enter `sysdm.cpl`
- Click the advanced tab
- In the System variable window, find the Path variable and click Edit: ◦ Click New 
- Add the path you copied in step 1
### 3. Set up Frida
- Find out the arch version of the device
`adb shell getprop ro.product.cpu.abi`
- Download the Frida server version from the following URL and extract it
- Push it to phone
`adb push .\frida-server-16.2.1-android-x86_64 /data/local/tmp`
- Add execute permissions
`adb shell chmod 777 /data/local/tmp/frida-server`

### 4. Download Burp certificate and push to device
- Download in DER format from Burp Suite
- Change to PEM format
`openssl x509 -inform DER -in cacert.der -out cacert.pem`
- Push to device
`adb push .\cacert /data/local/tmp`
- Add cert to device in settings

### 5. Proxy to burp
- `emulator -avd "Name"`
- Add proxy in settings

### 6. Find APK and run Frida
- Find APK
`adb shell pm list packages | grep <keyword>`
- Run on Frida server on android
`adb shell /data/local/tmp/frida-server &`
- Run Frida with script on computer
`frida -U -f com.ubermind.rei -l C:\ADB\universalUnpin.js `

### 7. Find APK
- Find APK
`adb shell pm list packages | grep <keyword>`
- Find path
`pm path <app>`
- Download
`adb pull <path>`
- Decompose using JADX
`./jadx --deobf --deobf-min 3 <apk>`
- Open in VS code
-
