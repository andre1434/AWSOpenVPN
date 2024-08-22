==Under Construction==
<h1>Setup VPN server in the AWS Cloud and connect to a Private Subnet</h1>

This repository contains a walkthrough in setting up a VPN server in the cloud and configured to be able to connect to a private subnet.

Resources used:
- VPC
- EC2
- OpenVPN

Region: us-west-2 (Oregon)

<h1>Objectives</h1>
The project aims to:
<ul>
  <li>Setup VPC with at least 1 private subnet</li>
  <li>Create an EC2 instance with OpenVPN AMI</li>
  <li>Configure VPN Server</li>
  <li>Set up OpenVPN Connect</li>
  <li>Update Private Subnet to allow ICMP traffic coming from VPN server</li>
  <lI>Test connection to a Private Subnet using ping</lI>
</ul>
  <img src="AWS VPN/VPN architechture.jpg" alt="OpenVPN">

<h2>Stage 1: Setup VPC with at least 1 Private Subnet</h2>
VPC setup can be checked <a href="https://github.com/andre1434/aws-vpc/blob/c7e4d69d960a84c1c36a371730ebfe121c1cbab4/README.md">here</a>.<br><br>
  <img src="AWS VPN/VPC VPN Architecture.jpg" alt="VPC VPN Architecture">

  
<h2>Stage 2: Create an EC2 instance with OpenVPN AMI</h2>
Start EC2 instance. Select OpenVPN Server AMI through "Browser more AMIs", AWS Marketplace AMIs, then select OpenVPN Access Server.<br><br>
  <img src="AWS VPN/EC2 Settings.jpg" alt="OpenVPN Settings">
  <br>Create new key pair if not currently available. This will download a .PEM file<br><br>
  <img src="AWS VPN/EC2.jpg" alt="OpenVPN">

<h2>Stage 3: Configure VPN Server</h2>
Connect to VPN Server. There are a few options to connect to the instance, for this project, I am connecting via SSH client.

  <img src="AWS VPN/OpenVPN SSH client.jpg" alt="SSH Connection for Configuration">
  Open powershell. Directory should be where the private key (.PEM) file is located.<br>
  Run Command <b> chmod 400 "VPNServer.pem"</b><br>
  
  SSH sample command can be used: <b>ssh -i "VPNServer.pem" root@ec2-35-90-14-50.us-west-2.compute.amazonaws.com</b>
  Edit the SSH command to <b>ssh -i "VPNServer.pem" openvpnas@ec2-35-90-14-50.us-west-2.compute.amazonaws.com</b> to login as openvpnas.<br>
  Configure as needed. Setup password of openvpn user.
  <img src="AWS VPN/login as openvpnas.jpg" alt="SSH Connection for Configuration login as openvpnas">

  Admin page can be accessed through WebUI. Link to Admin WebUI page can be checked after initial configuration.<br>
  Credentials to be used as per initial configuration.
  <img src="AWS VPN/admin login page.jpg" alt="OpenVPN Admin Page">

Edit VPN settings. Under Routing, enable "Should client internet traffic be routed through the VPN?"<br>
Save Settings. Then Update Running Server to update.
  <img src="AWS VPN/vpn settings.jpg" alt="OpenVPN Admin Settings Page">
  
<h2>Stage 4: Set up OpenVPN Connect</h2>
Link to the OpenVPN client page can be checked after initial configuration. Credentials of user open<br>
Download the client based on the Operating System. Install.
  <img src="AWS VPN/client page.jpg" alt="OpenVPN Client Page">
  <br><br>
  Once installed, import the profile if needed.<br>
  Test if VPN can connect.
  <img src="AWS VPN/connect openvpn.jpg" alt="VPN Connection">
  Test if VPN works. This can be done by searching <b>What is my IP address</b> on Google.
  <img src="AWS VPN/whatismyip.jpg" alt="What is my IP Address">

<h2>Stage 5: Update Private Subnet to allow ICMP traffic coming from VPN server</h2>
For the purposes of this project, I only allowed ICMP traffic to check if I am able to ping my private server when I'm connected to the VPN.
<br>Edit Private Security group. Allow ICMP traffic with source VPN.
<img src="AWS VPN/edit private subnet sec group inbound rule.jpg" alt="Security Group inbound rule">

<h2>Stage 6: Test connection to a Private Subnet using ping</h2>
I used ping to test if client is able to communicate with the Private Subnet.<br>
<b>Without VPN Connection</b>
<img src="AWS VPN/test without connection.jpg" alt="Test without VPN">
<b>With VPN Connection</b>
<img src="AWS VPN/test with connection.jpg" alt="Test without VPN">
