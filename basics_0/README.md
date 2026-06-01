# Networking Basics #0

This directory contains the answers and scripts for the Networking Basics #0 tasks.

## Files

- `0-OSI_model`: quiz answers for the OSI model.
- `1-types_of_network`: quiz answers for LAN, WAN, and Internet network types.
- `2-MAC_and_IP_address`: quiz answers for MAC and IP addresses.
- `3-UDP_and_TCP`: quiz answers for TCP and UDP behavior.
- `4-TCP_and_UDP_ports`: Bash script that displays listening ports with PID/program names.
- `5-is_the_host_on_the_network`: Bash script that pings a host five times.

## 0. OSI model

Question: What is the OSI model?

Answer: 2 - The OSI model is a conceptual model that characterizes the communication functions of a telecommunication system without regard to their underlying internal structure and technology.

Question: How is the OSI model organized?

Answer: 2 - From the lowest to the highest level.

## 1. Types of network

Question: What type of network a computer in local is connected to?

Answer: 3 - LAN.

Question: What type of network could connect an office in one building to another office in a building a few streets away?

Answer: 2 - WAN.

Question: What network do you use when you browse www.google.com from your smartphone, when not connected to the Wifi?

Answer: 1 - Internet.

## 2. MAC and IP address

Question: What is a MAC address?

Answer: 2 - The unique identifier of a network interface.

Question: What is an IP address?

Answer: 1 - Is to devices connected to a network what postal address is to houses.

## 3. UDP and TCP

Question: Which statement is correct for the TCP box?

Answer: 1 - It is a protocol that is transferring data in a slow way but surely.

Question: Which statement is correct for the UDP box?

Answer: 2 - It is a protocol that is transferring data in a fast way and might loss data along in the process.

Question: Which statement is correct for the TCP worker?

Answer: 1 - Have you received boxes x, y, z?

## 4. TCP and UDP ports

Question: Write a Bash script that displays listening ports, only shows listening sockets, and shows the PID and name of the program to which each socket belongs.

Answer:

```bash
#!/usr/bin/env bash
# Display listening ports with their PID and program name.
netstat -lpn
```

## 5. Is the host on the network

Question: Write a Bash script that accepts an IP address as an argument and pings it five times. If no argument is passed, display `Usage: 5-is_the_host_on_the_network {IP_ADDRESS}`.

Answer:

```bash
#!/usr/bin/env bash
# Ping the host passed as the first argument.
if [ "$#" -lt 1 ]; then
    echo "Usage: 5-is_the_host_on_the_network {IP_ADDRESS}"
    exit 1
fi

ping -c 5 "$1"
```
