# SOCKS SSH Proxy aka ssshp

An automated way to setup a Proxy connection over SSH for macOS

## Known Limitations and Issues

### OS

Tested exclusivly on macOS. Initially created With macOS 12.6.1 Monterey.

Tested on macOS 15.1 beta - 2024-10-19

### Config file v Custom ssshpHosts.json file

The two files "$HOME/.ssh/config" and "$HOME/.ssh/ssshpHosts.json" could become out of sync.

The config file belongs to the users ssh install, ssshpHosts.json is built with this script.

### Dependancies

Needs sudo to run correctly as it needs to change your active NIC Proxy settings.

Needs the following files: "$HOME/.ssh/config" and "$HOME/.ssh/sshpHosts.json" (Creates them if not found)

Needs jq installed: <https://stedolan.github.io/jq/download/>

## How To Use

>Tip: Run from custom 'bin' folder added to your $PATH

```sh
sudo ./ssshp
```

Then select from the menu.

1. "Make SSH Tunnel"

    Takes user input, establishes SSH tunnel and enable SOCKS Proxy on active NIC.
    Presents user with options to, 1. Close connection gracefully, 2. Save settings for later, 3. Exit script and keep connection open.

2. "Select SSH Tunnel"

    Let user select from previously saved connections.

>See file: `ssshp-example.md` For example useage.

## But what is this script doing exactly?

The script is more or less a convoluted way to run a simple 1 liner

```sh
ssh -f -N -M -S /tmp/sshtunnel -D 1080 $sshTarget
```

And then modify your Network Preferences for primary NIC. Enabling SOCKS Proxy for local host over port 1080

The following excerpt from script is where the actual connection is made. Everything else is just for building the menus, handling user input, saving connection details for later.

>Does not save passwords. Does not send telemetry data anywhere.

```sh
    ################################################################################################
    # Configuring SSH Tunnel                                                                       #
    ################################################################################################
    ## At this point the script will attempt to setup the SSH tunnel to target host.
    ## If successful, will use the following flags:
    ## -f = Requests ssh to go to background just before command execution.  This is useful if ssh is going to ask for passwords or passphrases
    ## -N = Do not execute a remote command.  This is useful for just forwarding ports.
    ## -M = Places the ssh client into "master" mode for connection sharing.
    ## -S = ctl_path - Specifies the location of a control socket for connection sharing, or the string "none" to disable connection sharing
    ##      Refer to the description of ControlPath and ControlMaster in ssh_config(5) for details.
    ##      e.g. /tmp/sshtunnel
    ## -D = [bind_address:]port - Specifies a local "dynamic" application-level port forwarding. 
    ##      This works by allocating a socket to listen to port on the local side
    ##      1080 is typical SOCKS Port.
    echo -e "${GREEN}\n>>> Attempting connection\n\nUsing command: ssh -f -N -M -S /tmp/sshtunnel -D 1080 $sshTarget ${NOCOLOR}\n"
    ssh -f -N -M -S /tmp/sshtunnel -D 1080 $sshTarget ##shellcheck gives an 'error', but we want word splitting here.
    echo -e "${GREEN}\n>>> Checking for Established SSH Tunnels with: lsof -i tcp | grep ^ssh ${NOCOLOR}"
    lsof -i tcp | grep ^ssh
    ################################################################################################
    # Configuring proxy settings under the 'Automatic' Location and only on the currently used NIC #
    ################################################################################################
    ## This will overwrite any pre-existing socks proxy settings for the targeted NIC
    networksetup -setsocksfirewallproxy "$whichNic" 127.0.0.1 1080
    echo -e "${GREEN}\nSOCKS settings for active NIC: $whichNic ${NOCOLOR}"
    networksetup -getsocksfirewallproxy "$whichNic"

    echo -e "${GREEN}\nDetected WAN IP addresses${NOCOLOR}"
    echo -e "\nSOCKS Connection for Browser:"
    ## Fix for curl leaving a % 'New line' character in output
    ## https://unix.stackexchange.com/questions/167582/why-zsh-ends-a-line-with-a-highlighted-percent-symbol
    curl -sS -x socks5h://localhost:1080 http://whatismyip.akamai.com/ ; echo
    echo -e "*TIP: if using curl, type: curl -sS -x socks5h://localhost:1080 http://whatismyip.akamai.com/ ; echo"
    echo -e "\nYour Computers Primary WAN IP:"
    dig @resolver4.opendns.com myip.opendns.com +short
    echo -e "${GREEN}\n>>> Keep terminal open until you are finished with SOCKS Proxy\n\nWhen ready select from options below${NOCOLOR}"
```
