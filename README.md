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

