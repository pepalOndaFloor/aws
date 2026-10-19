# Assignment 6: Create a Cloud Network with Amazon VPC and Subnets

## Objective
Create an Amazon VPC with two subnets, an Internet Gateway, and a route table. The source practical names the subnets `Public-Subnet` and `Private-Subnet`.

## Requirements
- AWS account and access to the AWS Management Console
- A plan for the VPC CIDR block and non-overlapping subnet CIDR blocks

## Instructions

### 1. Open Amazon VPC
1. Sign in to the AWS Management Console.
2. Search for **VPC** and open the VPC Dashboard.
3. Use one AWS Region consistently for all resources.

### 2. Create a VPC
1. Select **Your VPCs → Create VPC**.
2. Choose **VPC only**.
3. Enter a name such as `FirstVPC`.
4. Provide a valid IPv4 CIDR block, for example `10.0.0.0/16`, if it does not overlap with another network you need to connect.
5. Create the VPC.

### 3. Create the first subnet
1. Open **Subnets → Create subnet**.
2. Select `FirstVPC`.
3. Name it `Public-Subnet`.
4. Choose an Availability Zone and enter a subnet CIDR contained within the VPC CIDR, for example `10.0.1.0/24`.
5. Create the subnet.

### 4. Create the second subnet
1. Select **Create subnet** again.
2. Choose the same VPC.
3. Name it `Private-Subnet`.
4. Choose an Availability Zone and a different, non-overlapping CIDR, for example `10.0.2.0/24`.
5. Create the subnet.

> The example CIDRs are suggestions; ensure they fit your VPC and do not overlap with existing networks.

### 5. Create and attach an Internet Gateway
1. Open **Internet Gateways → Create internet gateway**.
2. Name it `MyInternetGateway`.
3. Create the gateway.
4. Select it, then choose **Actions → Attach to VPC**.
5. Select `FirstVPC` and attach it.

### 6. Create a route table
1. Open **Route Tables → Create route table**.
2. Name it `Public-Route-Table`.
3. Select `FirstVPC`.
4. Create the route table.

### 7. Add the Internet route
1. Select `Public-Route-Table` and open **Routes**.
2. Choose **Edit routes → Add route**.
3. Set **Destination** to `0.0.0.0/0`.
4. Set **Target** to `MyInternetGateway`.
5. Save the routes.

### 8. Associate the route table with the public subnet
1. Open the route table's **Subnet associations** tab.
2. Select **Edit subnet associations**.
3. Select `Public-Subnet`.
4. Save the associations.

## Important note about the private subnet
The source practical creates `Private-Subnet` but associates the Internet Gateway route table only with `Public-Subnet`. Keep the private subnet on a route table without a direct route to the Internet Gateway. A private subnet does not automatically gain internet access; outbound internet access, if required, needs additional configuration such as a NAT Gateway and suitable routes.

## If the assignment requires virtual machines
The supplied practical's written steps do not specify how to launch VMs/EC2 instances. To place instances in different subnets, launch one EC2 instance with `Public-Subnet` selected and another with `Private-Subnet` selected, then configure their security groups and network access according to the required test. An instance in a public subnet also needs an appropriate public IP configuration and security-group rules to be reachable from the internet.

## Expected result
The VPC contains two subnets, an attached Internet Gateway, and a route table with an internet route associated with `Public-Subnet`.

## Verification checklist
- [ ] VPC `FirstVPC` exists.
- [ ] `Public-Subnet` and `Private-Subnet` exist in that VPC.
- [ ] `MyInternetGateway` is attached to the VPC.
- [ ] `Public-Route-Table` has route `0.0.0.0/0` to the Internet Gateway.
- [ ] `Public-Route-Table` is associated with `Public-Subnet`.
