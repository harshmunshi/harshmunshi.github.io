# How to make a Bootable Drive in Linux

In case you want to make a bootable pen drive using ubuntu and you cannot find resources, you've come to a right place.
Usually all the modules are windows based since people don't have linux! (da?)

## 1. Essential Downloads

Download the version of Ubuntu you want to make a drive of [here](https://www.ubuntu.com/download/desktop).

## 2. Pen Drive cleanup

Plug in your pen drive and format it. You can use any software you want but I prefer gparted. To install gparted hit the following command:

```
$ sudo apt-get install gparted
```

To locate your pen drive type the following command:

```
$ df -h
```
In my case the location was /dev/sdb1

After the installation you can open the application using the UI or just type gparted in your terminal. On the top right corner you can 
select /dev/sdb1 (in my case) and format, resize the partition etc.

## 3. Unmount the partition (pen drive)

Your pendrive is physically located under /media/<root_name>. To unmount type the following command:

```
$ sudo umount /media/<root_name>/<name of the partition>
```
The name of the partition is different for everyone. 

## 4. Time for Magic!

To transfer the image onto your pen drive hit the following command:

```
$ sudo dd if=<path_to_the_linux_image> of=/dev/sdb1 bs=4m && sync
```

wait for few minutes and you're done!
