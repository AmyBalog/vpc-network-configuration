# vpc-network-configuration
Setting the VPC network configuration to allow internet access to public and private subnets

Current configuration:
- A VPC contains two subnets, a public and private subnet. There is no Internet Gateway attached to the VPC. 
- A Web Server is deployed inside the public subnet. 
- A DB Server is deployed inside the private subnet that does not contain a route table. 

Goal:
- Enable internet access with proper security controls to allow intenet access while maintaining protection of the private subnet.
- Attach an Internet Gateway, update NACLs, Security Groups, and route tables.

Background Information:
- To enable internet, an Internet Gateway must be attached to the VPC. A route table needs to be included in the subnet and point the route table to the Internet Gateway.
- The instances must have either public IPv4 addresses, IPv6 addresses, or elastic IP address.
- NACLs and Security Groups should include rules to allow relevant traffic to flow to and from the instance.
- A NAT gateway is needed in the public subnet to enable instances from a private subnet to connect to the internet, while preventing the internet from initiating connections to the private instance. 

**Steps to configure the network for a VPC with a public and private subnet:**

Set the route table for the Web Server in the public subnet:
1. In the AWS console, search for EC2.
2. Click on EC2, then click on Instances in the right side column.
3. Click on the details of the Web Server instance.
4. Select the Networking tab
5. Click on the Subnet ID. It will open a new tabl with the subnet in that VPC.
6. Checkmark the box with the subnet and click on the Route table tab.
7. Click on the route table with the link of the name that contains the web server subnet.
8. Edit the route table and add route with Destination: 0.0.0.0/0 and Target: Internet Gateway.
The subnet is now reachable from the internet.

Add connection between the Web Server in the public subnet and the DB Server in the private subnet:
1. Go to the Web Server instance and click on the Security tab.
2. Under Security Groups, click WebServerSecurityGroup
3. Click Edit on the inbound rules tab and add rule.
4. Select type = HTTP, Source Type = Anywhere-IPv4, and save.
5. Go to the instances and select the DB Server.
6. Go to the Security tab and click on the link for the Security Groups.
7. Edit the inbound rules: Type = MySQL/Aurora, Source Type = Custom, Source = WebServerSecurityGroup. Then Save.
There is now connection between the two subnets.

Add a NAT Gateway to enable internet access for the private subnet:
1. Enable a NAT gateway in the public subnet. The NAT gateway should reside in the public subnet, not the private subnet.
2. Specify an elastic IP address to associate the NAT gateway.
3. Go to the route table in the private subnet to point internet traffic to the NAT gateway. 
