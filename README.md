# NAT Instance, Internal Application Server and Bastion Box
Guide on creating an Internal Application Server that could reach the internet via a NAT Instance and allowed to communicate a Bastion Box via ssh on port 22
## <u>Architecture Diagram</u>
Below is the  is the Network Architectural diagram:
![Fig 1](https://github.com/otayoa/AWS_Internal_Application_Server/blob/master/Secured_Application_Server.png) 

## Preamble:
The design implementation is to enable us securely deployed our Application layer securely in our private cloud space in AWS, without undue exposures of the application layer to the public internet.
### Resources:
* Virtual Private Cloud (VPC) with an attached Internet GateWay (IGW)
* One public and private Subnet
* Two Elastic IPs
* Three EC2 Instances (NAT Instance, Application Server, and The Bastion Box)
* Security Groups for the NAT Instance, Application Server and Bastion Box)
## Create VPC with  a public and private subnet.
From aws console navigate to `Services` click `Network & Content Delivery` and then `VPC` <br>
On the left pane, select `Your VPC` then `Create VPC`<br>
Give a suitable name to your VPC e.g. 'MY VPC' and on the IPv4 CIDR block type: `192.168.0.0/16` and then create <br>
On the left pane, select `Subnets` and then `Create Subnets`<br>
In the Name Tag field type: `My Public Subnet`, select your newly created VPC, choose an Availability Zone (AZ) and on the IPv4 CIDR block type: `192.168.0.0/24` and then create <br>
Repeat the step above for the private subnet with Name Tag `My private Subnet` and IPv4 CIDR block `192.168.1.0/24` in the same AZ.
## Create an Internet Gateway and attach to the VPC
