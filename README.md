# Sample App
Sample app with Identity Vault 5.

## Issue - Unhandled error on getValue
On an Android device
`npm install`
`npm run build`
`ionic cap run android` (on a real android device)

In the app
- Enter some data for session data and press `set session data`
- Press `lock vault`
- Press `restore session data` 
- Passcode will be requested 
- The call to `getValue` will fail with `{"code":0,"message":"Unhandled Error: "}`

If run in the Android Studio you can see this error:
javax.crypto.IllegalBlockSizeException
2022-01-25 12:39:39.220 18086-18086/io.ionic.gettingstartedivangular W/System.err:     at android.security.keystore.AndroidKeyStoreCipherSpiBase.engineDoFinal(AndroidKeyStoreCipherSpiBase.java:519)
2022-01-25 12:39:39.221 18086-18086/io.ionic.gettingstartedivangular W/System.err:     at javax.crypto.Cipher.doFinal(Cipher.java:2055)
2022-01-25 12:39:39.221 18086-18086/io.ionic.gettingstartedivangular W/System.err:     at com.ionicframework.IdentityVault.DeviceSecurityStrongVault.setMasterKeyCipher(DeviceSecurityStrongVault.java:109)
2022-01-25 12:39:39.221 18086-18086/io.ionic.gettingstartedivangular W/System.err:     at com.ionicframework.IdentityVault.BiometricPromptActivity.onPromptSuccess(BiometricPromptActivity.java:238)
2022-01-25 12:39:39.221 18086-18086/io.ionic.gettingstartedivangular W/System.err:     at com.ionicframework.IdentityVault.BiometricPromptActivity.access$100(BiometricPromptActivity.java:21)
2022-01-25 12:39:39.222 18086-18086/io.ionic.gettingstartedivangular W/System.err:     at com.ionicframework.IdentityVault.BiometricPromptActivity$1.onAuthenticationSucceeded(BiometricPromptActivity.java:140)
2022-01-25 12:39:39.222 18086-18086/io.ionic.gettingstartedivangular W/System.err:     at androidx.biometric.BiometricFragment$9.run(BiometricFragment.java:907)
2022-01-25 12:39:39.222 18086-18086/io.ionic.gettingstartedivangular W/System.err:     at android.os.Handler.handleCallback(Handler.java:938)
2022-01-25 12:39:39.222 18086-18086/io.ionic.gettingstartedivangular W/System.err:     at android.os.Handler.dispatchMessage(Handler.java:99)
2022-01-25 12:39:39.222 18086-18086/io.ionic.gettingstartedivangular W/System.err:     at android.os.Looper.loop(Looper.java:223)
2022-01-25 12:39:39.222 18086-18086/io.ionic.gettingstartedivangular W/System.err:     at android.app.ActivityThread.main(ActivityThread.java:7656)
2022-01-25 12:39:39.222 18086-18086/io.ionic.gettingstartedivangular W/System.err:     at java.lang.reflect.Method.invoke(Native Method)
2022-01-25 12:39:39.223 18086-18086/io.ionic.gettingstartedivangular W/System.err:     at com.android.internal.os.RuntimeInit$MethodAndArgsCaller.run(RuntimeInit.java:592)
2022-01-25 12:39:39.223 18086-18086/io.ionic.gettingstartedivangular W/System.err:     at com.android.internal.os.ZygoteInit.main(ZygoteInit.java:947)
2022-01-25 12:39:39.223 18086-18086/io.ionic.gettingstartedivangular W/System.err: Caused by: android.security.KeyStoreException: Key user not authenticated
2022-01-25 12:39:39.224 18086-18086/io.ionic.gettingstartedivangular W/System.err:     at android.security.KeyStore.getKeyStoreException(KeyStore.java:1301)
2022-01-25 12:39:39.224 18086-18086/io.ionic.gettingstartedivangular W/System.err:     at android.security.keystore.KeyStoreCryptoOperationChunkedStreamer.update(KeyStoreCryptoOperationChunkedStreamer.java:140)
2022-01-25 12:39:39.224 18086-18086/io.ionic.gettingstartedivangular W/System.err:     at android.security.keystore.KeyStoreCryptoOperationChunkedStreamer.doFinal(KeyStoreCryptoOperationChunkedStreamer.java:169)
2022-01-25 12:39:39.224 18086-18086/io.ionic.gettingstartedivangular W/System.err:     at android.security.keystore.AndroidKeyStoreCipherSpiBase.engineDoFinal(AndroidKeyStoreCipherSpiBase.java:506)
2022-01-25 12:39:39.224 18086-18086/io.ionic.gettingstartedivangular W/System.err: 	... 13 more

## Timing Test
- Open `node_modules/@ionic-enterprise/identity-vault/src/android/com/ionicframework/IdentityVault/AsymmetricCrypto.java`
- Insert on line 45 `.setKeySize(4096)` 

Here's an example of the change:
```java
                .setUserAuthenticationRequired(true)                
                .setKeySize(4096)
                .setEncryptionPaddings(KeyProperties.ENCRYPTION_PADDING_RSA_PKCS1);
```

Run the application and change the lock type. It will show the amount of time it takes to generate a key of that particular size.