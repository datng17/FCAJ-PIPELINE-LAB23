# AWS CI/CD Pipeline: Code Deployment to EC2

This repository contains the configuration and scripts for automating software deployments to AWS EC2 instances using **AWS CodeDeploy**.

## Project Overview
The goal of this project is to establish a robust CI/CD pipeline that automatically deploys application updates from a source repository (like GitHub or AWS CodeCommit) to a fleet of Amazon EC2 instances.

## Infrastructure Setup
To prepare your **Amazon Linux** instance for CodeDeploy, you can use the following script as **User Data** during instance launch:

```bash
#!/bin/bash
# Update system and install dependencies
sudo yum update -y
sudo yum install ruby wget -y

# Download and install CodeDeploy agent
# Replace 'region-identifier' with your actual region (e.g., us-east-1)
cd /home/ec2-user
wget https://aws-codedeploy-us-east-1.s3.us-east-1.amazonaws.com/latest/install
chmod +x ./install
sudo ./install auto

# Ensure CodeDeploy agent is running
sudo service codedeploy-agent status

# Start the codedeploy-agent
systemctl start codedeploy-agent
```

## Repository Structure
- `appspec.yml`: Main configuration file for AWS CodeDeploy.
- `buildspec.yml`: Instructions for AWS CodeBuild (if applicable).
- `scripts/`: Deployment lifecycle hooks (start/stop/install).
- `index.html`: Sample application landing page.
- `.gitattributes`: Ensures Linux-compatible line endings for scripts.

## Deployment Lifecycle
AWS CodeDeploy uses the scripts in the `scripts/` directory during specific phases of the deployment:
1. **BeforeInstall**: `scripts/install_dependencies`
2. **AfterInstall**: Configuration and cleanup.
3. **ApplicationStart**: `scripts/start_server`
4. **ApplicationStop**: `scripts/stop_server`
