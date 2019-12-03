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
<ul>
  <li>From aws console navigate to <i>Services</i> click <i>Network & Content Delivery</i> and then <i>VPC</i> <br>
    On the left pane, select <i>Your VPC</i> then <i>Create VPC</i><br></li>
<li>Give a suitable name to your VPC e.g. <b>'MY VPC'</b> and on the IPv4 CIDR block type: <b>192.168.0.0/16</b> and then create <br></li>
  <li>On the left pane, select <i>Subnets</i> and then <i>Create Subnets</i><br></li>
  <li>In the Name Tag field type: <b>My Public Subnet</b>, select your newly created VPC, choose an Availability Zone (AZ) and on the IPv4 CIDR block type: `<b>92.168.0.0/24</b> and then create <br></li>
  <li>Repeat the step above for the private subnet with Name Tag <b>My private Subnet</b> and IPv4 CIDR block <b192.168.1.0/24<b> in the same AZ.</li>
</ul>

## Create an Internet Gateway (IGW) and attach to the VPC
* From the left pane, select <i>Internet Gateways</i>
* Click <i>Create Internet Gateways</i>; give a suitable name to your IGW and then create
* Select your just created IGW and click  <i>Action</i> and <i>Attach</i> to your VPC
## Create Security and EC2 Instances
* From the Left pane, select <i>Security Groups</i> and  <i>Create Security group</i>
* Create <b>SG_NAT, SG_BB and SG_APP</b> for the NAT Instance, Bastion Box and Application Servers respectively.
* On the SG_BB, click on <i>Inbound Rules</i> and <i>Edit<i> and <i>Add Rule</i>:<br>
          `Rule: ssh; Port: 22 Source IP:  <your work station public ip> `
* On the SG_NAT click on <i>Inbound Rules</i> and <i>Edit<i> and <i>Add Rule</i>:<br>
          `Rule: ssh; Port: 22 Source IP:  SG_BB`
          `Rule: http; Port: 80 Source IP:  192.168.1.0/24 `
          `Rule: https; Port: 443 Source IP:  192.168.1.0/24 `
* On the SG_APP click on <i>Inbound Rules</i> and <i>Edit<i> and <i>Add Rule</i>:<br>
          `Rule: ssh; Port: 22 Source IP:  SG_BB`
<ul> <li>Navigate to <i>Services<i> and click <i>EC2<i><br></li>
        <ul>
          <li>Select <i>Instances</i> and click <i>Launch Instance<i>
            <li>Click on <i>Community AMIs<i> and search for <i>'ami-00a9d4a05375b2763'</i> in the search bar</li>
              <li>Select the AMI then Click Configure Instance Details </li>
              <li>Choose My VPC in the Network field</li>
              <li>Select My Public Subnet in the Subnet field</li>
              <li>Click Add Storage and optionally add tags for ease of referencing</li>
              <li>Click Configure Security Group, choose existing Security Group and select SG_NAT</li>
              <li>Review and Lauch</li> 
          </ul>
</ul>
  
