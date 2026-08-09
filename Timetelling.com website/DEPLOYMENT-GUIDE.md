To create the project I will be performing following activities.
	1. Configuring the network so, that the website can translate the host URL to IP
	2. Creating multiple EC2 instances to host the website
	3. ALB to balance the traffic route across multiple EC2 instance
	4. Multiple AZ, so that there is HA across the network
	5. ASG, to maintain the desired level of EC2 instance so that the traffic load is in well maintained range
	6. And cloud watch to monitor the traffic


**Creating VPC**     
It’s a basic project, we are keeping the cost low.

	1. Go to VPC --> Create VPC
	2. Select the below options:
	VPC and more
	Name tag auto generate: whatsthetime
	IPv4 CIDR block: 10.0.0.0/16
	No, IPv6 CIDR block
	Tenancy: Default
	Encryption setting: none
	Number of availability zones: 2  (use1-az1 (us-east-1a), use1-az2(us-eat-1b)) 
	Number of public subnets: 2
	Number of private subnets: 0
	Public subnet: 10.0.0.0/20     10.0.16.0/20
	NAT gateways: None
	VPC endpoints: None
	DNS options:
	Enable DNS hostname
	Enable DNS resolution
	
	Note: Since, I haven't enabled private subnetting for my EC2 instance so NAT is not required here
	3. Once, configuring the VPC, check if all the parameters are correct and click create VPC.
	4. Once the creation is completed you can check the VPC. Subnets and route table also show the associated entries
This completed configuring the network.


**Creating security groups for ALB:**

In order to allow specific traffic to reach the network, we will be configuring security group. Here, we will configure 2 security groups one for ALB and another for website. This will help to avoid traffic directly being sent to the EC2 instance. 
This allows anyone on the internet to access your website through the ALB.

1.1. Creating security group for ALB:

	1. Navigate to EC2 --> security group --> create security group
	2. Enter the below configuration:
	Name: ALB-wtt-sg
	Description: Security group for Application Load Balancer
	VPC: (Select the VPC created in previous step) whatsthetime-vpc
	Inbound rules: 
		Type: HTTP
		Protocol: TCP
		Port Range: 80
		Source: Anywhere (0.0.0.0/0) *Allowing everyone to send request to ALB
	Outbound rules:
		Type: All traffic
		Protocol: All
		Port Range: All
		Destination: Custom (0.0.0.0/0) *This is the traffic exiting out of ALB to EC2
	
	This completes creation of the first ALB now creating the next one for website
	

1.2. Creating security group for ALB:

	1. Navigate to EC2 --> security group --> create security group
	2. Enter the below configuration:

	Name: website-wtt-sg
	Description: Security Group for EC2 Instances
	VPC: (Select the VPC created in previous step) whatsthetime-vpc
	Inbound rules: 
		Type: HTTP
		Protocol: TCP
		Port Range: 80
		Source: Custom (ALB-wtt-sg) *Only allowing HTTP traffic from ALB
	Outbound rules:
		Type: All traffic
		Protocol: All
		Port Range: All
		Destination: Custom (0.0.0.0/0) *This is the traffic exiting out of  EC2
	

**Creating Launch Template for EC2:**

Since we will be creating multiple EC2 instance at once, it would be easier to create a template to for creating EC2 instances rather than configuring the same parameters again and again. 

	1. In EC2 --> Launch Templates --> Create Launch Templates
	2. Enter the following parameter:
	Launch Template name: mywhatsthetime-lt
	Description: Launch Template for What's The Time web application deployed through Auto Scaling Group
AMI: Amazon Linux 2023 kernel-6.18 AMI
Instance Type: t2.micro
	Key Pair: wtt-keypair
	Note create a new keypair
	Firewall: select existing security group
	Common security groups: website-wtt-sg
	Storage volume: EBS 8GB GP3 (default)
	Advanced details: user data
	#!/bin/bash
	
	dnf update -y
	dnf install nginx -y
	
	INSTANCE_ID=$(curl -s http://169.254.169.254/latest/meta-data/instance-id)
	
	cat <<EOF > /usr/share/nginx/html/index.html
	<!DOCTYPE html>
	<html>
	<head>
	<title>What's The Time?</title>
	<style>
	body {
	    font-family: Arial;
	    text-align: center;
	    margin-top: 100px;
	}
	</style>
	</head>
	
	<body>
	
	<h1>What's The Time?</h1>
	
	<h2 id="clock"></h2>
	
	<h3>Instance ID</h3>
	<p>$INSTANCE_ID</p>
	
	<script>
	function updateClock() {
	 document.getElementById("clock").innerHTML =
	 new Date().toLocaleString();
	}
	setInterval(updateClock,1000);
	updateClock();
	</script>
	
	</body>
	
	</html>
	EOF
	
	systemctl enable nginx
	systemctl start nginx
	
	
	
	
	Finally, click create template
	

**Creating Target group:**

By creating target group we inform the ALB to send traffic to this EC2 instances.


	1. In EC2 --> Target Groups --> Create
	2. Enter the following parameters:
	Target Type: Instance
	Target group name: whatsthetime-tg
	Protocol: HTTP
	Port: 80
	IP address type: IPv4
	VPC: whatsthetime-vpc
	Protocol version: HTTP1
	Health check: HTTP
	Health check path: /
	3. Click next and we skip the register target for now. The intention is to create an empty bucket for EC2.
	4. This completes creation of target group


**Creating Application Load Balancer:**

In order to distribute load among the users, need to configure ALB and for that we also created target group


	1. In EC2 --> Load Balancer --> Create Load Balancer
	2. Select ApplicationLoad Balancer by clicking on create.
	3. Enter the following parameters:
	Load Balancer name: whatsthetime-alb
	Scheme: Internet-facing
	Load balancerIP address type: IPv4
	VPC: whatsthetime-VPC
	Availability zones and subnet:
		Us-east-1a (10.0.0.0/20)
		Us-east-1b (10.0.16.0/20)
	Security group: ALB-wtt-sg
	Listeners and routing:
		Protocol: HTTP     Port: 80
	Default action: Forward to target group
	Target group: whatsthetime-tg
	
	4. Verify the configuration and create the ALB.



**Creating Auto Scaling Group:**

This stage is important in order to maintain the desired EC2 instance level and check the healthy intances

	1. In EC2 --> Auto Scaling Group --> create ASG
	2. Choose the launch template
	Name: whatsthetime-asg
	Launch template: mywhatsthetime-lt
	Version: 1
	3. Choose instance launch options
	VPC: whatsthetime-vpc
	AZ and subnets: us-east-1a, us-east-1b
	Availability zone distribution: Balanced best effort
	Additional capacity setting: Default
	4. Integrate with other services
	Select Load Balancing option: Attach to an existing load balancer
	Select load balancer to attach: Choose from your load balancer target groups
	Existing load balancer target group: whatsthetime-tg
	VPC Lattice integration options: No VPC Lettice service
	Health Checks: Enable ELB health checks
	Health check grace period: 300 seconds 

	1. Configure group size and scaling
	Group size:
		Desired capacity: 2
	Scaling: 
		Minimum desired capacity:2
		Maximum desired capacity: 4
		Automatic scaling: Target tracking scaling policy
		Metric type: Average CPU utilization
		Target value: 70
		Instance warmup: 300 seconds 
	2. Add notifications

**Verify Deployment**

Checks:
Target Group Healthy
ALB Active
EC2 Running
ASG Healthy
Get ALB DNS Name.

Open:
http://<ALB-DNS-NAME>
Show more lines

Expected Output:
Current Date
Current Time
EC2 Instance ID
