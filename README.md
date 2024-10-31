# CloudFormation Template for VPC, Subnet, Keypair, Security Group, Internet Gateway, Route Table and EC2 Instance

This CloudFormation template creates a basic networking setup in AWS, including a VPC, public subnet, security group, and an EC2 instance with SSH and HTTP access. The setup is suitable for applications or development environments needing a public-facing server.

## Resources Created

### 1. **VPC (Virtual Private Cloud)**
   - **Logical ID**: `MyVPC`
   - **CIDR Block**: `10.0.0.0/16`
   - **Tags**:
     - `Name`: `VPC-Diamond`
     - `Date`: `October 2024`

### 2. **Subnet**
   - **Logical ID**: `MySubnet`
   - **CIDR Block**: `10.0.1.0/24`
   - **Public IP Mapping**: Enabled for automatic assignment on instance launch
   - **Associated VPC**: `MyVPC`
   - **Tag**: `Subnet-Diamond`

### 3. **Security Group**
   - **Logical ID**: `MyEC2SecurityGroup`
   - **Description**: Allows inbound SSH (port 22) and HTTP (port 80) traffic from any IP
   - **Ingress Rules**:
     - **SSH Access**: TCP, Port 22, CIDR `0.0.0.0/0`
     - **HTTP Access**: TCP, Port 80, CIDR `0.0.0.0/0`

### 4. **EC2 Instance**
   - **Logical ID**: `MyEC2Instance`
   - **Instance Type**: `t2.micro`
   - **AMI ID**: `ami-0b8c6b923777519db` (NOTE: Adjust to your region’s AMI)
   - **Key Pair**: `dms-keypair` (Must be created in advance)
   - **Network Configuration**:
     - Associates with public IP
     - Uses `MyEC2SecurityGroup` for security
     - Placed in `MySubnet`

### 5. **key Pair**
   - **Logical ID**: `MyKeyPair`
   - **KeyName**: `dms-keypair` (In the AWS Console, go to **Parameter Store**, select the key pair, and click **Show decrypted value**. Copy the RSA prvate key, paste it into a Notepad file and save it as `dms-keypair.pem`.)

### 6. **Internet Gateway**
   - **Logical ID**: `InternetGateway`
   - **Attached to**: `MyVPC`

### 7. **VPC Gateway Attachment**
   - **Logical ID**: `IgAttachment`
   - **Purpose**: Attaches `InternetGateway` to `MyVPC`

### 8. **Route Table and Public Route**
   - **Route Table Logical ID**: `RouteTable`
   - **Associated VPC**: `MyVPC`
   - **Route Logical ID**: `PublicRoute`
     - **Destination CIDR**: `0.0.0.0/0`
     - **Gateway**: `InternetGateway`

### 9. **Subnet Route Table Association**
   - **Logical ID**: `PublicSubRouteTableAssociation`
   - **Purpose**: Associates `RouteTable` with `MySubnet` for internet access

## Prerequisites

. **AMI ID**: Update the `ImageId` in `MyEC2Instance` if needed to match a valid AMI ID for your preferred region.

## Deployment

To deploy this template:

1. Save this template.
2. Use the CloudFormation Console to create the stack:
   - **Console**: Go to the **CloudFormation** section, choose **Create Stack**, upload this file, and follow the prompts.

## Notes

- **Security Warning**: The security group allows unrestricted access to SSH and HTTP, which can be insecure. For production environments, restrict the CIDR range.
- **Internet Access**: This template sets up a public subnet with internet access, suitable for web servers or other public resources.

---

This was created by Diamond Simmons, and provides a comprehensive overview for deploying and understanding the components in this template. Feel free to download, modify and practice creating Infrastructure as Code (IaC)! Enjoy. 
