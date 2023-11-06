# README.md - Setting up Jenkins for Continuous Integration and Docker Deployment

This guide will walk you through the process of setting up a Jenkins server on an EC2 instance and configuring it to automatically build and deploy a Node.js application using Docker. This setup will help you achieve continuous integration and automated deployment for your project.

## Step 1: EC2 Instance Setup

- Launch an Amazon EC2 instance and SSH into the server.

## Step 2: Update Package Repositories

Run the following command to update the package repositories on your EC2 instance:

```bash
sudo apt update
```

## Step 3: Install Jenkins

Install Jenkins on your EC2 instance and start the service:

```bash
sudo apt install jenkins
sudo systemctl start jenkins
sudo systemctl status jenkins
```

## Step 4: Access Jenkins Web Interface

Now, open a web browser and navigate to `http://[public-IP]:8080/` to access the Jenkins web interface. You will be prompted to unlock Jenkins using the initial admin password located at `/var/lib/jenkins/secrets/initialAdminPassword`. Follow the on-screen instructions to complete the setup.

## Step 5: Install Docker

Install Docker on your EC2 instance with the following command:

```bash
sudo apt install docker.io
```

## Step 6: Create a Dockerfile

Create a Dockerfile for your Node.js application with the following content:

```Dockerfile
FROM node:12.2.0-alpine
WORKDIR /app
COPY . .
RUN npm install
EXPOSE 8000
CMD ["node", "app.js"]
```

This Dockerfile sets up a Node.js environment, copies your application code into the container, installs dependencies, exposes port 8000, and defines the command to start your application.

## Step 7: Jenkins Configuration

Now, configure Jenkins to automate the build and deployment process:

1. Create a new Jenkins job:
   - Click "New Item" in the Jenkins dashboard.
   - Add a description for the job.
   - Select "GitHub project" and add your project's URL.

2. Configure Source Code Management:
   - Select "Git" and provide the Repository URL and Credentials for your GitHub repository.
   - Choose the branches you want to build.

3. Build Triggers:
   - Select "GitHub hook trigger for GITScm polling." This will automatically trigger the build whenever there's a code change in your GitHub repository.

4. Execute Shell:
   - Add the following commands to build and run your Docker container in the Execute Shell section:

   ```bash
   docker build -t [image_name] .
   docker run -d --name [container_name] -p 8000:8000 [image_name]
   ```

   Replace `[image_name]` and `[container_name]` with your desired names.

## Step 8: Configure SSH and GPG Keys

Configure SSH keys and GPG keys in Jenkins as required for authentication and access to your GitHub repository.

Your Jenkins setup is now ready to automate your build and deployment process. When developers push code to the GitHub repository, Jenkins will automatically trigger the build, creating a Docker container with your Node.js application. If you encounter any errors, try to resolve them based on the information provided. Happy coding!
