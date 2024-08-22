# Jenkins CICD pipeline with Github integration for deploying Docker application on EC2 instance (Freestyle Project)

## Introduction 

This project outlines the steps to set up a CI/CD pipeline using Jenkins for a Node.js To-Do application. 

## Prerequisites

- **Server**: A EC2 instance with Ubuntu installed.
- **Docker**: Installed on the server.
- **Jenkins**:Installed and should be running.
- **Git**: Installed should be installed on the server.
- **Github repository**: consisting of your Node.js application code.

## Setting up the server

1. Choose an instance with sufficient resources. In this project t2.micro is used on AWS.
2. Select the key-value pair.

![Screenshot (101)](https://github.com/user-attachments/assets/e79c605b-88bf-451c-8fe6-c8250248e4dd)

3. Configure security groups to allow SSH(port 22).
4. Allow HTTP/HTTPS(port 80/ 443) access.

![Screenshot (102)](https://github.com/user-attachments/assets/9dbf5945-81ac-4ca1-ab71-96314182b9d7)

## Connecting to the EC2 instance

1. Once the instance is running, select it in the EC2 dashboard.
2. click on the "Connect" button to connect to the instance.

![105](https://github.com/user-attachments/assets/6b04aa75-b356-4a49-addc-6710cceee190)

![Screenshot (106)](https://github.com/user-attachments/assets/3e09ef0c-aaa6-4afb-b970-837a680fc979)

## Setting Up the Environment on the EC2 Instance

1. Update the package manager
```
sudo apt update
```

2. Install java development kit(JDK-11) so as to run jenkins.
```
sudo apt install openjdk-11-jre
```

3. You can also check the version of your JDK by specific block of code.

```
java -version
```

![Screenshot (107)](https://github.com/user-attachments/assets/8b9f3603-ec65-4934-b84d-891efc392688)

![Screenshot (108)](https://github.com/user-attachments/assets/17def9af-d5fe-48a6-94af-947cb8f6386c)

  ![Screenshot (109)](https://github.com/user-attachments/assets/ea6d7df1-fd1c-41cf-81bb-832636ecd5b5)

## Installing and Starting Jenkins

1. Add jenkins Repository to your package manager

```
sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc]" \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins
```

![Screenshot (110)](https://github.com/user-attachments/assets/b3a44425-e6cb-46f9-942f-c859b90678da)

2. Now, install jenkins using the package manager

```
sudo apt-get install jenkins
```

3. Once jenkins is installed, start the jenkins server and also check the status of jenkins whether it is running or not 

```
 sudo systemctl enable jenkins
 sudo systemctl start jenkins
 sudo systemctl status jenkins
```

![Screenshot (113)](https://github.com/user-attachments/assets/9fda45d6-9d35-4b8d-9f88-86213b41fdb0)

## Acessing the jenkins Web Interface 

1. Jenkins will ask you to unlock it using an initial admin password. Retrieve this password by running:

```
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

![Screenshot (114)](https://github.com/user-attachments/assets/8c28dafd-2735-46f1-b505-2f69ef6a0b7a)

2. Now after unlocking jenkins, it will prompt you to install plugins. Select "Install suggested plugins" for a standard setup.

![Screenshot (115)](https://github.com/user-attachments/assets/4dbe33b4-0daa-471d-a03a-c577c584bfda)

4. Now create your first admin user
5. After this setup, Jenkins is ready to be configured for our CI/CD pipeline.

![Screenshot (116)](https://github.com/user-attachments/assets/b726e923-2b47-4e35-90b3-e2316a30f4a2)

## Installing Docker on the EC2 instance

1. Install Docker on your EC2 instance with the following command:

```
sudo apt install docker.io
```

## Configuring jenkins Credentials

1. Go to 'Manage Jenkins' in the sidebar.
2. Select 'Manage Credentials' from the list.

![Screenshot (118)](https://github.com/user-attachments/assets/be7a36f0-78cf-448b-bcf4-6c9ac940a962)

3. Now choose the appropriate domain (usually Global).
4. After that click on 'Add Credentials'.
5. Kind: Select SSH Username with private key.
6. Scope: Choose Global if you want the credential to be available to all jobs, in my case i have choosed global.

![Screenshot (120)](https://github.com/user-attachments/assets/472efea8-d17d-4512-8cdc-be218ac8754f)

7. ID: Enter a unique identifier for this credential (e.g. github-jekins-integration).
8. Username: Provide the SSH username that Jenkins will use to connect to the remote server, Eg. Ubuntu.
 
![Screenshot (122)](https://github.com/user-attachments/assets/b6d832b6-cff0-4900-9277-883d4d1b060b)

9. Now we have to provide it with the private key as jenkins will then authenticate with our git repositories securely using SSH.
- We can generate the SSH key pair, you can generate one using the following command:

```
ssh-keygen
```

- Add the public key to your Github Account:
- Navigate to Settings > SSH and GPG keys.
- Click on New SSH key, give it a title (e.g., "Jenkins CI Key"), and paste the public key into the key field.
- Click Add SSH key to save it.

- Now Add the private key to Jenkins credentials provider 

![Screenshot (123)](https://github.com/user-attachments/assets/4b309ed7-acb7-46b4-af8d-776ea9f24ef2)

![Screenshot (124)](https://github.com/user-attachments/assets/3afcb4d0-ab87-43fe-900c-daed62e668de)

After all these steps Jenkins is now Configured successfully.

![Screenshot (128)](https://github.com/user-attachments/assets/3a95eb55-75dc-4bb9-aa71-b984ef43b8ab)

## Installing Node.js

1. Firstly, install Node.js using the following command:

```
sudo apt install nodejs
```

![Screenshot (130)](https://github.com/user-attachments/assets/a0b51bc8-73b1-4702-81aa-61e9fff3f342)

2. Navigate to the application's root directory and install the npm packages using these commands:

```
cd /path/to/your/todo-app
npm install 
```

![Screenshot (133)](https://github.com/user-attachments/assets/53caafe8-4819-45e3-8b0a-711641bc2fc1)

## Installing and Configuring Docker 

- Now we need to install Docker as it is essential for containerizing of Node.js To-Do application, allowing it to run consistently across different environments. Follow these steps

1. Start by installing Docker and also add the EC2 user to the Docker group using these command:

```
sudo apt install docker.io
sudo service docker start
sudo systemctl enable docker
sudo usermod -aG docker ec2-user

```

![Screenshot (138)](https://github.com/user-attachments/assets/4182fd27-f0ec-4966-adc8-843e377c5e98)

2. The next step is to create a Dockerfile for our Node.js To-Do application. This file is crucial as it defines the environment in which your application will run.

- Writing the Dockerfile:

```
FROM node:12.29.0:alpine
WORKDIR app
COPY . .
RUN npm install
EXPOSE 8000
CMD ["node","app.js"]

```

![Screenshot (139)](https://github.com/user-attachments/assets/c9f2803a-c340-4cda-b63a-f1886e34f51e)

3. After creating the Dockerfile, you can build your Docker image with the following command:

```
docker build . -t todo-node-app
```

![Screenshot (140)](https://github.com/user-attachments/assets/2a3231d0-a0ea-4651-b318-ddae7c0437e2)

![Screenshot (143)](https://github.com/user-attachments/assets/97190791-e96e-4781-8775-b34f1484911e)

### Now we have to configure jenkins to build and Deploy our docker application

1. Navigate to Jenkins Dashboard
2. Configure the Project
3. Scroll down to the Build section and click on Add build step.
4. Scroll down to the Build section and click on Add build step.

- Now provide it with the shell commands written below

```
# Build the Docker image
docker build . -t node-app

# Stop and remove the previous container if it exists
docker stop todo-app-container || true
docker rm todo-app-container || true

# Run the new Docker container
docker run -d --name todo-app-container -p 8000:8000
```

![Screenshot (146)](https://github.com/user-attachments/assets/9ccfea9f-6099-4d9c-8fa0-efa6e7b2df74)

- You can check the output by going to "Console Output" tab.

![Screenshot (152)](https://github.com/user-attachments/assets/c54b4a52-54ac-4be4-8128-d006c5c1290e)

- After the build is complete, you can verify that your To-Do application is running by accessing http://your-ec2-public-ip:8000 in your web browser.

![Screenshot (154)](https://github.com/user-attachments/assets/f5e63992-7b05-470e-9cd5-fb10d4bf64c0)

## Installing the GitHub Integration Plugin and Setting Up Webhooks

- To fully automate your CI/CD pipeline, you need to integrate Jenkins with GitHub. This allows Jenkins to automatically trigger builds whenever changes are pushed to your GitHub repository. The following steps cover the installation of the GitHub integration plugin, setting up a webhook in GitHub, and configuring Jenkins to use the webhook.

### Install the GitHub Integration Plugin

1. Go to the Jenkins dashboard.
2. Click on Manage Jenkins from the left sidebar.
3. Select Manage Plugins.

### Search for Github PLugin

1. In the Available tab, use the search bar to look for "GitHub Integration Plugin."
2. Check the box next to the plugin and click Install without restart.

![Screenshot (156)](https://github.com/user-attachments/assets/91373f48-569f-473b-a86a-8f6174adde5c)

## Set Up a Webhook in GitHub

- A webhook allows GitHub to notify Jenkins whenever you push changes to your repository, triggering a new build automatically.

1. Go to the GitHub repository that contains your Node.js To-Do application.
2. Click on Settings in the repository menu.
3. In the left sidebar, click on Webhooks.
4. Click on Add webhook.
5. In the Payload URL field, enter your Jenkins URL followed by /github-webhook/ (e.g., http://your-jenkins-url/github-webhook/).
6. Under Which events would you like to trigger this webhook?, select Just the push event.
7. Click Add webhook to save the configuration.

![Screenshot (160)](https://github.com/user-attachments/assets/2ab219f8-8744-45c7-b06c-f3511dfcd8d8)

## Enable GitHub Hook Trigger in Jenkins

- Now that your GitHub repository is set up with a webhook, configure Jenkins to trigger builds based on these webhooks.

1. Go to the Jenkins dashboard.
2. Click on your project to open the project dashboard.
3. Click on Configure in the left sidebar.
4. Scroll down to the Build Triggers section.
5. Check the box next to GitHub hook trigger for GITScm polling. This setting allows Jenkins to trigger a build whenever a push is made to your GitHub repository, as notified by the webhook.
6. Scroll down and click Save to store the changes.

![Screenshot (163)](https://github.com/user-attachments/assets/1623ff16-904c-4b0b-a4b8-8dd1b331f168)


![Screenshot (164)](https://github.com/user-attachments/assets/c6752384-433d-4d6b-88aa-476be04ea144)

## Test the webhook Integration 

- To test if the webhook integration is working:
  1. Make a small change to your code and push it to your GitHub repository.
  2. Jenkins should automatically start a new build once the push is detected.
 
![Screenshot (168)](https://github.com/user-attachments/assets/889d6ecf-2bed-43cc-a53c-d2ce198bff59)


![Screenshot (170)](https://github.com/user-attachments/assets/44a6655e-7cfe-41e6-a946-5eb931b8fdda)

## Conclusion

By following this guide, you've successfully set up a comprehensive CI/CD pipeline using Jenkins, Docker, and GitHub. This pipeline automates the process of building, testing, and deploying a Dockerized Node.js To-Do application on an AWS EC2 instance. With GitHub integration and webhooks, your pipeline is fully automated, ensuring that every code change is quickly and reliably deployed.

## Future Improvements

- There are several ways you can further enhance this project:
  1. Automated Testing: Integrate automated testing into your Jenkins pipeline to ensure code quality before deployment.
  2. Monitoring and Logging: Set up monitoring and logging tools to keep track of application performance and detect any issues in real time.

     
 
Thank you for exploring this project. I hope it provides valuable insights into building and automating CI/CD pipelines. Feel free to reach out if you have any questions or suggestions for improvement.

