# SOCKS SSH Proxy
An automated way to setup a Proxy connection over SSH for macOS

## Inspiration taken from here
https://www.hostdime.com/kb/hd/security/browsing-the-internet-through-an-ssh-tunnel-on-macos

https://linuxize.com/post/using-the-ssh-config-file/


### Known Limitations and Issues:
Works on macOS only. Tested with macOS 12.6.1 Monterey

The two files "$HOME/.ssh/config" and "$HOME/.ssh/sshpHosts.json" could become out of sync.

### Dependancies:
Needs root privilidges as it will change your active NIC Proxy settings.

Needs the following files: "$HOME/.ssh/config" and "$HOME/.ssh/sshpHosts.json" (Creates them if not found)

Needs jq installed: brew install jq

## How To Use:
```sudo ./ssshp```

Then select from the menu.

1. "Make SSH Tunnel"

Takes user input, establishes SSH tunnel and enable SOCKS Proxy on active NIC.
Presents user with options to, 1. Close connection gracefully, 2. Save settings for later, 3. Exit script and keep connection open.

2. "Select SSH Tunnel"

Let user select from previously saved connections.
