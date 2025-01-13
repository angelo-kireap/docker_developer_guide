# Docker Installation on Ubuntu 22.04 

 

This guide provides step-by-step instructions for installing Docker on Ubuntu 22.04, addressing common issues such as permission errors. 

 

--- 

 

## Prerequisites 

- A system running Ubuntu 22.04. 

- A user account with `sudo` privileges. 

 

--- 

 

## Steps to Install Docker 

 

### 1. Update System Packages 

Ensure your system is up to date: 

```bash 

sudo apt update 

sudo apt upgrade -y 

``` 

 

### 2. Install Required Dependencies 

Install essential packages: 

```bash 

sudo apt install -y ca-certificates curl gnupg lsb-release 

``` 

 

### 3. Add Docker's Official GPG Key 

Add Docker's GPG key to verify packages: 

```bash 

sudo mkdir -p /etc/apt/keyrings 

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg 

``` 

 

### 4. Set Up Docker Repository 

Add Docker's stable repository: 

```bash 

echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null 

``` 

 

### 5. Install Docker 

Install Docker Engine and related components: 

```bash 

sudo apt update 

sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin 
sudo apt install -y docker.io
``` 

 


### 6. Verify Installation 

Check if Docker is installed: 

```bash 

sudo docker --version 

``` 

 

### 7. Enable Docker Service 

Start and enable Docker to run at boot: 

```bash 

sudo systemctl start docker 

sudo systemctl enable docker 

``` 

 

--- 

 

## Testing Docker 

 

### Run a Test Container 

Test Docker by running the `hello-world` container: 

```bash 

docker run hello-world 

``` 

 

### Common Error: Permission Denied 

You may encounter the following error: 

 

``` 

docker: permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Post "http://%2Fvar%2Frun%2Fdocker.sock/v1.24/containers/create": dial unix /var/run/docker.sock: connect: permission denied. 

See 'docker run --help'. 

``` 

 

--- 

 

## Resolving Permission Issues 

 

### 1. Add Your User to the Docker Group 

Allow your user to access Docker without `sudo`: 

```bash 

sudo usermod -aG docker $USER 

``` 

 

### 2. Apply Group Changes 

For the changes to take effect, either: 

- Log out and log back in, or 

- Apply the changes immediately: 

  ```bash 

  newgrp docker 

  ``` 

 

### 3. Verify Group Membership 

Check if your user is part of the `docker` group: 

```bash 

groups $USER 

``` 

You should see `docker` listed. 

 

### 4. Retry the Test 

Run the test container again: 

```bash 

docker run hello-world 

``` 

 

--- 

 

## Additional Troubleshooting 

 

### Check Docker Socket Permissions 

Ensure the Docker socket has the correct group ownership: 

```bash 

ls -l /var/run/docker.sock 

``` 

Expected output: 

``` 

srw-rw---- 1 root docker ... 

``` 

If it doesn’t show `docker`, restart the Docker service: 

```bash 

sudo systemctl restart docker 

``` 

 

### Reboot System 

If the issue persists, reboot your system: 

```bash 

sudo reboot 

``` 

 

--- 

 

## Conclusion 

Docker should now be successfully installed and functional on Ubuntu 22.04. You can run Docker commands without requiring `sudo`. If you encounter further issues, ensure all steps have been followed correctly. Happy containerizing! 🚀 