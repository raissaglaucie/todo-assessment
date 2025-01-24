# **Project: Flask Todo App Deployment using Terraform and Ansible**

## **Prerequisites**

To successfully deploy the Flask Todo App, the following prerequisites were met:

- **AWS Account**: Used for provisioning EC2 instances and security groups.
- **Terraform**: Installed locally for infrastructure provisioning.
- **Ansible**: Installed locally for configuration management and application deployment.
- **SSH Key Pair**: Required for secure access to the EC2 instance.

---

## **Project Structure**

The project is organized as follows:

```plaintext
.
├── terraform/
│   └── main.tf
├── ansible/
│   ├── inventory.ini
│   └── setup_flask_app.yml
└── README.md

```

# Deployment Steps

## Step 1: Infrastructure Provisioning

Navigate to the Terraform directory:
```plaintext
cd terraform
```
Initialize Terraform:
```plaintext
terraform init
```
Review the plan and apply the configuration:
```plaintext
terraform plan
terraform apply
```

# Terraform provisions:
An -**EC2** instance for hosting the Flask application.
A -**security** group to allow traffic on ports 22 (SSH), 5000 (Flask app), and optionally 80 (HTTP).

## Step 2: Configure Ansible Control Node

Update inventory.ini with the public IP of the EC2 instance (output from Terraform):
```plaintext
[webserver]
<EC2_PUBLIC_IP> ansible_user=ubuntu ansible_private_key_file=~/.ssh/raissa-east1.pem ansible_python_interpreter=/usr/bin/python3
```
## Step 3: Deploy the Application
Navigate to the Ansible directory:
```plaintext
cd ../ansible
```
Run the Ansible playbook to configure the EC2 instance and deploy the Flask application:
```plaintext
ansible-playbook -i inventory.ini setup_flask_app.yml
```

The playbook installs the necessary dependencies (Python, Flask, MySQL, etc.).
Configures the Flask app to connect to MySQL.
Sets up the Flask application as a systemd service (todoapp.service).

# Verification Steps
## SSH into the EC2 Instance:

```plaintext
ssh -i ~/.ssh/raissa-east1.pem ubuntu@<EC2_PUBLIC_IP>
```
## Verify Services:
1 Check MySQL service:
```plaintext
sudo systemctl status mysql
2 Check Flask application service:
```plaintext
sudo systemctl status todoapp
```
3 Check Application Logs:
```plaintext
sudo journalctl -u todoapp
```
4 Access the Application:
-**Open a browser and go to:**
```plaintext
http://<EC2_PUBLIC_IP>:5000
```
Verify that the to-do list functionality (adding, editing, deleting tasks) works.

# Key Features
## Infrastructure as Code:
Used Terraform to provision EC2 instance and security group.
Outputs public IP for easy integration with Ansible.
## Configuration Management:
Used Ansible to automate installation of dependencies, MySQL setup, and Flask deployment.
Flask app is managed as a systemd service for reliability.
## Database Integration:
MySQL database set up on the EC2 instance.
Flask app connects to MySQL to manage tasks.
