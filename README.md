Here's an enhanced version of your README that demonstrates your understanding and expertise. I’ve removed the “Additional Tasks” section and refined the steps for clarity and technical depth.

---

# CloudFormation Apache Web Server Project

## Project Overview
This project demonstrates an end-to-end infrastructure setup using AWS CloudFormation to deploy a simple yet functional Apache web server on an Amazon Linux EC2 instance. The solution is composed of two main CloudFormation stacks, each designed to modularly address network setup and application deployment.

### Stacks:
1. **Network Stack** (`SampleNetworkCrossStack`): Defines the foundational networking components, including a VPC, subnet, and security configurations to support secure and scalable application deployment.
2. **Apache Web Server Stack**: Leverages the resources from the network stack to deploy an EC2 instance configured with Apache, complete with public access and Elastic IP allocation.

## Prerequisites
- AWS CLI installed and configured with credentials that grant CloudFormation and EC2 permissions.
- An existing EC2 KeyPair in the specified AWS region for SSH access to the server.

## Deployment Steps

### Step 1: Deploy the Network Stack
This stack creates a robust network foundation for the application and includes:
- A VPC with DNS support and hostnames enabled to simplify EC2 DNS resolution.
- A public subnet within the VPC to host the publicly accessible EC2 instance.
- An Internet Gateway and appropriate route table for internet connectivity.
- A security group that restricts access to only necessary ports (HTTP port 80 in this case).

#### Command to Deploy the Network Stack:
```bash
aws cloudformation create-stack --stack-name SampleNetworkCrossStack --template-body file://network_template.yml
```

This step establishes the core network infrastructure required for hosting a publicly accessible web server.

### Step 2: Deploy the Apache Web Server Stack
Once the network stack is up, deploy the Apache Web Server Stack. This stack references resources created in the network stack to ensure the server is correctly integrated into the network.

**Key Configurations of this Stack**:
- **Elastic IP**: Assigns a fixed public IP to the instance for consistent, reliable access.
- **AMI Mapping**: Ensures compatibility across regions by dynamically selecting the correct Amazon Linux AMI ID based on the deployment region.
- **Parameters**: Accepts configurable parameters to customize the deployment environment.

#### Command to Deploy the Apache Web Server Stack:
```bash
aws cloudformation create-stack --stack-name ApacheWebServerStack --template-body file://apache_template.yml --parameters ParameterKey=KeyName,ParameterValue=<YourKeyName>
```

### Configurable Parameters
- **VpcId**: Specify an existing VPC ID, or use the VPC created by `SampleNetworkCrossStack`.
- **PublicSubnetId**: Public subnet ID within the VPC for the instance.
- **KeyName**: EC2 KeyPair name for SSH access.
- **InstanceType**: Choose the instance type for the server (default: `t2.micro`).
- **EnvironmentType**: Specify `Dev` or `Prod` to control environment-specific settings.

### Outputs
Upon successful deployment, the stack outputs essential information for accessing and managing the server:
- **InstanceID**: EC2 instance ID for tracking and management.
- **InstanceIpAddress**: Public IP of the web server (use this for accessing the server via a browser).
- **InstancePrivateIp**: Private IP for intra-VPC communications.
- **Ec2InstancePublicDnsName**: Public DNS name, which can be used to access the server.

## Verification and Access
1. **Web Access**: Open a browser and navigate to `http://<InstanceIpAddress>`. You should see the message "Hello World from user data," confirming Apache is installed and serving content.
   
2. **SSH Access**: Connect to the instance via SSH to verify configurations or make further adjustments:
   ```bash
   ssh -i <YourKeyFile.pem> ec2-user@<InstanceIpAddress>
   ```

This structure and configuration demonstrate a strong understanding of network setup, secure access controls, and automated deployment using AWS CloudFormation. The project can be scaled or adapted to accommodate additional infrastructure needs, such as load balancing or integration with a CI/CD pipeline. 