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






