# Holberton School Network

This repository contains introductory networking projects for Holberton School. The files cover networking concepts, TCP/UDP behavior, ports, ICMP, host resolution, and basic Bash scripts for inspecting or testing network behavior on Ubuntu.

## Repository Structure

```text
.
├── basics_0
│   ├── 0-OSI_model
│   ├── 1-types_of_network
│   ├── 2-MAC_and_IP_address
│   ├── 3-UDP_and_TCP
│   ├── 4-TCP_and_UDP_ports
│   ├── 5-is_the_host_on_the_network
│   └── README.md
└── basics_1
    ├── 0-change_your_home_IP
    ├── 1-show_attached_IPs
    ├── 2-port_listening_on_localhost
    └── README.md
```

## Basics 0

The `basics_0` directory contains quiz answers and small Bash scripts for Networking Basics #0.

| File | Description |
| --- | --- |
| `0-OSI_model` | Numeric answers about the OSI model and its layer organization. |
| `1-types_of_network` | Numeric answers about LAN, WAN, and Internet network types. |
| `2-MAC_and_IP_address` | Numeric answers about MAC addresses and IP addresses. |
| `3-UDP_and_TCP` | Numeric answers comparing TCP and UDP behavior. |
| `4-TCP_and_UDP_ports` | Bash script that displays listening sockets with PID/program names. |
| `5-is_the_host_on_the_network` | Bash script that pings an IP address five times. |
| `README.md` | Full questions, answers, and script details for the directory. |

Example commands:

```bash
./basics_0/4-TCP_and_UDP_ports
./basics_0/5-is_the_host_on_the_network 8.8.8.8
```

## Basics 1

The `basics_1` directory contains Bash scripts for Networking Basics #1.

| File | Description |
| --- | --- |
| `0-change_your_home_IP` | Updates `/etc/hosts` so `localhost` resolves to `127.0.0.2` and `facebook.com` resolves to `8.8.8.8`. |
| `1-show_attached_IPs` | Displays active IPv4 addresses attached to the machine. |
| `2-port_listening_on_localhost` | Starts a Netcat listener on localhost port `98`. |
| `README.md` | Code answers, explanations, expected output, and task commit commands. |

Example commands:

```bash
sudo ./basics_1/0-change_your_home_IP
./basics_1/1-show_attached_IPs
sudo ./basics_1/2-port_listening_on_localhost
```

## Requirements

- Ubuntu or another Linux environment with Bash.
- `netstat` for `basics_0/4-TCP_and_UDP_ports`.
- `ping` for `basics_0/5-is_the_host_on_the_network`.
- `ip`, `awk`, and `cut` for `basics_1/1-show_attached_IPs`.
- `nc` for `basics_1/2-port_listening_on_localhost`.
- `sudo` for scripts that edit `/etc/hosts` or bind to privileged ports.

## Notes

Some scripts affect the local machine or require elevated permissions:

- `basics_1/0-change_your_home_IP` modifies `/etc/hosts`. After testing, restore `localhost` to `127.0.0.1`.
- `basics_1/2-port_listening_on_localhost` listens on port `98`, which is below `1024` and usually requires `sudo`.
- Quiz answer files contain only numbers, as required by the project.
