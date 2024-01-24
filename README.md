# Design-a-full-e2e-3-tier-web-architecture-using-terraform
Architecture with 3-tiers. With all resources provisioned. The website is tested for load balancing, and project outputs at the end on AWS  

teh goal of project is to create a fully functional 3-tier web server with capability of auto scaling and load balancing. Below archtecture diagram shows the required provisoned resources and entities. 

![image](https://github.com/fazalUllah-Khan/Design-a-full-e2e-3-tier-web-architecture-using-terraform/assets/148821704/749f4859-889e-4aee-823d-0ca65b33ebf4)

At the our Terraform code will create resources like VPC, Internet Gateway,Public and Private Subnets, Route Table and Route associations, Security Groups for Application Load Balancer, Application Tier, Presentation Tier and Database Tier,EC2 Instances, Auto Scaling Groups, Application Load Balancer, RDS Instance as will be verifieid in AWS acoount. 

First thing we will create an IAM user with attached policies in AWS and will install AWS plugins 

Below snaps shows created IAM used and adding plugins to VScode i.e tools we will use for terraform code, test and deploy,.

![image](https://github.com/fazalUllah-Khan/Design-a-full-e2e-3-tier-web-architecture-using-terraform/assets/148821704/ad7e7b26-25fa-446b-b940-9ea9c0dbaead)

![image](https://github.com/fazalUllah-Khan/Design-a-full-e2e-3-tier-web-architecture-using-terraform/assets/148821704/3f8c6095-843f-4de3-9a46-be38c0150682)

![image](https://github.com/fazalUllah-Khan/Design-a-full-e2e-3-tier-web-architecture-using-terraform/assets/148821704/64f95c38-44c0-4b02-bfe4-a4ac9bd77468)

Add teraform extensions to vscode
![image](https://github.com/fazalUllah-Khan/Design-a-full-e2e-3-tier-web-architecture-using-terraform/assets/148821704/618e5f11-dced-4a2a-93a6-d8c956782c5d)

Below are important terraform commands to create the infrastructure on our AWS account,. Which we will running to test our code and in the end to create final resources in project. 
* terraform init It initialize the working directory and downloads plugins of the provider
* terraform fmt It formats our code to look clean and standard.
* terraform plan It creates the execution plan of our code. Check each resource to confirm the exact resource is what you intend to provision. If there are errors, read out the error as terraform makes it easy to debug
* terraform apply It creates the actual infrastructure
* terraform destroy It destroys all the resources you provisioned on your AWS account

# Step 1:
 Create provider.tf file to interact with AWS for resources creatation, choose prefered region.
# Step 2: 
Create VPC by code my_vpc.tf file. Note, Make sure the enable_dns_hostnames attribute is set to true.
# Step 3: 
Create a internet_gateway.tf file to create the Internet Gateway. Attach the Internet Gateway to the VPC using the above VPC id.
# Step 4: 
Create a subnets.tf file for the Public and Private Subnets. Two public subnets are created with the attribute map_public_ip_on_launch set to true. The public IP will be needed to access the user data. Create a route_public.tf file for the public. Also created private (application layer & DB tier)subnet,. The private subnets for both application and database tier are set. Lets create a route table and route associations for these subnets. First, we need to create a NAT gateway for access to the internet
# Step 5: 
Create a Nat_gw.tf file and create a router_privt.tf file for the private subnets.
# Step 6: 
Create a file for Security Group for the Presentation Tier.i.e alb_sg.tf file. Note that ports 80 and 443 are opened for the inbound connection and all ports was opened for the outbound connection
# Step 7: 
Create a file for Security Group for the Application Tier,	i.e app_sg.tf file., Port 22 is opened for SSH access for incoming connections and the CIDR block should be your IP address.
# Step 8:
Create a file for Security Group for the Presentation Tier, i.e  web_sg.tf file. Ports 80, 443 and 22 were opened for the inbound connection and all ports were opened for outbound connection. The security groups for application load balancer and application tier was used as security groups for inbound connection.
# Step 9:
Create a file for Security Group for the Database Tier, i.e db_scg.tf file. Ports 3306 were opened for the inbound connection and all ports were opened for outbound connection. The security groups for presentation layer was used as a security group for inbound connection
# Step 10:
Create EC2 instances for Presentation and Application tier, i.e ec2_instance.tf file . Under the user_data attribute, a file is attached to the public instance for configuration. This will help check if the web server is functioning
# Step 11:
Create a file for Auto scaling groups, i.e auto-scaling-grp.tf file . The maximum capacity is set to 2 instances for presentation and application tier. You can adjust it according to your preference
# Step 12:
Create a file for Application Load Balancer, i.e LB.tf file. The load balancer is an application load balancer that listen to requests on port 80 and redirect traffic on port 443. The aws_lb_target_group_attachment resource attaches the public instance to the Target Group
# Step 13:
Create a file for the Database Instance, i.e rds_db.tf file and first, create a database subnet group . From this code, you could make changes to the engine_version attribute, the username attribute and the password attribute(it should be at least 8 in length). Also the multi_az attribute is set to true for high availability. Check the variable.tf below for reference
# Step 14:
Create a file for the my_outputs.tf, 1)	Create an output file for the Application Load Balancer 2)	The DNS of the application load balancer will be used to send requests to the public instance
# Step 15: 
Create variables.tf file and add all the variables values used from the all code blocks
# Step 16: 
Create a file for the user data, Create install-apache_webserver.sh file.	The code above will install apache webserver and a html file is added to the web page

Fially after terrform apply all the resources will be created you check code the Public IP address from your presentation tier and check if your code in install-apache_webserver.sh shows on your web browser




