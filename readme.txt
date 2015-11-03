Netstat.py

This python script is designed to work with Python 2.6 and 2.7 for Linux systems. It is based off of a python netstat script that was originally written by Ricardo Pascal, available on http://voorloopnul.com/blog/a-python-netstat-in-less-than-100-lines-of-code/ with some improvements to the original (udp support, ipv6 support as well as support for identifying packet sockets... more on this in a minute)

This script is designed as a clone of the Linux/Unix netstat command. It's use case would be in the event that the system's 'netstat' or 'ss' binary are suspected to have been modified and can no longer be trusted (userland binary tampering) The script reads directly from /proc/net/tcp, /proc/net/udp, /proc/net/tcp6, /proc/net/udp6 and additionally, /proc/net/packet. This reveals all TCP and UDP connections on the host (ipv4 OR ipv6) in addition to notifying you of all processes with open packet sockets (most sniffing programs like tcpdump utilize packet sockets for sniffing network traffic, although some other programs like dhclient and wpa_supplicant also utilize packet sockets as well.)

For best effect, the script will need to be ran as root (to display the pid and executable name associated with a given connection, like the "netstat -p" option), but can optionally be ran as a standard user if you're willing to deal with the loss of some functionality.

sample output:
[root@Exterminatus ~]# ./netstat.py

Legend: Connection ID, UID, localhost:localport, remotehost:remoteport, state, pid, exe name

TCP (v4) Results:

['0:', 'root', '0.0.0.0:42550', '0.0.0.0:0', 'LISTEN', '1843', '/usr/sbin/sshd']
['1:', 'root', '127.0.0.1:25', '0.0.0.0:0', 'LISTEN', '1922', '/usr/libexec/postfix/master']
['2:', 'root', '192.168.1.18:22', '192.168.1.17:51078', 'ESTABLISHED', '4393', '/usr/sbin/sshd']
['3:', 'root', '192.168.1.18:22', '192.168.1.17:51013', 'ESTABLISHED', '4361', '/usr/sbin/sshd']

TCP (v6) Results:

['0:', 'root', '00:00:00:00:00:00:00:00:00:00:00:00:00:00:00:00:22', '00:00:00:00:00:00:00:00:00:00:00:00:00:00:00:00:0', 'LISTEN', '1843', '/usr/sbin/sshd']
['1:', 'root', '00:00:00:00:00:00:00:00:00:00:00:00:00:00:00:01:25', '00:00:00:00:00:00:00:00:00:00:00:00:00:00:00:00:0', 'LISTEN', '1922', '/usr/libexec/postfix/master']

UDP (v4) Results:

['71:', 'root', '0.0.0.0:68', '0.0.0.0:0', 'Stateless', '1414', '/sbin/dhclient']

UDP (v6) Results:


Packet Socket Results:

['1717', '/usr/sbin/wpa_supplicant (deleted)']
['1414', '/sbin/dhclient']