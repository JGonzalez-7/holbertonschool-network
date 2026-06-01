# Networking Basics #1

This directory contains the Bash scripts and task notes for Networking Basics #1.

## Files

- `0-change_your_home_IP`: configures custom hostname mappings in `/etc/hosts`.
- `1-show_attached_IPs`: displays active IPv4 addresses on the machine.
- `2-port_listening_on_localhost`: listens for TCP connections on localhost port 98.

## 0. Change your home IP

### Code answer

File: `0-change_your_home_IP`

```bash
#!/usr/bin/env bash
# Configures custom IP resolutions for localhost and facebook.com.

sed -i 's/^127\.0\.0\.1[[:space:]]\+localhost/127.0.0.2 localhost/' /etc/hosts
sed -i '/facebook\.com/d' /etc/hosts
echo "8.8.8.8 facebook.com" >> /etc/hosts
```

Make it executable:

```bash
chmod +x 0-change_your_home_IP
```

Run it with `sudo` because `/etc/hosts` is a protected system file:

```bash
sudo ./0-change_your_home_IP
```

### Explanation

This task modifies the `/etc/hosts` file.

The `/etc/hosts` file is used by your machine to manually map hostnames to IP addresses before asking DNS servers.

This line:

```bash
sed -i 's/^127\.0\.0\.1[[:space:]]\+localhost/127.0.0.2 localhost/' /etc/hosts
```

changes the `localhost` entry from:

```text
127.0.0.1 localhost
```

to:

```text
127.0.0.2 localhost
```

This line:

```bash
sed -i '/facebook\.com/d' /etc/hosts
```

removes any old `facebook.com` entry so you do not create duplicates.

This line:

```bash
echo "8.8.8.8 facebook.com" >> /etc/hosts
```

adds a new manual rule that makes `facebook.com` resolve to `8.8.8.8`.

Important: after testing, change `localhost` back to `127.0.0.1`, because many programs expect `localhost` to use the normal loopback address.

To revert it later:

```bash
sudo sed -i 's/^127\.0\.0\.2[[:space:]]\+localhost/127.0.0.1 localhost/' /etc/hosts
```

### Expected output

Before running the script:

```bash
ping localhost
```

You may see:

```text
PING localhost (127.0.0.1) 56(84) bytes of data.
```

After running the script:

```bash
sudo ./0-change_your_home_IP
ping localhost
```

You should see:

```text
PING localhost (127.0.0.2) 56(84) bytes of data.
```

For Facebook:

```bash
ping facebook.com
```

You should see:

```text
PING facebook.com (8.8.8.8) 56(84) bytes of data.
```

### Git commit command

```bash
git add 0-change_your_home_IP
git commit -m "implemented custom hostname IP mappings"
```

## 1. Show attached IPs

### Code answer

File: `1-show_attached_IPs`

```bash
#!/usr/bin/env bash
# Displays all active IPv4 addresses on the machine.

ip -4 addr show up | awk '/inet / {print $2}' | cut -d/ -f1
```

Make it executable:

```bash
chmod +x 1-show_attached_IPs
```

Run it:

```bash
./1-show_attached_IPs
```

### Explanation

This script displays all active IPv4 addresses on the machine.

This command:

```bash
ip -4 addr show up
```

shows only IPv4 addresses from active network interfaces.

The `awk` part:

```bash
awk '/inet / {print $2}'
```

finds lines that contain IPv4 addresses and prints the second field.

Example line from `ip` output:

```text
inet 10.0.2.15/24 brd 10.0.2.255 scope global dynamic eth0
```

The second field is:

```text
10.0.2.15/24
```

Then this part:

```bash
cut -d/ -f1
```

removes the subnet suffix `/24`, leaving only the IP address:

```text
10.0.2.15
```

This should also show the localhost IPv4 address:

```text
127.0.0.1
```

or `127.0.0.2` if you already ran Task 0 and changed localhost.

### Expected output

Example:

```bash
./1-show_attached_IPs | cat -e
```

Output may look like:

```text
10.0.2.15$
127.0.0.1$
```

Your IP addresses may be different depending on your machine or sandbox.

### Git commit command

```bash
git add 1-show_attached_IPs
git commit -m "implemented active IPv4 address display"
```

## 2. Port listening on localhost

### Code answer

File: `2-port_listening_on_localhost`

```bash
#!/usr/bin/env bash
# Listens for incoming TCP connections on localhost port 98.

nc -l 127.0.0.1 98
```

Make it executable:

```bash
chmod +x 2-port_listening_on_localhost
```

Run it with `sudo`:

```bash
sudo ./2-port_listening_on_localhost
```

### Explanation

This task uses `nc`, also called Netcat.

Netcat is a networking tool that can create TCP or UDP connections. It is useful for testing ports, checking if a server is reachable, or debugging network connections.

This command:

```bash
nc -l 127.0.0.1 98
```

means:

`nc` runs Netcat.

`-l` puts Netcat in listening mode.

`127.0.0.1` means it listens only on localhost.

`98` is the port number.

Port 98 is a privileged port because it is below 1024, so you need to run the script with `sudo`.

Once the script is listening, another terminal can connect to it using:

```bash
telnet localhost 98
```

Anything typed in the Telnet terminal will appear in the terminal running the Netcat listener.

### Expected output

In one terminal:

```bash
sudo ./2-port_listening_on_localhost
```

In another terminal:

```bash
telnet localhost 98
```

When you type text in the Telnet terminal, it should appear in the Netcat listener terminal.

### Git commit command

```bash
git add 2-port_listening_on_localhost
git commit -m "implemented localhost port listener"
```
