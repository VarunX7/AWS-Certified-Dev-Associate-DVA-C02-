# VPC : Virtual Private Cloud

* A private network within the AWS cloud
* VPC is a regional resource. If you have resources in 2 different regions they will have 2 different VPCs
* Subnets: Allow you to partition your network inside your VPC
    * Availability Zone resource
    * Public subnet: Accessible from the internet
    * Private subnet: Not accessible from the internet
* To define access to internet and the subnets we use Route Tables
* CIDR range: IP ranges allowed in your VPC
* Internet Gateway: Helps the VPC to access the internet
    * Public subnets have a direct route to the Internet Gateway
    * Operate at VPC level
* NAT Gateway and Nat Instances: Allow the instances in private subnets to connect to the internet while remaining private
    * Nat Gateway is AWS managed
    * NAT instances are self managed 
    * A NAT gateway or instance is deployed in the public subnet. The private subnet has direct route to it. The NAT gateway can connect to the internet using the Internet Gateway of the public subnet.

### Network ACL(NACL)
* A firewall which controls traffic from and to a subnet
* Can have allow and deny rules
* Are attached at the subnet level
* Rules only include IP addresses
* First line of defence (comes before security groups)

### Flow Logs
* Capture info about the IP traffic going into your interfaces:
    * VPC flow logs
    * Subnet flow logs
    * Elastic Network and Interface flow logs
* Helps to monitor and troubleshoot connectivity issues. eg-
    * Subnets to internet
    * Subnet to subnet
    * internet to subnets
* Capture the network info from AWS managed interfaces too: ELB, Elasticache, RDS, Aurora, etc.
* VPC flow logs data can go to S3, CloudWatch logs and Kinesis Data Firehose

### Summary
* VPC: Virtual Private Cloud default for each region
* Subnet: Tied to an AZ, network partion of a VPC
* Internet Gateway: Provide internet access. Operates at VPC level
* NAT Gateway/ Instances: Provide internet access from private subnet
* NACL: Network ACL. Defines rules for inbound and outbound traffic to subnet
* VPC peering: Connect 2 VPCs with non-overlapping IP ranges. Non transitive
* VPC Endpoint: Provide private access to AWS services within VPC
* Flow Logs: Network traffic logs
* Site to Site VPN: VPN over public network between on-premise Data Center and AWS
* Direct Connect: Direct private connection to AWS