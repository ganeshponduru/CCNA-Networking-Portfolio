# Configuring SSH on Cisco IOS — Study Notes

SSH (Secure Shell) provides encrypted remote management. The default SSH transport port is TCP 22. On Cisco IOS, exact commands and supported algorithms can vary by platform and software version.

## Configuration sequence

The following is a study template for a lab router. Replace placeholders with lab-specific values. Do not use real production credentials in a public repository.

```text
enable
configure terminal
hostname R1
ip domain-name lab.example
username netadmin secret <choose-a-strong-lab-password>
crypto key generate rsa modulus 2048
ip ssh version 2
line vty 0 4
 login local
 transport input ssh
end
```

Notes:

- Set the hostname and domain name before generating RSA keys on platforms that require them.
- Use a platform-supported, security-approved RSA key size. The 1024-bit example in the handout is outdated for a new configuration; 2048 bits is a common minimum where supported.
- `username ... secret` is preferable to placing a plain-text password in a configuration example.
- `line vty 0 4` selects VTY lines 0 through 4 on platforms that use that range. Other models may provide a different range.
- `login local` uses the locally configured username database for authentication.
- `transport input ssh` permits incoming SSH on the selected VTY lines and excludes Telnet there.
- `transport output ssh` is optional and controls outgoing remote protocols from those lines; it is not required simply to accept inbound SSH.

Configure a reachable interface address as appropriate for the topology, and ensure the interface is enabled. For example, in an isolated two-router lab only:

```text
interface GigabitEthernet1/0
 ip address 192.168.1.1 255.255.255.0
 no shutdown
```

Use a different address on the peer router, in the same subnet for this simple point-to-point Ethernet example.

## Verification commands

```text
show ip interface brief
show ip ssh
show running-config
show users
ping <peer-ip>
```

Some commands or output fields vary by IOS release. Avoid posting full running configurations publicly without reviewing them for secrets, management addresses, usernames, and other sensitive details.

## Initiating an SSH session

From a Cisco IOS device, the common syntax is:

```text
ssh -l <username> <remote-ip>
```

From a terminal on a computer, a common OpenSSH syntax is:

```text
ssh <username>@<remote-ip>
```

The username syntax differs between the Cisco CLI form and a typical computer terminal.

## Troubleshooting checklist

1. Check the local and remote interface status and IP addressing.
2. Verify IP reachability; remember that ping can be blocked even when another service is reachable.
3. Confirm the remote device has SSH enabled and has the required host keys.
4. Check VTY configuration, local user configuration, and whether SSH is permitted by any ACL or firewall.
5. Verify that the client is connecting to the correct address and TCP port 22.
6. Check client/server SSH version and algorithm compatibility if TCP connects but SSH negotiation fails.

## Practice status

This is a written configuration template based on the class transcript and PDF, not evidence that I personally configured a router. I will mark the lab complete only after I run it and record the actual outputs.
