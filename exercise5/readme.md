# OpenVPN

Let's set up an OpenVPN server!

Launch a new EC2 instance, make sure it:

- Uses the `Ubuntu Server 16.04 LTS` AMI
- Has a public IP address
- Has a security group with ports for SSH and HTTPS open to the world

SSH to the box

`ssh -i <path-to-pem file> ubuntu@<ip-address>`

Make a directory to house the vpn files

`mkdir vpn && cd vpn`

Run a script to automatically install an OpenVPN server

`wget https://git.io/vpn1604 -O openvpn-install.sh`

Make the script executable and run it

`chmod +x ./openvpn-install.sh && sudo ./openvpn-install.sh`

In the installer:

- Set the IP address to the public IP address
- Set the protocol to TCP
- Select whichever DNS you prefer
- Leave the client name as `client`

Now let's download the `.ovpn` file to our local machine

`scp -i <path-to-pem-file> ubuntu@<ip-address>:./vpn/client.ovpn ./client.ovpn`

Now if you install an OpenVPN client for your OS, you should be able to use that `.ovpn` file to connect to the OpenVPN server!
