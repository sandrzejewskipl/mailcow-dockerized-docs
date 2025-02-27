So far three methods for _Two-Factor Authentication_ are implemented: U2F, Yubi OTP, and TOTP

-   For U2F to work, you need an encrypted connection to the server (HTTPS) as well as a FIDO security key.
-   Both U2F and Yubi OTP work well with the fantastic [Yubikey](https://www.yubico.com).
-   While Yubi OTP needs an active internet connection and an API ID + key, U2F will work with any FIDO U2F USB key out of the box, but can only be used when mailcow is accessed over HTTPS.
-   U2F and Yubi OTP support multiple keys per user.
-   As the third TFA method mailcow uses TOTP: time-based one-time passwords. Those passwords can be generated with apps like "Google Authenticator" after initially scanning a QR code or entering the given secret manually.

As administrator you are able to temporary disable a domain administrators TFA login until they successfully logged in.

The key used to login will be displayed in green, while other keys remain grey.

Information on how to remove 2FA can be found [here](https://mailcow.github.io/mailcow-dockerized-docs/debug-reset_pw/#remove-two-factor-authentication).

## Yubi OTP

The Yubi API ID and Key will be checked against the Yubico Cloud API. When setting up TFA you will be asked for your personal API account for this key.
The API ID, API key and the first 12 characters (your YubiKeys ID in modhex) are stored in the MySQL table as secret.

### Example setup

First of all, the YubiKey must be configured for use as an OTP Generator. To do this, download the `YubiKey Manager` from the Yubico website: [here](https://www.yubico.com/support/download/)

In the following you configure the YubiKey for OTP.
Via the menu item `Applications` -> `OTP` and a click on the `Configure` button. In the following menu select `Credential Type` -> `Yubico OTP` and click on `Next`.

Set a checkmark in the `Use serial` checkbox, generate a `Private ID` and a `Secret key` via the buttons. 
So that the YubiKey can be validated later, the checkmark in the `Upload` checkbox must also be set and then click on `Finish`.

Now a new browser window will open in which you have to enter an OTP of your YubiKey at the bottom of the form (click on the field and then tap on your YubiKey). Confirm the captcha and upload the information to the Yubico server by clicking 'Upload'. The processing of the data will take a moment.

After the generation was successful, you will be shown a `Client ID` and a `Secret key`, make a note of this information in a safe place.

Now you can select `Yubico OTP authentication` from the dropdown menu in the mailcow UI on the start page under `Access` -> `Two-factor authentication`. 
In the dialog that opened now you can enter a name for this YubiKey and insert the `Client ID` you noted before as well as the `Secret key` into the fields provided.
Finally, enter your current account password and, after selecting the `Touch Yubikey` field, touch your YubiKey button.

Congratulations! You can now log in to the mailcow UI using your YubiKey!

## U2F

To use U2F, the browser must support this standard.

The following desktop browsers support this authentication type:

-   Edge (>=79)
-   Firefox (>=47, enabled by default since version 67)
-   Chrome (>=41)
-   Safari (>=13)
-   Opera (40, >=42, not 41)

The following mobile browsers support this authentication type:

-   Safari on iOS (>=13.3)
-   Firefox on Android (>=68)

Sources: [caniuse.com](https://caniuse.com/u2f), [blog.mozilla.org](https://blog.mozilla.org/security/2019/08/05/web-authentication-in-firefox-for-android/)

U2F works without an internet connection.

### TOTP

The best known TFA method mostly used with a smartphone.
