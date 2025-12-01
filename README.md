Here is a comprehensive README.md file covering Experiments 8 through 12, derived strictly from the provided laboratory record. This guide details the steps for Jenkins automation, Scripted Pipelines, Minikube/Nagios monitoring, CI/CD with Webhooks/Email, and AWS Cloud Deployment.

Software Engineering Lab Record (Experiments 8 - 12)
This repository contains the documentation and step-by-step procedures for advanced Software Engineering lab experiments, focusing on DevOps tools including Jenkins, Docker, Kubernetes (Minikube), Nagios, and AWS EC2.

Table of Contents
Experiment 8: Jenkins Automation (Manual Pipelines)

Experiment 9: Pipeline Creation using Script

Experiment 10: Working with Minikube and Nagios

Experiment 11: Jenkins CI/CD (Webhooks & Email)

Experiment 12: AWS EC2 Deployment

Experiment 8: Jenkins Automation
Objective: Hands-on practice on manual creation of Jenkins pipelines using Maven projects (Java and Web) from GitHub.

Part A: Maven Java Project Automation
1. Create Build Job (MavenJava_Build)

Type: Freestyle Project.

Source Code Management: Git (Enter Repository URL).

Build Steps: Invoke top-level Maven targets.

Goals: clean install

Post-build Actions:

Archive the artifacts: **/*

Build other projects: MavenJava_Test (Trigger only if build is stable).

2. Create Test Job (MavenJava_Test)

Type: Freestyle Project.

Build Environment: Check Delete workspace before build starts.

Build Steps:

Copy artifacts from another project:

Project Name: MavenJava_Build

Artifacts to copy: **/*

Invoke top-level Maven targets:

Goals: test

Post-build Actions: Archive the artifacts (**/*).

3. Create Pipeline View

Click + -> Name: MavenJava_Pipeline -> Select Build Pipeline View.

Layout: Based on upstream/downstream relationship.

Initial Job: MavenJava_Build.

Run: Trigger the pipeline and verify green status across all stages.

Part B: Maven Web Project Automation
1. Create Build Job (MavenWeb_Build)

Type: Freestyle Project.

Source Code Management: Git (Enter Web Project Repo URL).

Build Steps: Maven Goals -> clean install.

Post-build Actions:

Archive artifacts (**/*).

Build other projects: MavenWeb_Test.

2. Create Test Job (MavenWeb_Test)

Type: Freestyle Project.

Build Environment: Delete workspace before build starts.

Build Steps:

Copy artifacts from MavenWeb_Build.

Maven Goals -> test.

Post-build Actions:

Archive artifacts.

Build other projects: MavenWeb_Deploy.

3. Create Deploy Job (MavenWeb_Deploy)

Type: Freestyle Project.

Build Environment: Delete workspace before build starts.

Build Steps: Copy artifacts from MavenWeb_Test.

Post-build Actions: Deploy war/ear to a container.

