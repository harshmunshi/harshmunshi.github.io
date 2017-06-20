## Hacking IP cams using ONVIF protocol

Onvif which is a short form of Open Network Video Interface Forum, is an industrial standard for network cameras. However the existing support and documentation is very limited as far as this protocol is concerened. In this module we will try to make a very simple code to know the device name using python and an open source library.

## IP cams and Safety

Most of the IP cameras use this protocol but there are is a device specific password which can be modified. There are lots of camera manufacturers which have different set of default ID and Password; the most common one being 'admin', 'admin'. Network cameras are pretty safe for the fact that you have to be on the network and have proper authentication level to access any camera. However, the network security can be dodged, but generally not so easy.

## IPCam Hardware

Depending on the type of camera and it's power requirement, the hardware changes. But in general, if you are using a good camera with your laptop, you will require a PoE (Power over Ethernet) module, lan cables, laptop and the camera itself. The genral diagram looks like the following:

![alt text](http://www.veracityglobal.com/media/20689/pinpoint-diagram-large.png)

In case you are trying to use several IP cameras at the same time you requrie an NVR (Network Video Recorder) which can record 'N' video feeds in parallel. The value 'N' varies with different companies. In this case the architecture would look like the following:

<div style="text-align:center"><img src ="https://www.securitycameraking.com/securityinfo/wp-content/uploads/2014/07/POE-Setup.jpg" /></div>
