# 📦 Project Title: VPC Architecture with Bastion Hosts and NAT Gateway

## 📖 Overview
This project outlines the architecture of a Virtual Private Cloud (VPC) on AWS, including the use of Bastion Hosts, NAT Gateways, Auto Scaling Group for servers and more , in a secure and highly available environment. 

## 🌐 Architecture Diagram
<img width="1722" alt="Frame 1 (4)" src="https://github.com/user-attachments/assets/57f804bf-b407-4fce-b8fd-45ef1e4ad4ed">

## 🖥 Server Response
![image](https://github.com/user-attachments/assets/af890fa6-a748-4806-a1ff-5b29cc8ac3b2)



## 📋 Key Components
- **VPC**: A virtual network dedicated to your AWS account.
- **Public Subnets**: Contain resources that need to be accessible from the internet.
- **Private Subnets**: Contain resources that do not have direct internet access.
- **Bastion Hosts**: Serve as secure entry points for administrative access to the private subnets.
- **NAT Gateways**: Allow instances in private subnets to initiate outbound traffic to the internet while preventing inbound traffic.
- **Application Load Balancer**: Distributes incoming application traffic across multiple targets.
- **Auto Scaling Group**: Automatically adjusts the number of instances based on demand.

## 🏗️ Architecture Breakdown

### 1. **Bastion Hosts**
- **Purpose**: A VPC is a logically isolated network dedicated to your AWS account, allowing you to define your own network topology, including IP address range, subnets, route tables, and network gateways. 🌍 The VPC enhances security by providing private and public subnets, allowing resources to operate independently.

- **Placement**: Located in the public subnets of both Availability Zones (AZs) to ensure high availability.

### 2. **NAT Gateways**
- **Purpose**: NAT (Network Address Translation) Gateways enable instances in private subnets to initiate outbound traffic to the internet while preventing unsolicited inbound traffic. This is crucial for maintaining security while allowing updates and external API calls. 
- **Placement**: NAT Gateways are deployed in the public subnets. They facilitate outbound traffic from private subnets, ensuring that resources can remain secure yet functional. 📡

### 3. **Application Load Balancer**
- **Purpose**: The Application Load Balancer (ALB) distributes incoming application traffic across multiple targets (e.g., EC2 instances) in different subnets, enhancing fault tolerance and availability. It can intelligently route requests based on HTTP headers and paths, allowing for optimized performance. 
- **Benefit**: This reduces the load on individual instances and improves response times for end-users. ⚙️
- **Placement**: Application Load Balancer are deployed in the public subnets. They facilitate outbound/inbound traffic from subnets, ensuring that resources can remain secure yet functional. 📡

### 4. **Auto Scaling Group**
- **Purpose**: The Auto Scaling Group automatically adjusts the number of EC2 instances based on defined scaling policies. This ensures that the application can scale up during peak traffic and scale down during low usage periods, optimizing cost efficiency. 
- **Benefit**: Automatic scaling helps maintain performance and availability without manual intervention, allowing the application to meet varying demand seamlessly. 📈
- **Placement**: In Here Used In Private subnet servers to automatically scale with demand

## 🌍 Getting Started: AWS Resource Creation Guide

### Step 1: Log in to AWS Management Console
- Go to the [AWS Management Console](https://aws.amazon.com/console/) and log in with your credentials.

### Step 2: Create a VPC
1. In the **AWS Management Console**, search for **VPC** in the services search bar.
2. Click on **Your VPCs** in the left-hand menu.
3. Click on the **Create VPC** button.
4. Configure the VPC:
   - **Name tag**: `MyVPC`
   - **IPv4 CIDR block**: `10.0.0.0/16`
   - **Tenancy**: Default
5. Click **Create**.

### Step 3: Create Subnets
#### Create Public Subnets
1. In the VPC Dashboard, click on **Subnets** in the left-hand menu.
2. Click on the **Create Subnet** button.
3. Configure the public subnet:
   - **Name tag**: `PublicSubnet1`
   - **VPC**: Select `MyVPC`
   - **Availability Zone**: Select `ap-southeast-1a`
   - **IPv4 CIDR block**: `10.0.1.0/24`
4. Click **Create**.
5. Repeat the steps to create `PublicSubnet2` in **ap-southeast-1b** with CIDR `10.0.2.0/24`.

#### Create Private Subnets
1. Click on the **Create Subnet** button again.
2. Configure the private subnet:
   - **Name tag**: `PrivateSubnet1`
   - **VPC**: Select `MyVPC`
   - **Availability Zone**: Select `ap-southeast-1a`
   - **IPv4 CIDR block**: `10.0.3.0/24`
3. Click **Create**.
4. Repeat the steps to create `PrivateSubnet2` in **ap-southeast-1b** with CIDR `10.0.4.0/24`.

### Step 4: Create Internet Gateway
1. In the VPC Dashboard, click on **Internet Gateways**.
2. Click on the **Create Internet Gateway** button.
3. Configure the Internet Gateway:
   - **Name tag**: `MyInternetGateway`
4. Click **Create** and then **Attach to VPC**. Select `MyVPC`.

### Step 5: Configure Route Tables
#### Public Route Table
1. In the VPC Dashboard, click on **Route Tables**.
2. Select the **Route Table** associated with `MyVPC`.
3. Click on the **Routes** tab, then click **Edit routes**.
4. Click **Add route**:
   - **Destination**: `0.0.0.0/0`
   - **Target**: Select the `MyInternetGateway`
5. Click **Save routes**.
6. Click on the **Subnet Associations** tab, click **Edit subnet associations** and associate `PublicSubnet1` and `PublicSubnet2`.

#### Private Route Table
1. Create a new route table for the private subnets.
2. In the Route Tables page, click **Create route table**.
   - **Name tag**: `PrivateRouteTable`
   - **VPC**: Select `MyVPC`
3. Click **Create**.
4. Select `PrivateRouteTable`, and add a route to the NAT Gateway once it's created.

### Step 6: Create NAT Gateways
1. In the VPC Dashboard, click on **NAT Gateways**.
2. Click **Create NAT Gateway**.
3. Configure the NAT Gateway:
   - **Name tag**: `MyNATGateway1`
   - **Subnet**: Select `PublicSubnet1`
   - **Elastic IP**: Allocate a new Elastic IP.
4. Click **Create NAT Gateway**.
5. Repeat to create `MyNATGateway2` in `PublicSubnet2`.

### Step 7: Create Auto Scaling Group and Launch Configuration
1. In the AWS Management Console, go to the EC2 Dashboard.
2. Click on **Launch Configurations** and then **Create launch configuration**.
3. Choose an Amazon Machine Image (AMI), instance type, and configure security groups.
4. Click **Create Auto Scaling group** after creating the launch configuration and configure it to span both private subnets.

### Step 8: Set Up Application Load Balancer
1. In the EC2 Dashboard, click on **Load Balancers**.
2. Click on **Create Load Balancer** and select **Application Load Balancer**.
3. Configure the load balancer:
   - **Name**: `MyApplicationLoadBalancer`
   - **Scheme**: Internet-facing
   - **Listeners**: HTTP (port 80)
4. Select the public subnets and click **Next** to complete the setup.

## 🌊 How Request/Response Works

1. **Client Request**:
   - A user sends a request to the **Application Load Balancer** via the internet. 🌍

2. **Load Balancer**:
   - The Load Balancer routes the request to one of the servers in the **Auto Scaling Group** based on the configured algorithms. ⚙️

3. **Server Handling**:
   - The selected server processes the request. If it needs to access the internet (for example, to fetch data), it sends the request to the **NAT Gateway**. 📬

4. **NAT Gateway**:
   - The NAT Gateway translates the private IP of the server to a public IP and forwards the request to the internet. 🌐

5. **Response Handling**:
   - The response from the internet goes back through the NAT Gateway to the server, which then sends the final response back to the Load Balancer, and subsequently back to the client. 🎉

## 🔒 Security Considerations

### 1. **Security Groups**
- **Definition**: Security Groups act as virtual firewalls for your EC2 instances to control inbound and outbound traffic. Each instance can be associated with multiple security groups.
- **Configuration**:
  - **Bastion Host**: Only allow inbound SSH (port 22) or RDP (port 3389) traffic from trusted IP addresses (e.g., your office IP, VPN).
  - **Private Instances**: Allow inbound traffic from the ALB on necessary ports (e.g., HTTP 80, HTTPS 443) and outbound traffic as needed for internal communication and NAT Gateway access. 🚀

### 2. **Network Access Control Lists (NACLs)**
- **Definition**: NACLs provide an additional layer of security at the subnet level. They control traffic in and out of subnets and are stateless, meaning rules must be defined for both inbound and outbound traffic.
- **Configuration**:
  - **Public Subnets**: NACLs should allow inbound traffic from the internet (e.g., HTTP, HTTPS) and allow outbound responses. This is essential for the Bastion Host and NAT Gateway to function correctly.
  - **Private Subnets**: NACLs should allow inbound traffic from the ALB and outbound traffic to the NAT Gateway. They can be configured to deny all other traffic for enhanced security. 🔐


## 🌍 Conclusion
This architecture provides a scalable, secure, and highly available solution for deploying applications in AWS. Utilizing Bastion Hosts and NAT Gateways ensures that private instances remain secure while still having the necessary access to the internet for updates and API calls.