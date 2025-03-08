# SSH

CrossMixOs coms with a built-in ssh server provided through [dropbear](https://github.com/mkj/dropbear).
It is a lightweight ssh server that runs well on the Trimui.

To connect to it, one should `"System Tools"->NETWORK->SSH(<status>)->"SSH Server - enable"`. A "toast"
message with the [ip address](https://en.wikipedia.org/wiki/IP_address) you should connect with is shown
for **6 seconds**; connect to it with `ssh root@<ip-address>`. Default password is `tina`. Official
docs [here](https://github.com/cizia64/CrossMix-OS/wiki/Apps/).

If you have your own ssh key ([create one with these instructions](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)) you can also enable passwordless login.

Just:

```sh
ssh-copy-id root@<ip-address>
ssh root@<ip-address>
mkdir .ssh
cp /etc/dropbear/authorized_keys ~/.ssh/
```

Then edit "/root/sdcard/Apps/SystemTools/Menu/NETWORK##SSH (state)/SSH Server  - enable.sh" line 22 to look like this (the line that starts with `nice`):

`nice -2 dropbear -s -I 300 -k -j -R`

Also, CrossMixOS really likes blank spaces, which looks a little weird when command lining around.

Now, without exiting your ssh session, try to connect again:

`ssh root@<ip-address>`

You should be able to connect without a password now. If it did not work, you can always retrace your
steps and or undo the changes to the shell script and have password login back.