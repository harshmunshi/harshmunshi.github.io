## Hacking IP cams using ONVIF protocol

Onvif which is a short form of Open Network Video Interface Forum, is an industrial standard for network cameras. However the existing support and documentation is very limited as far as this protocol is concerened. In this module we will try to make a very simple code to know the device name using python and an open source library.

## IP cams and Safety

Most of the IP cameras use this protocol but there are is a device specific password which can be modified. There are lots of camera manufacturers which have different set of default ID and Password; the most common one being 'admin', 'admin'. Network cameras are pretty safe for the fact that you have to be on the network and have proper authentication level to access any camera. However, the network security can be dodged, but generally not so easy.

## IP Cam Hardware

Depending on the type of camera and it's power requirement, the hardware changes. But in general, if you are using a good camera with your laptop, you will require a PoE (Power over Ethernet) module, lan cables, laptop and the camera itself. The genral diagram looks like the following:

![alt text](http://www.veracityglobal.com/media/20689/pinpoint-diagram-large.png)

In case you are trying to use several IP cameras at the same time you requrie an NVR (Network Video Recorder) which can record 'N' video feeds in parallel. The value 'N' varies with different companies. In this case the architecture would look like the following:

<div style="text-align:center"><img src ="https://www.securitycameraking.com/securityinfo/wp-content/uploads/2014/07/POE-Setup.jpg" /></div>

## Finding the IP address of the Camera(s)

```
import math
```

## Installing Onvif and getting the Device Name

Enter the following set of commands to install Onvif

```
$ sudo pip install onvif
$ git clone https://github.com/quatanium/python-onvif.git
$ cd python-onvif && python setup.py install
```

This would install everything required. To know the device we write the following script:

```
from onvif import ONVIFCamera
mycam = ONVIFCamera('192.168.0.2', 80, 'user', 'passwd', '/etc/onvif/wsdl/')

# Get Hostname
resp = mycam.devicemgmt.GetHostname()
print 'My camera`s hostname: ' + str(resp.Name)

# Get system date and time
dt = mycam.devicemgmt.GetSystemDateAndTime()
tz = dt.TimeZone
year = dt.UTCDateTime.Date.Year
hour = dt.UTCDateTime.Time.Hour
```

## What Next?

You can go on and take a snapshot or record a video using this. The complete set of examples are in the repo [python-onvif](https://github.com/quatanium/python-onvif). If you get a good know-how of the system, you can go on and design your own Video Management System. 
