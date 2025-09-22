# Terraform AWS ECS Infrastructure

This Terraform configuration creates a complete AWS infrastructure for running a Node.js application using ECS Fargate with the following components:

## Architecture

- **VPC**: Using the official AWS VPC module with public and private subnets
- **ECS Cluster**: Using the official AWS ECS module with Fargate capacity providers
- **Application Load Balancer**: For distributing traffic to ECS tasks
- **ECS Service**: Running your Node.js application from ECR
- **Security Groups**: Properly configured for ALB and ECS communication
- **IAM Roles**: ECS execution and task roles with necessary permissions
- **CloudWatch Logs**: For application logging

## Prerequisites

1. AWS CLI configured with appropriate credentials
2. Terraform >= 1.0 installed
3. Your Node.js application image available in ECR public registry

## Usage

1. **Clone and configure**:
   ```bash
   cp terraform.tfvars.example terraform.tfvars
   ```

2. **Update terraform.tfvars**:
   - Set your ECR repository URL
   - Adjust region and other parameters as needed

3. **Initialize and apply**:
   ```bash
   terraform init
   terraform plan
   terraform apply
   ```

4. **Access your application**:
   After deployment, use the `application_url` output to access your Node.js app.

## Configuration

### Key Variables

- `ecr_repository_url`: Your ECR repository URL (e.g., `public.ecr.aws/your-registry/simple-nodejs-app:latest`)
- `aws_region`: AWS region for deployment
- `app_port`: Port your Node.js application listens on (default: 3000)
- `project_name`: Name prefix for all resources

### ECR Repository URL Format

For public ECR repositories:
```
public.ecr.aws/[registry-alias]/[repository-name]:[tag]
```

## Outputs

- `application_url`: URL to access your deployed application
- `load_balancer_dns_name`: ALB DNS name
- `ecs_cluster_name`: Name of the created ECS cluster
- `vpc_id`: ID of the created VPC

## Clean Up

```bash
terraform destroy
```

## Security Features

- ECS tasks run in private subnets
- Security groups restrict access appropriately
- IAM roles follow least privilege principle
- ALB provides SSL termination capability (can be extended)

## Scaling

The ECS service is configured with Fargate capacity providers including FARGATE_SPOT for cost optimization. You can adjust the capacity provider strategies in the ECS cluster module configuration.