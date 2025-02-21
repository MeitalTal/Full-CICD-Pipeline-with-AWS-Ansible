# Full CI/CD Pipeline with AWS & Ansible
A pipeline using Ansible, Docker, and Jenkins to automate AWS infrastructure deployment. It provisions a test EC2 instance, configures the environment, deploys an app, and verifies functionality. If successful, changes are applied to production via CI/CD. 

# Overview
1. Jenkins Pipeline triggers an Ansible playbook that provisions a testing EC2 instance.
2. The application is built into a Docker container and tested.
3. If tests pass, the application is deployed to production (a separate EC2 instance).
4. Once deployment is complete, the testing environment is terminated automatically.

# Jenkins Pipeline Stages
1. Build:
   - Runs an Ansible playbook to create a temporary EC2 instance for testing.
   - Installs dependencies inside the instance.

2. Test:
   - Verifies that the application responds correctly.

3. Deploy:
   - If tests pass, the app is deployed to a production EC2 instance.
   - Runs the Flask app inside a Docker container.

4. Cleanup:
   - Destroys the testing EC2 instance after deployment.
  
# Setup & Installation
### Prerequisites
- AWS account
- Jenkins installed & configured on a master node
- Production instance configured as a production node in jenkins and logged on to dockerhub
- Ansible installed on the Jenkins master
- Docker installed on all relevant servers
- AWS CLI configured

### Clone the Repository
```sh
git clone https://github.com/MeitalTal/Full-CICD-Pipeline-with-AWS-Ansible.git
cd Full-CICD-Pipeline-with-AWS-Ansible/ansible
mv ansible /home/ec2-user/ansible
```

### Configure Your Vault Secrets
Before running the pipeline, you must update the encrypted secrets stored in Ansible Vault.

1. Open the Vault file for editing:

```sh
ansible-vault create group_vars/all/pass.yml
```
2. Update the following values with your AWS credentials and any other required secrets:

```yaml
aws_access_key: "your-aws-access-key"
aws_secret_key: "your-aws-secret-key"
dockerhub_username: "your-dockerhub-username"
dockerhub_password: "your-dockerhub-password"
```
3. Save the file and re-encrypt if needed.
4. Update your vault password in ansible/vault_pass.txt file

### Configure Ansible playbook
###### Variables to Update:
Modify the following values in the vars section:

```yaml
vars:
    key_name: <Your-KeyPair-Name>         # Replace with your AWS KeyPair name
    region: <Your-AWS-Region>             # Example: us-east-1
    image: <Your-AMI-ID>                  # Find in EC2 > AMI
    id: "<Your-Instance-Name>"            # A descriptive name for your instance
    instance_type: <Your-Instance-Type>    # Example: t2.micro, check AWS pricing
    vpc_id: <Your-Subnet-ID>              # Find in EC2 > Subnets
    sec_group: <Your-Security-Group-ID>   # Find in EC2 > Security Groups
```

### Run the Jenkins Pipeline
Once everything is set up, trigger the Jenkins pipeline, and it will:
- Create a test environment
- Run tests on the application
- Deploy the application to production
- Destroy the test environment after deployment





