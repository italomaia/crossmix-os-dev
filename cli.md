# Quality of Life Tips

This section will cover some basic quality of life tips when playing around
with your device using the command line (CLI).

```sh
# create a .profile
touch .profile
echo "export SD=/root/sdcard" >> .profile
echo "export PATH=$PATH:$SD/System/bin" >> .profile

# symlink to mounted sdcard as most of the fun happens there
ln -s /mnt/SDCARD/ ~/sdcard

# keep .ash_history size in check; add to .profile
mv .ash_history .ash_history.old; tail -n 50 .ash_history.old > .ash_history
```
