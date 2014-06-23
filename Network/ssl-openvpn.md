---
title: Building a VPN SSL server on exoscale
date: 2014-06-23 09:52 +01:00
category: Network
tags: linux, ssl, vpn, security, encryption, debian, ubuntu
---

[OpenVPN](https://openvpn.net/index.php/open-source/333-what-is-openvpn.html) enables 
secure client to server or network connections via a single TCP or UDP port encrypted with
SSL.
It is the perfect companion for a rapid and secure multitier application deployement where
applications trafic between the client and the app is not always encrypted or well
described to apply security directly.

## Proposed setup

We are going to use the following:

* a VPN server (under Ubuntu or Debian)
* clients with Mac and Windows laptops
* Additional application servers that are only visible to clients through the VPN

![architecture](openvpn-1.png)

## Installing the server

Start by launching an exoscale instance. Select Ubuntu or Debian as the
image, a 10 GB drive size and your SSH key.

It is best to create a dedicated security group for the OpenVPN server. 
I am using a security group called `VPN`in which I have the following rules:

* SSH: TCP port 22 allowed from 0.0.0.0
* VPN: TCP port 6443 allowed from 0.0.0.0 (note: you can configure any other port)

Once the server is running, connect as root and update it

    apt-get update
    apt-get upgrade

Install the OpenVPN server

    apt-get install openvpn

## Configure the SSL layer and VPN stack

The OpenVPN suite comes with a collection of scripts that can be used 
to ease the creation of an certificate authority and the issuing of 
both server and client certificates and keys.

We are going to rely on those. They are stored in 
`/usr/share/doc/openvpn/examples/easy-rsa/2.0/` 

Copy the basic set to the OpenVPN configuration files directory

    mkdir /etc/openvpn/easy-rsa
    cp -r /usr/share/doc/openvpn/examples/easy-rsa/2.0/* /etc/openvpn/easy-rsa/

### Adjust your settings

The collection of scripts relies on the `vars` files for issuing certificates.
Edit this file and adjust the values of the following lines to your liking:

    export KEY_COUNTRY="US"
    export KEY_PROVINCE="CA"
    export KEY_CITY="SanFrancisco"
    export KEY_ORG="Fort-Funston"
    export KEY_EMAIL="me@myhost.mydomain"
    export KEY_EMAIL=mail@host.domain
    export KEY_CN=changeme
    export KEY_NAME=changeme
    export KEY_OU=changeme
    export PKCS11_MODULE_PATH=changeme
    export PKCS11_PIN=1234

### Initialize the PKI and create the server certificate

Once the variables are adjusted, we are going to import them in our 
shell session:

    cd /etc/openvpn/easy-rsa/
    source vars

Then we will clean the default certicates and CA

    ./clean-all

finally initialize the CA and server certificates

    ./build-dh
    ./pkitool --initca
    ./pkitool --server server
    openvpn --genkey --secret keys/ta.key

Now the CA is initialized and we will copy the relevant files in the
main OpenVPN configuration directory

    cp keys/ca.crt keys/ta.key keys/server.crt keys/server.key keys/dh1024.pem /etc/openvpn/

### Configure the settings

In order for OpenVPN to operate in a CHROOT jail we will create a directory for this

    mkdir /etc/openvpn/jail

and then create a `/etc/openvpn/server.conf file with the following contents

    # Server TCP/6443
    mode server
    proto tcp
    port 6443
    dev tun
    # Certificates
    ca ca.crt
    cert server.crt
    key server.key
    dh dh1024.pem
    tls-auth ta.key 1
    key-direction 0
    cipher AES-256-CBC
    # Network settings
    server 10.8.0.0 255.255.255.0
    push "redirect-gateway def1 bypass-dhcp"
    push "dhcp-option DNS 8.8.8.8"
    push "dhcp-option DNS 8.8.4.4"
    keepalive 10 120
    # Security and user
    user nobody
    group nogroup
    chroot /etc/openvpn/jail
    persist-key
    persist-tun
    comp-lzo
    # Log
    verb 3
    mute 20
    status openvpn-status.log
    ; log-append /var/log/openvpn.log

This file enables the creation of an OpenVPN configuration on TCP port 6443
that will issue IP addresses in the 10.8.0.0/24 range to our clients.

The server will run as the nobody user and be confined in a jail
in case the OpenVPN stack was compromised.

### Start the server

to start the server issue the command

    openvpn server.conf

you should get to the following successful message

    root@openvpn:/etc/openvpn# openvpn openvpn.conf
    Mon Jun 23 10:43:48 2014 OpenVPN 2.2.1 x86_64-linux-gnu [SSL] [LZO2] [EPOLL] [PKCS11] [eurephia] [MH] [PF_INET6] [IPv6 payload 20110424-2 (2.2RC2)] built on Jun 18 2013
    Mon Jun 23 10:43:48 2014 NOTE: OpenVPN 2.1 requires '--script-security 2' or higher to call user-defined scripts or executables
    Mon Jun 23 10:43:48 2014 Diffie-Hellman initialized with 1024 bit key
    Mon Jun 23 10:43:48 2014 Control Channel Authentication: using 'ta.key' as a OpenVPN static key file
    Mon Jun 23 10:43:48 2014 Outgoing Control Channel Authentication: Using 160 bit message hash 'SHA1' for HMAC authentication
    Mon Jun 23 10:43:48 2014 Incoming Control Channel Authentication: Using 160 bit message hash 'SHA1' for HMAC authentication
    Mon Jun 23 10:43:48 2014 TLS-Auth MTU parms [ L:1560 D:168 EF:68 EB:0 ET:0 EL:0 ]
    Mon Jun 23 10:43:48 2014 Socket Buffers: R=[87380->131072] S=[16384->131072]
    Mon Jun 23 10:43:48 2014 ROUTE default_gateway=185.19.28.1
    Mon Jun 23 10:43:48 2014 TUN/TAP device tun0 opened
    Mon Jun 23 10:43:48 2014 TUN/TAP TX queue length set to 100
    Mon Jun 23 10:43:48 2014 do_ifconfig, tt->ipv6=0, tt->did_ifconfig_ipv6_setup=0
    Mon Jun 23 10:43:48 2014 /sbin/ifconfig tun0 10.8.0.1 pointopoint 10.8.0.2 mtu 1500
    Mon Jun 23 10:43:48 2014 /sbin/route add -net 10.8.0.0 netmask 255.255.255.0 gw 10.8.0.2
    Mon Jun 23 10:43:48 2014 Data Channel MTU parms [ L:1560 D:1450 EF:60 EB:135 ET:0 EL:0 AF:3/1 ]
    Mon Jun 23 08:43:48 2014 chroot to '/etc/openvpn/jail' and cd to '/' succeeded
    Mon Jun 23 08:43:48 2014 GID set to nogroup
    Mon Jun 23 08:43:48 2014 UID set to nobody
    Mon Jun 23 08:43:48 2014 Listening for incoming TCP connection on [undef]
    Mon Jun 23 08:43:48 2014 TCPv4_SERVER link local (bound): [undef]
    Mon Jun 23 08:43:48 2014 TCPv4_SERVER link remote: [undef]
    Mon Jun 23 08:43:48 2014 MULTI: multi_init called, r=256 v=256
    Mon Jun 23 08:43:48 2014 IFCONFIG POOL: base=10.8.0.4 size=62, ipv6=0
    Mon Jun 23 08:43:48 2014 MULTI: TCP INIT maxclients=1024 maxevents=1028
    Mon Jun 23 08:43:48 2014 Initialization Sequence Completed

now that the server is working, uncomment the last line
of the `/etc/openvpn/server.conf` file to stop logs from
appearing on STDOUT

    log-append /var/log/openvpn.log

The server can now be launched as a deamon

    service openvpn start

## Routing

If the tunnel is only used to access this specific server then the server side configuration
is over. In most cases we will want to use the OpenVPN server as 
a gateway to other servers. For this we need to enable forwarding on
the server.

    sh -c 'echo 1 > /proc/sys/net/ipv4/ip_forward'

and add the line add the end of `/etc/sysctl.conf`

    net.ipv4.ip_forward = 1

for a permanent setting across reboots.

## Generating client certicates and keys

for each client that will connect to the OpenVPN
server create a clientconf directory

    mkdir -p /etc/openvpn/clientconf/clientA

then generate the files

    cd /etc/openvpn/easy-rsa
    source vars
    ./build-keys clientA

The script has generated 3 files in the `/etc/openvpn/easy-rsa/keys`
directory:

* clientA.csr: the certificate request
* clientA.crt: the client certificate
* clientA.key: the client private key

copy all 3 files and the root certificate in `/etc/openvpn/clientconf/clientA` 

    cp ca.crt ta.key  keys/clientA* /etc/openvpn/clientconf/clientA/

create a client configuration file `/etc/openvpn/clientconf/clientA/client.conf`

    # Client
    client
    dev tun
    proto tcp-client
    remote AAA.BBB.CCC.DDD. 6443
    resolv-retry infinite
    cipher AES-256-CBC
    # Keys
    ca ca.crt
    cert clientA.crt
    key clientA.key
    tls-auth ta.key 1
    key-direction 1
    # Security
    nobind
    persist-key
    persist-tun
    comp-lzo
    verb 3

be sure to replace the AAA.BBB.CCC.DDD ip address by the public ip
address of the VPN server you are working on
`ifconfig eth0|grep "inet addr"`

copy the `client.conf` file as `client.ovpn` in order for Windows
client to retrieve settings automatically.

we will create a zip file of the client directory and transfer it on the 
clientA laptop

    cd /etc/openvpn/clientcong/clientA
    zip clientA-vpn.zip *.*

## Connecting the client

Download the client tool

* [Windows](https://openvpn.net/index.php/open-source/downloads.html)
* Mac: with the [TunnelBlick client](https://code.google.com/p/tunnelblick/) 
* Linux: just install the openvpn package and on Ubuntu the desktop helper (network manager)

unzip the clientA.zip file and point the client to the `client.ovpn` or 
`client.conf` file. Trust and create the profile.

## Enabling communications beyond the OpenVPN server

Various combinations can be done to communicate between client and servers:

* route all trafic through the VPN and allow/disallow from there
* push specific routes to server side destination on VPN initialization

read on at http://openvpn.net/index.php/open-source/documentation/howto.html#scope
for extended possibilities

## notes

the easy-rsa tool suite can be found at https://github.com/OpenVPN/easy-rsa
