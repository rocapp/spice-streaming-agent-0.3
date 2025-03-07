# spice-streaming-agent-0.3

Forked from: https://gitlab.freedesktop.org/spice/spice-streaming-agent/-/tree/v0.3?ref_type=tags

Introduction
============

The SPICE Streaming Agent is a guest-side daemon which captures the
guest video output, encodes it to a video stream,and forwards the resulting
stream to the host to be sent through SPICE. The capture and encoding are done
through the use of plugins, so a variety of video format and capture/encoding
methods (hardware/software) can be used. For now, spice-streaming-agent only
provides software encoding to MJPEG.


Virtual Machine Configuration
=============================

In order to set up streaming, qemu needs to expose a
`org.spice-space.stream.0` virtio port, associated with a
corresponding Spice port.

Using virt-manager
------------------

In the hardware details, click on "Add Hardware", then select
"Channel". Add a "Spice port" device type with the
"org.spice-space.stream.0" name. You also need to set "Channel" to
"org.spice-space.stream.0"


Using libvirt
-------------

[source,xml]
<devices>
    <channel type='spiceport'>
        <source channel='org.spice-space.stream.0'/>
        <target type='virtio' name='org.spice-space.stream.0'/>
    </channel>
</devices>

Using QEMU
----------

[source,sh]
-device virtserialport,bus=virtio-serial0.0,nr=1,chardev=charchannel1,id=channel1,name=org.spice-space.stream.0 -chardev spiceport,name=org.spice-space.stream.0,id=charchannel1
