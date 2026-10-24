# Console Access Basics

I'm learning the difference between local console access and remote management access.

## Console access

Console access is a local management method commonly used for initial setup, recovery, or troubleshooting when network management is not yet available.

The supplied hardware notes describe a basic setup using a computer, a console cable, and terminal software such as PuTTY. Depending on the device and computer, the cable may connect directly or through a USB-to-serial adapter.

## Basic workflow

1. Confirm the exact device's console-port type and the required cable.
2. Connect the cable to the device console port and computer/adapter.
3. Check the computer's Device Manager for the assigned COM port when using serial/USB-to-serial access.
4. Open terminal software and select the appropriate serial connection.
5. Use the serial settings specified by the device documentation. **9600 baud is common on many Cisco console setups, but it is not universal.** Other serial parameters may also need to match.
6. Open the session and press Enter if needed to display the prompt.
7. Follow the organization's access and authorization process.

## First login and configuration

The handout shows an example Cisco workflow using commands such as:

```text
enable
configure terminal
hostname MyRouter
```

These are examples, not a complete secure configuration template. On a real device, I should confirm the initial state and follow the approved configuration procedure. I must not set or publish real passwords in a public portfolio.

## Console vs remote access

- **Console:** local physical management path; useful before the device has working IP connectivity.
- **SSH:** encrypted remote management over an IP network when configured and reachable.
- **Telnet:** remote terminal access without SSH's encryption and generally unsuitable for secure management.

Console access does not itself prove the data-plane links are working. Remote SSH access requires suitable network connectivity and configuration.

## Lab status

These are notes about the process. I have not marked a physical console-login lab as completed unless I separately record the device, connection, settings, commands, and observed result from my own practice.