WAR/EAR files: **/*.war

Context path: Webpath

Container: Tomcat 9.x Remote.

Credentials: admin / 1234.

Tomcat URL: http://localhost:8085/

4. Pipeline View

Create Build Pipeline View -> Initial Job: MavenWeb_Build.

Run pipeline and verify output at http://localhost:8085/Webpath.

Experiment 9: Pipeline Creation using Script
Objective: Create and evaluate a Jenkins pipeline using Groovy script (Jenkinsfile).

Part A: Scripted Pipeline Configuration
New Item: Pipeline Project (Name: Jenkins_Java_using_Script).

Definition: Pipeline script.

Script:

Groovy

pipeline {
    agent any
    tools {
        maven 'MAVEN_HOME' // Ensure Global Tool Config matches this name
    }
    stages {
        stage('git repo & clean') {
            steps {
                // bat "rmdir /s /q mavenjava" // Optional cleanup
                bat "git clone <YOUR_GITHUB_LINK>"
                bat "mvn clean -f mavenjava/pom.xml"
            }
        }
        stage('install') {
            steps {
                bat "mvn install -f mavenjava/pom.xml"
            }
        }
        stage('test') {
            steps {
                bat "mvn test -f mavenjava/pom.xml"
            }
        }
        stage('package') {
            steps {
                bat "mvn package -f mavenjava/pom.xml"
            }
        }
    }
}
Part B: Build Triggers
Build Periodically:

Syntax: H * * * * (Runs once every hour).

Poll SCM: Checks for changes in Git before building.

Experiment 10: Working with Minikube and Nagios
Objective: Manage Kubernetes pods and monitor services using Nagios.

Part A: Minikube (Kubernetes)
Start Cluster: minikube start

Create Deployment:

kubectl create deployment mynginx --image=nginx

Expose Service:

kubectl expose deployment mynginx --type=NodePort --port=80 --target-port=80

Scale Deployment:

kubectl scale deployment mynginx --replicas=4

Verify: kubectl get pods, kubectl get deployments.

Part B: Nginx Port Forwarding
Command: kubectl port-forward svc/mynginx 8081:80

Access: http://localhost:8081

Part C: Nagios Monitoring (Docker)
Pull Image: docker pull jasonrivers/nagios:latest

Run Container:

Bash

docker run --name nagiosdemo -p 8888:80 jasonrivers/nagios:latest
Access Dashboard: http://localhost:8888

Username: nagiosadmin

Password: nagios

Features: Monitor Hosts, Services (CPU/Memory), and Alerts.

Experiment 11: Jenkins CI/CD
Objective: Continuous Integration using Webhooks and Email Notifications.

Part A: CI using Webhooks (Ngrok)
Setup Ngrok:

Download and extract Ngrok.

Add Auth Token: ngrok config add-authtoken <YOUR_TOKEN>

Start Tunnel: ngrok http 8081 (Assuming Jenkins runs on 8081).

Copy the public HTTPS URL provided by Ngrok.

Configure GitHub Webhook:

Repo Settings -> Webhooks -> Add webhook.

Payload URL: https://<ngrok-url>/github-webhook/

Content type: application/json

Event: Just the push event.

Configure Jenkins Job:

In Job Configuration -> Build Triggers.

Select: GitHub hook trigger for GITScm polling.

Outcome: Pushing code to GitHub automatically triggers the Jenkins build.

Part B: Email Notifications
Gmail Setup: Enable 2-Step Verification -> Generate App Password (select 'Other').

Jenkins Configuration:

Install Email Extension Plugin.

Manage Jenkins -> Configure System -> E-mail Notification:

SMTP Server: smtp.gmail.com

Port: 465 (SSL enabled).

User: Your Gmail Address.

Password: The 16-digit App Password.

Job Configuration:

Post-build Actions: Editable Email Notification.

Triggers: Select Failure, Success, etc.

Recipient: Add email addresses.

Experiment 12: Creation of Virtual Machine and Deployment
Objective: Launch an Ubuntu EC2 instance on AWS and deploy a web application.

Part A: Launch EC2 Instance
Login: AWS Console -> Services -> EC2.

Launch Instance:

Name: ubuntu-server

AMI: Ubuntu Server (Free tier eligible).

Instance Type: t2.micro.

Key Pair: Create new .pem file.

Network: Allow HTTP/HTTPS traffic.

Storage: 8GB (Default).

Connect:

Use SSH client in terminal: ssh -i "keyname.pem" ubuntu@<public-dns>

Part B: Deploy Web Application (Docker)
Install Docker on EC2:

Bash

sudo apt-get update
sudo apt-get install docker.io
Setup Project:

git clone <YOUR_WEB_PROJECT_URL>

Navigate to folder.

Create/Edit Dockerfile using nano.

Build and Run:

Bash

sudo docker build -t mywebapp .
sudo docker run -d -p 80:80 mywebapp
Access: Open the EC2 Instance's Public IPv4 address in a browser.

Part C: Cleanup
Stop Container: sudo docker stop <container-id>

Terminate Instance: AWS Console -> Instance State -> Terminate.











apt-get install sudo
sudo apt-get install docker.io
clone your project
navigate to that folder(cd project folder cloned name)
sudo docker build -t image_name .
sudo docker run -d -p 80:80 image_name
sudo docker images
