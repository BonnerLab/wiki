# SSH (`man ssh`)

The [Secure Shell (SSH) protocol](http://www.openssh.com/specs.html) ([Wikipedia](https://en.wikipedia.org/wiki/Secure_Shell)) allows you to connect from computer `A` to computer `B` securely. This document is a quick guide to some SSH options that make life easy.

## Basics

If you're on computer `A` and computer `B` is on the same network, you can SSH to it using:

`ssh <username-on-machine-B>@<hostname-or-IP-address-of-machine-B>`

This will probably generate a password prompt, which can be very inconvenient if you're constantly SSHing into machines.

## Key-based authentication (`man ssh-keygen`)

Do you find yourself typing a password each time you SSH into a machine? Fear no more: key-based authentication is here to save you the trouble!

- Generate an SSH key pair on your local machine using `ssh-keygen -t ed25519` (`ed25519` is the algorithm used to generate the keys; at the time of writing, it's considered secure)
  - If you want automated (passwordless) access to the server, don't enter a passphrase when prompted; leave it blank
  - If you used the default settings, this will create the files:
    - `~/.ssh/id_ed25519` (your private key), and
    - `~/.ssh/id_ed25519.pub` (your public key)
  - Copy your public key to the remote machine using `ssh-copy-id -i ~/.ssh/id_ed25519.pub <remote-user>@<remote-IP-or-hostname>`
    - Your public key will be appended to the file `~/.ssh/authorized_keys` on the remote machine

  - **WARNING**: Keep your private key safe! If compromised (i.e. someone other than you gets access to it), you should immediately delete the corresponding public key from the `~/.ssh/authorized_keys` file on any remote machine that has it and inform the admin.

Now, if you SSH into the remote machine, it should log you in automatically!

## SSH config files (`man ssh_config`)

`ssh <username-on-machine-B>@<hostname-or-IP-address-of-machine-B>` is a mouthful: I can't remember long hostnames or IP addresses. Instead, you can use a configuration file to rename these to more convenient aliases.

Here's the SSH config file at `/.ssh/config` on my local machine:

```
# This block makes all my connections (Host *) persistent so that SSHing again into the same machine uses the existing tunnel instead of creating a new one. It also uses my private key for authenticating to every server.

Host *
   ControlMaster auto
   ControlPath ~/.ssh/control-%r@%h:%p
   ControlPersist 3s
   IdentityFile /home/raj/.ssh/id_ed25519  # path to my local private key used for key-based authentication

# The following blocks describe aliases to the Bonner Lab workstation, Bonner Lab storage server, MARCC login node and MARCC data transfer node respectively.

Host lab-server
    HostName 10.160.192.70  # Bonner Lab workstation
    User rgautha1@jh.edu  # my username on the Bonner Lab workstation

Host lab-storage
    HostName 10.99.95.227
    User rgautha1@jh.edu

Host marcc
    HostName login.rockfish.jhu.edu
    User rgautha1

Host marcc-dtn
    HostName rfdtn1.rockfish.jhu.edu
    User rgautha1
```

With the above configuration, if I run `ssh lab-server`, I get immediate access to a terminal on the Bonner Lab workstation, which is super convenient.
