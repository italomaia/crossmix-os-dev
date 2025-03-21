# SSH

CrossMixOs comes with a built-in ssh server provided through [dropbear](https://github.com/mkj/dropbear).
It is a lightweight ssh server that runs well on resource scarse environments.

To connect to it, one needs to enable the ssh server that is **disabled by default** by going to "Apps" in the UI
and then `"System Tools" -> NETWORK -> SSH(<status>) -> "SSH Server - enable"`.

A "toast" message with the [ip address](https://en.wikipedia.org/wiki/IP_address) you should connect with is shown
for **6 seconds**; connect to it with `ssh root@<ip-address>`. Default password is `tina`. Official
docs [here](https://github.com/cizia64/CrossMix-OS/wiki/Apps/).

    NOTE
    If you enable the ssh server at any given time, it will stay active until you explicitly disable it,
    even after turning the device off.

    If the device goes into sleep mode, your ssh session will freeze.

## Tricks & Tips

### Setting Up Passwordless Auth

If you have your own ssh key ([create one with these instructions](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)) you can enable passwordless login.

    NOTE
    <ip-address> is whatever your device shared with you when you enabled the ssh server.

Just:

```sh
ssh-copy-id root@<ip-address>
ssh root@<ip-address>
mkdir .ssh
cp /etc/dropbear/authorized_keys ~/.ssh/
```

Now exit the ssh session and test everything is working by connecting again:

```sh
ssh -o PasswordAuthentication=no root@<ip-address>
```

Now edit "/root/sdcard/Apps/SystemTools/Menu/NETWORK##SSH (state)/SSH Server  - enable.sh" line 22 to look like this (the line that starts with `nice`):

```sh
# disables password authentication and drops idle connections
nice -2 dropbear -s -I 300 -k -j -R
```

Now, without exiting your current ssh session (just in case), try to connect again:

`ssh root@<ip-address>`

You should be able to connect without a password. If it did not work, it is probably related
to file permissions on the "authorized_keys" file. Try this:

`chmod 700 /etc/dropbear/authorized_keys`

Do consider creating an alias to your device ip address, it might simplify your life a bit:

`echo "<ip-address> crossmix" >> /etc/hosts`

Now you can connect to your device like this:

`ssh root@crossmix`

### .profile

`.profile` is loaded for every login shell session, so you can add some quality of life to
your shell sessions with something like this:

```
export SD=/root/sdcard
export SDBIN=$SD/System/bin
export PATH=$PATH:$HOME/bin

# keeps .ash_history size in check
mv .ash_history .ash_history.old; tail -n 50 .ash_history.old > .ash_history
```

Now you can easily add shortcuts to the binaries you most use under $HOME/bin.

### Shortcuts

If you find yourself going to some folders often, or have favority programs, create shortcuts:

```sh
mkdir $HOME/bin
ln -s $SDBIN/python3 $HOME/bin/
ln -s /mnt/SDCARD/ sdcard
ln -s /root/sdcard/Apps/SystemTools/Menu/ menu
```