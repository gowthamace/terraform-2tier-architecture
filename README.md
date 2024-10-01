
# Terraform AWS Infrastructure

This project contains Terraform modules to set up a scalable infrastructure on AWS, including an Auto Scaling Group (ASG), Elastic Load Balancer (ELB), Virtual Private Cloud (VPC), and EC2 instances. The infrastructure is deployed in the `ap-south-1` (Mumbai) region.

## Modules

The project is organized into multiple Terraform modules:

1. **Auto Scaling Group (ASG)**: Automatically scales EC2 instances based on the configured health checks.
2. **Elastic Load Balancer (ELB)**: Distributes traffic across multiple EC2 instances and performs health checks.
3. **VPC and Networking**: Creates a custom VPC, subnets, route tables, internet gateways, and NAT gateways.
4. **EC2 Instances**: Provisions individual EC2 servers based on the configuration provided.

## Prerequisites

- Terraform version `>= 1.6.6`
- AWS account with appropriate permissions
- AWS CLI configured with credentials

## Architecture

The setup consists of:
- A **VPC** with both public and private subnets.
- An **Internet Gateway** attached to the VPC.
- A **Security Group** allowing ingress and egress traffic as per the rules defined.
- **EC2 instances** behind an **Elastic Load Balancer** (ELB), which distributes traffic.
- An **Auto Scaling Group** (ASG) to manage instance scaling.
- A **NAT Gateway** to allow private instances access to the internet.

## Usage

### 1. Clone the Repository

```bash
git clone https://github.com/gowthamace/terraform-2tier-architecture.git
cd terraform-aws-infrastructure
```

### 2. Initialize Terraform

Before applying the infrastructure, initialize the Terraform project to download the required providers and modules:

```bash
terraform init
```

### 3. Modify Variables

You can modify the following variables in the `*.tf` files to customize the setup:

- **VPC CIDR block**: Default is `10.0.0.0/16`.
- **Public and Private subnets**.
- **EC2 instance types**.
- **AMI ID** for EC2 instances.

### 4. Plan and Apply

Generate an execution plan to review the changes Terraform will make:

```bash
terraform plan
```

If the plan looks good, apply the configuration:

```bash
terraform apply
```

### 5. Outputs

After applying, Terraform will output useful information like the ELB's DNS name, which you can use to access your application.

### 6. Destroy the Infrastructure

When you are done with the infrastructure, you can destroy it using:

```bash
terraform destroy
```

## Modules Overview

### Auto Scaling Group (`autoscaling`)

This module creates an ASG for managing the lifecycle of EC2 instances, using a launch template. The instances use a user data script to start a simple HTTP server.

**Key Parameters**:
- `min_size`: Minimum number of instances in the group.
- `max_size`: Maximum number of instances.
- `desired_capacity`: Number of instances to maintain at any given time.
- `instance_type`: EC2 instance type (e.g., `t3.micro`).
- `image_id`: AMI ID to use for launching instances.
- `security_groups`: Security group for the instances.

### Elastic Load Balancer (`elb`)

This module provisions an ELB that balances traffic to instances in the ASG. It also performs health checks to ensure the instances are responding to requests.

**Key Parameters**:
- `vpc_id`: The VPC to associate the ELB with.
- `subnets`: List of subnets where the ELB should be deployed.
- `listener`: List of listener configurations defining how traffic is handled.

### VPC and Networking (`network`)

This module creates a VPC along with public and private subnets, security groups, route tables, and NAT gateways.

**Key Parameters**:
- `vpc_cidr`: CIDR block for the VPC.
- `public_subnets`: List of public subnets to be created.
- `private_subnets`: List of private subnets to be created.
- `enable_nat_gateway`: Whether or not to create a NAT Gateway for private subnets.

### EC2 Instances (`instance`)

This module provisions EC2 instances based on a given map of instance types and names.

**Key Parameters**:
- `amiid`: AMI ID to use for the instances.
- `type`: EC2 instance type (e.g., `t2.micro`).
- `pemfile`: Name of the key pair to use for SSH access.

## File Structure

```
├── demo
│   ├── main.tf.asg           # Auto Scaling Group setup
│   ├── main.tf.elb           # ELB setup
│   ├── main.tf.full          # Full setup with ASG, ELB, and networking
│   ├── main.tf.instance      # EC2 instance provisioning
│   └── main.tf.network       # VPC and networking setup
├── instances
│   ├── main.tf               # EC2 instances module
│   └── variables.tf          # Variables for instances module
├── loadbalancer
│   ├── main.tf               # Load balancer module
│   ├── outputs.tf            # Outputs from the ELB module
│   └── variables.tf          # Variables for the ELB module
├── network
│   ├── main.tf               # VPC and networking module
│   ├── variables.tf          # Variables for the VPC and networking module
│   └── version.tf            # Terraform version configuration
└── README.md                 # This file
```

## Contact

For any questions or issues, reach out to:

**GOWTHAM**
- Email: 20gowthams@gmail.com  
- Phone: +91-8667087102

## License

MIT License
-----------
Copyright (c) [2024] [Gowtham]

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

1. The above copyright notice and this permission notice shall be included in
   all copies or substantial portions of the Software.

2. THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
   IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
   FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
   AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
   LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
   OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
   SOFTWARE.
