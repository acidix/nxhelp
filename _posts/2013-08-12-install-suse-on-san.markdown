---
comments: true
layout: post
title: Safely install SuSE Linux Enterprise Server on a SAN Volume
tags:
- Linux
- Storage
---
Running diskless servers sounds like a great idea - if it wasn't for the fiddling around with auto-install scripts to finally get to a working system. I want to share and comment on my AutoYaST configuration that has served me well over the last couple of months.

<!-- more -->

The only thing this solution depends on is that the device you want to install on is connected an Lun-ID 00 - which is a prerequisite for SAN boot for most controllers anyway.

## Bootloader Configuration

The initial bootloader configuration is rather straightforward. Simply setting it to `(hd0)`, deciding to bother about selecting the right `hd0` later and moving on:

```xml
<loader_type>grub</loader_type>
<sections config:type="list">
    <section>
        <append>ksdevice=bootif locale=en_US text priority=critical resume=/dev/vg_%%HOSTNAME%%/lv_swap splash=silent crashkernel=256M-:128M showopts</append>
        <image>(hd0,0)/vmlinuz-3.0.13-0.27-default</image>
        <initial>1</initial>
        <initrd>(hd0,0)/initrd-3.0.13-0.27-default</initrd>
        <lines_cache_id>0</lines_cache_id>
        <name>SLES for SAP Applications - 3.0.13-0.27</name>
        <original_name>linux</original_name>
        <root>/dev/vg_%%HOSTNAME%%/lv_root</root>
        <type>image</type>
    </section>
```

## Pre-Scripts

Here things are getting a bit more complicated. In the Pre-Script I am doing 2 things:

1. Renaming the volume group to incorporate the hostname
2. Selecting the right volume to install the system on

Here is my pre-script. The magic is in assigning the `$mpathvol` variable. Paying attention to only use commands that are actually available at this stage of the install is crucial here.

See that I am using 2 strings to match against the `/proc/scsi/scsi` output - which strings you need highly depends on the SAN setup you have. I recommend checking on sn installed system to find a string that's not too general and lets you find the device(s) safely.

```xml
<pre-scripts config:type="list">
    <script>
        <debug config:type="boolean">false</debug>
        <feedback config:type="boolean">true</feedback>
        <filename>config-hostname.sh</filename>
        <interpreter>shell</interpreter>
        <source><![CDATA[
#!/bin/bash
# Find multipathed volume on LUN ID 00
# IBM is for volumes mapped by SVC
# WD3000 is for local devices of IBM Blades
mpathvol=\$(cat /proc/scsi/scsi | egrep -B1 "IBM|WD3000" | grep "Lun: 00" | while read x1 dev x2 chan x3 id x4 lun; do echo \${dev/scsi/}\:\${chan:1}\:\${id:1}\:\${lun:1}; done | while read device; do ls -1 /sys/class/scsi_device/\$device/device/block; done | tr '\n' '|' | sed 's/|\$/\n/g' | while read search; do multipath -ll | egrep -w -B3 "\$search"; done | head -n1 | cut -d" " -f2)

# Set the correct Volume-Group Name
cat /tmp/profile/autoinst.xml | sed "s,%%HOSTNAME%%,`echo \$HOSTNAME | cut -d"." -f1`,g" > /tmp/profile/step1.xml

# Set the correct root device & LVM settings
# Check if we have a multipathed environment
if [ "_\$mpathvol" != "_" ]; then
    cat /tmp/profile/step1.xml | sed "s,/dev/sda,/dev/\${mpathvol},g" >/tmp/profile/modified.xml

    # Do not paste the "Enable all devices for LVM" snipped here as LVM will spit on the reoccurrence of devices by path
else
    mv /tmp/profile/step1.xml /tmp/profile/modified.xml

    # Enable all devices for LVM
    sed -i 's#\[ "r|/dev/\.\*/by-path/\.\*|", "a|/dev/mapper/\.\*|", "a|/dev/\.\*/by-id/\.\*|", "r/\.\*/" \]#\[ "a/\.\*/" \]#g' /etc/lvm/lvm.conf
fi

]]>
        </source>
    </script>
</pre-scripts>
```

As long as you assign the final autoyast configuration to `/tmp/profile/modified.xml` the installer will pick it up and use it instead of the autoinst.xml.

## Post-Scripts

Here we are, having our system installed on the correct disk - so we should be good to go. Wrong. The system will only boot if it's really (really) diskless. As soon as there's another local disk we're screwed. Grub will install on `hd0`, and that's the local disk we don't want it to install on. I am working around this with 2 post-scripts.

The reason I have 2 post scripts is actually a bit complicated. The first script doesn't run chrooted so we can still be sure to have multipath support that'll be crucial for getting the SCSI-ID of the device. The second script however does run chrooted to make sure we're using the correct grub commands to install the bootloader (plus, the bootloader will install in this stage and our 'device.map' would get overwritten by the installer):

```xml
<chroot-scripts config:type="list">
     <script>
         <debug config:type="boolean">false</debug>
         <feedback config:type="boolean">false</feedback>
         <filename>config.sh</filename>
         <interpreter>shell</interpreter>
         <chrooted config:type="boolean">false</chrooted>
         <source><![CDATA[
#!/bin/bash
mpathid=\$(cat /proc/scsi/scsi | egrep -B1 "IBM|WD3000" | grep "Lun: 00" | while read x1 dev x2 chan x3 id x4 lun; do echo \${dev/scsi/}\:\${chan:1}\:\${id:1}\:\${lun:1}; done | while read device; do ls -1 /sys/class/scsi_device/\$device/device/block; done | tr '\n' '|' | sed 's/|\$/\n/g' | while read search; do multipath -ll | egrep -w -B3 "\$search"; done | head -n1 | cut -d" " -f1)

# Set the correct root device & LVM settings
# Check if we have a multipathed environment
if [ "_\$mpathid" != "_" ]; then
    echo -e "(hd0)\t/dev/disk/by-id/scsi-\$mpathid" >/boot/grub/device.map
    echo -e "(hd0)\t/dev/disk/by-id/scsi-\$mpathid" >/mnt/etc/device.map
fi
]]>
        </source>
    </script>
    <script>
        <debug config:type="boolean">false</debug>
        <feedback config:type="boolean">false</feedback>
        <filename>config-chroot.sh</filename>
        <interpreter>shell</interpreter>
        <chrooted config:type="boolean">true</chrooted>
        <source><![CDATA[
#!/bin/bash
if [ -f /etc/device.map ]; then
    mv /etc/device.map /boot/grub/device.map
    /usr/sbin/grub <<EOF
root (hd0,0)
setup (hd0)
EOF
fi
]]>
        </source>
    </script>
</chroot-scripts>
```

I am planning to do a Debian / Ubuntu preseed version as well, so stay tuned.

You can download the whole AutoYaST configuration here: [autoyast_san.xml]({% postfile autoyast_san.xml %})
