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

