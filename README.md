# Telemetry Sourcerer

<p align="center">
  <img src="https://www.publicdomainpictures.net/pictures/180000/nahled/wizard-with-wand.jpg" height="250" />
</p>

## Introduction

Telemetry Sourcerer can enumerate and disable common sources of telemetry used by AV/EDR on Windows.

Red teamers and security enthusiasts can use this tool in a lab environment to:

- Identify collection-based blind spots in the products they're up against.
- Determine which sources of telemetry generate particular types of events.
- Validate whether using the tool's tampering capabilities can lead to detection.

For details on building a private lab, consider reading my post on [Diverting EDR Telemetry to Private Infrastructure](http://jackson-t.ca/edr-reversing-evading-03.html).

## Features

- Enumerates various kernel-mode callbacks with the ability to suppress them.
- Detects inline user-mode hooks within the process, with the ability to unhook them.
- Lists ETW sessions and providers while highlighting potentially relevant ones to disable.

## Screenshots

<p align="center">
  <img src="https://i.imgur.com/W6EODwb.png" />
  <br><br>
  <img src="https://i.imgur.com/WzjTNnP.png" />
  <br><br>
  <img src="https://i.imgur.com/dqwidfM.png" />
</p>

## Usage Instructions

1. Download the [latest release](#).
1. Extract files.
1. Launch the executable.

### Kernel-mode Callbacks

To view kernel-mode callbacks, consider enabling test signing mode or signing the driver with a valid certificate:

#### Test Signing Mode

1. Disable BitLocker and Secure Boot.
1. Open an elevated Command Prompt window.
1. Enter `bcdedit.exe -set TESTSIGNING ON`.
1. Reboot system.
1. Launch Telemetry Sourcerer with elevated privileges.

#### Sign Driver

1. Get [SignTool](https://docs.microsoft.com/en-us/windows/win32/seccrypto/signtool) from the Windows SDK and an appropriate [cross-certificate](https://docs.microsoft.com/en-us/windows-hardware/drivers/install/cross-certificates-for-kernel-mode-code-signing) from Microsoft Docs.
1. `signtool sign /a /ac "cross-cert.cer" /f "cert.pfx" /p "password" TelemetrySourcererDriver.sys` 
1. Launch Telemetry Sourcerer with elevated privileges.

## Caveats and Limitations

- This tool is meant for research purposes only and is not OPSEC-safe for production use.
- Compiled with Visual Studio 2019 using the Windows 10 SDK (10.0.19041.0) and WDK (2004).
- Currently does not look for IAT/EAT user-mode hooks, or kernel-mode hooks.
- The driver has not been thoroughly tested for abuse cases.
- Tested on Windows 7 and 10 only.

## Credits

This tool was developed by [@Jackson_T](https://twitter.com/Jackson_T) but builds upon the work of others:

- [@gentilkiwi](https://twitter.com/gentilkiwi) and [@fdiskyou](https://twitter.com/fdiskyou) for driver code that enumerates callback functions.
- [@0x00dtm](https://twitter.com/0x00dtm) for the inline user-mode hook comparison logic.

## Related Articles and Projects

- [@matterpreter](https://twitter.com/matterpreter): [Mimidrv In Depth: Exploring Mimikatz’s Kernel Driver](https://posts.specterops.io/mimidrv-in-depth-4d273d19e148)
- [@fdiskyou](https://twitter.com/fdiskyou): [Windows Kernel Ps Callbacks Experiments](http://deniable.org/windows/windows-callbacks)
- [@matteomalvica](https://twitter.com/matteomalvica): [Silencing the EDR. How to disable process, threads and image-loading detection callbacks.](https://www.matteomalvica.com/blog/2020/07/15/silencing-the-edr/)
- [@0x00dtm](https://twitter.com/0x00dtm): [Defeating Userland Hooks (ft. Bitdefender)](https://0x00sec.org/t/defeating-userland-hooks-ft-bitdefender/12496) ([Code](https://github.com/NtRaiseHardError/Antimalware-Research/tree/master/Generic/Userland%20Hooking/AntiHook))
- [@palantir](https://medium.com/palantir): [Tampering with Windows Event Tracing: Background, Offense, and Defense](https://medium.com/palantir/tampering-with-windows-event-tracing-background-offense-and-defense-4be7ac62ac63)

## Licence

This project is licensed under the Apache License 2.0.
