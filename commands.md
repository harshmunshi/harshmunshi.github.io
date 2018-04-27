
# Making custom commands in Ubuntu

Hello folks!! Been long since I've written a blog. Today we are going to make custom commands which are global and can be used from anywhere any directory. Since I've been working in ROS (Robot Operating system) I was in awe by the shortcuts that it offered. For example this command:

```$ roscd ```

takes you to your ROS dvelve folder which is the current workspace. I was inspired to make something like this as well. So how can we do it?

## Building a bash file

So let's say I have a Masters folder under documents and I want to make a command which can directly take me there. We make the following bash file:

```#!/bin/bash ```
```cd /home/harsh/Document/Masters```
```exec bash ```

Save this as let's say "mastercd" . Then do the following:

```$ chmod +x mastercd```

Congrats, your command is ready but wait! not yet :)

## Making the command global

To make your command global place it under /usr/bin

```$ sudo mv mastercd /usr/bin```

Open a new terminal and run:

```$ mastercd```

and Boom! You've just made your own command!

SO this is it for this time, goodbye until next time :)
