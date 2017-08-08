# FingerprintDialog [ ![Download](https://api.bintray.com/packages/omaflak/maven/fingerprintdialog/images/download.svg) ](https://bintray.com/omaflak/maven/fingerprintdialog/_latestVersion)

**FingerprintDialog** is an Android library that simplifies the process of fingerprint authentications.

# Usecase n°1 : Without CryptoObject

You only want to check if the user's fingerprint is enrolled in the phone.

```java
FingerprintDialog.initialize(this)
    .title(R.string.title)
    .message(R.string.message)
    .callback(new FingerprintCallback(...))
    .show();
```

[EXAMPLE](https://github.com/omaflak/FingerprintDialog-Library/blob/master/app/src/main/java/me/aflak/fingerprintdialoglibrary/FingerprintExample.java)
        
# Usecase n°2 : With CryptoObject

Check if the user's fingerprint is enrolled in the phone and detect if a new fingerprint was added since last time authentication was used.

```java
FingerprintDialog.initialize(this)
    .title(R.string.title)
    .message(R.string.message)
    .callback(new FingerprintSecureCallback(...), "KeyName")
    .show();
```
        
[EXAMPLE](https://github.com/omaflak/FingerprintDialog-Library/blob/master/app/src/main/java/me/aflak/fingerprintdialoglibrary/FingerprintSecureExample1.java)

# Usecase n°3 : Secure a CryptoObject via authentication

The CryptoObject will be valid only if the user has authenticated via fingerprint. It can then be used to perform cryptographic operations (such as encrypting / decrypting data)

```java
// You can use CipherHelper and SignatureHelper to generate a CryptoObject, or you can create your own one
SignatureHelper helper = new SignatureHelper("KeyName");
FingerprintManager.CryptoObject cryptoObject = helper.getSigningCryptoObject();
if(cryptoObject==null){
    // /!\ A new fingerprint was added /!\
    //
    // Prompt a password to verify identity, then :
    // if (password correct) {
    //      helper.generateNewKey();
    // }
    //
    // OR
    //
    // Use PasswordDialog to simplify the process

    PasswordDialog.initialize(this, helper)
            .title(R.string.password_title)
            .message(R.string.password_message)
            .callback(new PasswordCallback(...))
            .passwordType(PasswordDialog.PASSWORD_TYPE_TEXT)
            .show();
}
else{
    if(FingerprintDialog.isAvailable(this)) {
        FingerprintDialog.initialize(this)
                .title(R.string.fingerprint_title)
                .message(R.string.fingerprint_message)
                .callback(new FingerprintCallback(...))
                .cryptoObject(cryptoObject)
                .show();
    }   
}
```

[EXAMPLE](https://github.com/omaflak/FingerprintDialog-Library/blob/master/app/src/main/java/me/aflak/fingerprintdialoglibrary/FingerprintSecureExample2.java)

# Gradle

```gradle
implementation 'me.aflak.libraries:fingerprintdialog:2.4.2'
```
    
# Rendering

![alt text](https://github.com/omaflak/FingerprintDialog/blob/master/GIF/demo.gif?raw=true)
