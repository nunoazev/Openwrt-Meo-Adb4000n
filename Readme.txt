Flashing Firmware

1º step
You must set your TCP/IP v4 protocol to:
1-IP: 192.168.1.2
2-Netmask: 255.255.255.0
3-Gateway: 192.168.1.1
4-DNS: (optional, can be blank).


2º step
Does not always work, you need try

Power off the router.
Press reset button near the antenna and power on.
Keep it pressed while powering up during ~20+ seconds.
Acces to http://192.168.1.1 and upload binary file.
Wait until router reboots.
------------------------------------------------

Adsl Setup.

1º use winscp to upload adsl.bin file to /lib/firmware/
2º configure Adsl interface  vim /etc/config/network 

adsl interface looks like:

config interface 'loopback'
        option ifname 'lo'
        option proto 'static'
        option ipaddr '127.0.0.1'
        option netmask '255.0.0.0'

config globals 'globals'
        option ula_prefix 'fd2b:0c81:0b8f::/48'

config interface 'lan'
        option ifname 'eth0.1'
        option force_link '1'
        option type 'bridge'
        option proto 'static'
        option ipaddr '192.168.1.1'
        option netmask '255.255.255.0'
        option ip6assign '60'

config switch
        option name 'eth0'
        option reset '1'
        option enable_vlan '1'

config switch_vlan
        option device 'eth0'
        option vlan '1'
        option ports '0 1 2 3 8t'

config atm-bridge 'atm'
        option encaps 'llc'
        option payload 'bridged'
        option vci '35'
        option vpi '0'

config interface 'wan'
        option proto 'pppoe'
        option macaddr '11:22:43:33:aa:ac'
        option _orig_ifname 'nas0'
        option _orig_bridge 'false'
        option ifname 'nas0'
        option ipv6 '1'
        option username 'username'
        option password 'password'

config vdsl 'dsl'
        option annex 'a'
        option firmware '/lib/firmware/vdsl.bin'
        option tone 'av'
        option xfer_mode 'atm'