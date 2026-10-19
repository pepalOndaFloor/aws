# Assignment 8: Set Up an Application Load Balancer on AWS

## Objective
Create an Application Load Balancer (ALB), register two EC2 instances as targets, and test traffic through the load balancer's DNS name.

## Requirements
- AWS account and access to the AWS Management Console
- Two running EC2 instances in the same VPC
- A simple HTTP page on each instance so that Server 1 and Server 2 can be distinguished
- HTTP access configured in the relevant security groups

## Instructions

### 1. Check the EC2 instances
1. Sign in to AWS and select **Asia Pacific (Mumbai), `ap-south-1`**, as in the source practical.
2. Open **EC2 → Instances**.
3. Confirm that `AWS-LB-Server1` and `AWS-LB-Server2` are running.
4. Wait until both instances pass their status checks.
5. Open each instance's public IPv4 address in a browser and confirm that its HTTP page loads. The pages should identify Server 1 and Server 2 separately.

### 2. Create a target group
1. Open **EC2 → Target Groups → Create target group**.
2. Set **Target type** to `Instances`.
3. Set the target group name to `aws-alb-target-group`.
4. Set **Protocol** to `HTTP` and **Port** to `80`.
5. Select the same VPC as the EC2 instances.
6. Set the health check protocol to `HTTP` and path to `/`.
7. Continue to the target registration page.

### 3. Register both instances
1. Select `AWS-LB-Server1` and `AWS-LB-Server2`.
2. Choose **Include as pending below**.
3. Create the target group.
4. Open **Target Groups → aws-alb-target-group → Targets**.
5. Wait until both instances show **Healthy**.

### 4. Create an Application Load Balancer
1. Open **EC2 → Load Balancers → Create Load Balancer**.
2. Choose **Application Load Balancer → Create**.
3. Enter a name such as `my-application-load-balancer`.
4. Set the scheme to **Internet-facing** and the IP address type to **IPv4**.
5. Select the same VPC as the EC2 instances.
6. Select at least two Availability Zones and one suitable subnet in each selected zone.

### 5. Configure security groups
1. Assign a security group to the ALB that allows inbound **HTTP, port 80**, from the clients that need to access the practical.
2. Configure the EC2 instances' security group to allow inbound HTTP on port 80 **from the ALB's security group**.
3. Save the rules.

For a short classroom demonstration, broader HTTP access may be temporarily used if required by the lab, but restricting the EC2 rule to the ALB security group is preferable. Do not expose SSH/RDP broadly.

### 6. Configure the listener and routing
1. Under **Listeners and routing**, set the listener protocol to `HTTP`.
2. Set the listener port to `80`.
3. Set the default action to **Forward to**.
4. Select `aws-alb-target-group`.
5. Review the configuration and create the load balancer.

### 7. Test the load balancer
1. Wait for the load balancer state to become **Active**.
2. Open **EC2 → Load Balancers** and select your ALB.
3. Copy its **DNS name**.
4. Open `http://YOUR-ALB-DNS-NAME` in a browser, replacing the placeholder with the actual DNS name.
5. Refresh the page several times. Requests may show the page from Server 1 or Server 2, depending on load-balancer routing and connection behavior.

## Expected result
The ALB DNS name serves the application through the target group, and both registered EC2 instances are healthy.

## Troubleshooting
- **Target unhealthy:** Confirm the web server is running, the health-check path `/` returns a successful response, and security groups permit ALB-to-instance traffic on port 80.
- **ALB page does not load:** Check that the ALB is Active, the listener forwards to the correct target group, and the ALB security group permits inbound HTTP.
- **Only one server appears:** Repeated requests do not guarantee alternating responses. Check target health and ensure the two pages have different content.

## Cleanup and cost
Load balancers and EC2 instances can incur charges. When the practical is complete, delete resources you no longer need, following your course instructions.
