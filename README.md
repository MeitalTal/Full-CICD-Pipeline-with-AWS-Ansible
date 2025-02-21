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
