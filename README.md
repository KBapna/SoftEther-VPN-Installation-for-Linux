# SoftEther VPN Installation on Linux

This guide will walk you through the installation process of SoftEther VPN on Linux.

## Prerequisites

- Linux operating system
- sudo or root access

## Installation Steps

1. Update and upgrade system packages:
```bash
sudo apt update && sudo apt upgrade -y && sudo apt autoremove -y
```
2. Install the required dependencies:
```bash
sudo apt -y install build-essential wget curl gcc make wget tzdata git libreadline-dev libncurses-dev libssl-dev zlib1g-dev
```
3. Download SoftEther VPN Server:
```bash
wget https://www.softether-download.com/files/softether/v4.41-9787-rtm-2023.03.14-tree/Linux/SoftEther_VPN_Server/64bit_-_Intel_x64_or_AMD64/softether-vpnserver-v4.41-9787-rtm-2023.03.14-linux-x64-64bit.tar.gz
tar xzf softether-vpnserver-v4.41-9787-rtm-2023.03.14-linux-x64-64bit.tar.gz && rm softether-vpnserver-v4.41-9787-rtm-2023.03.14-linux-x64-64bit.tar.gz
```
4. Compile SoftEther VPN Server:
```bash
cd vpnserver && sudo make
cd ..
```
5. Move SoftEther VPN Server to the installation directory:
```bash
sudo mv vpnserver /usr/local && cd /usr/local/vpnserver/
```
6. Set permissions for the VPN Server:
```bash
sudo chmod 600 *
sudo chmod 700 vpnserver vpncmd
```
7. Start the SoftEther VPN Server:
```bash
sudo ./vpnserver start
```
8. Configure the SoftEther VPN Server:
```bash
sudo ./vpncmd
```
- Follow the prompts and set a server password.
```bash
ServerPasswordSet
```
9. Create a systemd service for the SoftEther VPN Server:
```bash
sudo cat >> /lib/systemd/system/vpnserver.service << EOF
[Unit]
Description=SoftEther VPN Server
After=network.target

[Service]
Type=forking
ExecStart=/usr/local/vpnserver/vpnserver start
ExecStop=/usr/local/vpnserver/vpnserver stop

[Install]
WantedBy=multi-user.target
EOF

```
10. Enable IP forwarding:
```bash
echo net.ipv4.ip_forward = 1 | ${SUDO} tee -a /etc/sysctl.conf
echo net.ipv6.ip_forward = 1 | ${SUDO} tee -a /etc/sysctl.conf
```
11. Start and enable the SoftEther VPN Server service:
```bash
systemctl enable vpnserver
systemctl start vpnserver
```
- If needed, you can stop or restart the SoftEther VPN Server service:
```bash
systemctl stop vpnserver
systemctl restart vpnserver
```
12. Check the status of the SoftEther VPN Server service:
```bash
systemctl status vpnserver
```
13. Configure the firewall rules:
```bash
sudo ufw allow 500,4500/udp
ufw allow 443
ufw allow 1701
ufw allow 1194
ufw allow 5555
```
14. Set up the static route push (replace with your desired configuration):
```bash
Static Route Push
Format: <VPC Network>/<VPC Netmask>/<VPN Gateway IP>
Example: 10.125.0.0/255.255.0.0/10.130.30.1
```
That's it! You have successfully installed SoftEther VPN on your Linux machine. Make sure to customize the configuration according to your needs.



