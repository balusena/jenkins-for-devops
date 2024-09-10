# Jenkins For DevOps

## 1.Introduction to Jenkins
Jenkins is an open-source automation server that enables continuous integration (CI) and continuous delivery (CD) in 
software development. It automates tasks such as building, testing, and deploying software, reducing the need for manual
intervention. Jenkins helps developers integrate code changes frequently, ensuring early detection of bugs, and allows 
for quicker feedback loops. It's built in Java and is highly customizable with a rich ecosystem of plugins that extend 
its functionality. Jenkins supports a variety of version control systems (Git, SVN, etc.) and can be integrated with many
build tools and deployment platforms.

## 2.Jenkins Features
Jenkins offers a wide range of features designed to streamline development pipelines:

- **Ease of Use**: Simple setup with a web-based interface.
- **Extensibility**: Over 1,500 plugins available, supporting integration with various tools like Docker, Kubernetes, Git, and more.
- **Distributed Builds**: Supports running builds on multiple nodes (agents), helping to distribute load.
- **Pipeline as Code**: Jenkins allows you to define your entire build pipeline as code using `Jenkinsfile`, offering better maintainability.
- **Platform Agnostic**: Runs on most platforms including Windows, Linux, and macOS.
- **Robust Reporting**: Provides detailed build and test reports, improving debugging and optimization efforts.
- **Community Support**: Large active community contributing to Jenkins plugins, making it a flexible and evolving tool.

## 3.Jenkins Installation and Setup on Ubuntu Linux
Before installing Jenkins, ensure the following prerequisites are met:

- **Java Development Kit (JDK)**: Jenkins requires Java to run. You should install the **JDK 11** or **JDK 17**, as these are the supported versions for Jenkins.
- **Check Java Version**: Ensure that Java is installed and verify the version using the following command:

### 1.Install Java openjdk11 version in your linux system
```
ubuntu@balasenapathi:~$ sudo apt update

ubuntu@balasenapathi:~$ sudo apt install openjdk-11-jdk

ubuntu@balasenapathi:~$ java -version
openjdk version "11.0.19" 2023-04-18
OpenJDK Runtime Environment (build 11.0.19+7-post-Ubuntu-0ubuntu120.04.1)
OpenJDK 64-Bit Server VM (build 11.0.19+7-post-Ubuntu-0ubuntu120.04.1, mixed mode, sharing)
```
### 2.Install the web server like Tomcat or NGINX in your linux system
We need web server to run Jenkins on top of it so here we are installing NGINX
```
ubuntu@balasenapathi:~$ sudo apt-get install nginx
[sudo] password for balu:balu
Setting up nginx-core (1.18.0-0ubuntu1.2) ...
Setting up nginx (1.18.0-0ubuntu1.2) ...
Processing triggers for systemd (245.4-4ubuntu3.11) ...
Processing triggers for man-db (2.9.1-1) ...
Processing triggers for ufw (0.36-6) ...
```
### 3.Install Jenkins on your linux system
Debian/Ubuntu
On Debian and Debian-based distributions like Ubuntu you can install Jenkins through apt.

- **1.Add Jenkins Repository: First, add the Jenkins package repository to your system:**
```
ubuntu@balasenapathi:~$ wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -

ubuntu@balasenapathi:~$ sudo sh -c 'echo deb https://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d jenkins.list'
```
- **2.Install Jenkins: After adding the repository, update your package index and install Jenkins:**
```
ubuntu@balasenapathi:~$ sudo apt-get update

ubuntu@balasenapathi:~$ sudo apt-get install jenkins 
```
- **3.Start Jenkins: Once Jenkins is installed, start the Jenkins service:**
```
ubuntu@balasenapathi:~$ sudo systemctl start jenkins
```
- **4.Enable Jenkins on Boot: Enable Jenkins to start at system boot:**
```
ubuntu@balasenapathi:~$ sudo systemctl enable jenkins
```
- **5.Access Jenkins: After installation, Jenkins will run on port 8080. Open your web browser and navigate to 127.0.0.1:8080:**
```
http://127.0.0.1:8080
```
- **6.Unlock Jenkins You will be prompted to enter an initial administrator password, which can be found by running:**
To ensure Jenkins is securely set up by the administrator, a password has been written to the log (not sure where to find it?) 
and this file on the server:
```
/var/lib/jenkins/secrets/initialAdminPassword
```
- **7.Copy the password from either location and paste it below:**
```
ubuntu@balasenapathi:~$ sudo cat /var/lib/jenkins/secrets/initialAdminPassword
[sudo] password for ubuntu: balasenapathi
36082e0143594981aa764beaf4a760d3  -------> click on it
```
- **8.Copy the password from either location and paste it below:**
```
Administrator password
36082e0143594981aa764beaf4a760d3 
```
![Jenkins Administrator password](https://github.com/balusena/jenkins-for-devops/blob/main/Jenkins%20Introduction%20and%20Setup/jenkins-config-1.png)

- **9.Customize Jenkins Plugins extend Jenkins with additional features to support many different needs:**
```
Install suggested plugins  -------> click on it
```
![Jenkins Customize Plugins](https://github.com/balusena/jenkins-for-devops/blob/main/Jenkins%20Introduction%20and%20Setup/jenkins-config-2.png)

![Jenkins Getting Started](https://github.com/balusena/jenkins-for-devops/blob/main/Jenkins%20Introduction%20and%20Setup/jenkins-config-3.png)

- **10.Create First Admin User:**
```
Username: jenbalu
Password: balu	
Confirm password: balu	
Full name: balusena	
E-mail address: balasena@gmail.com  -------> Save and Continue
```
![Jenkins First Admin User](https://github.com/balusena/jenkins-for-devops/blob/main/Jenkins%20Introduction%20and%20Setup/jenkins-config-4.png)

![Jenkins Instance Configuration](https://github.com/balusena/jenkins-for-devops/blob/main/Jenkins%20Introduction%20and%20Setup/jenkins-config-5.png)

![Jenkins Is Ready ](https://github.com/balusena/jenkins-for-devops/blob/main/Jenkins%20Introduction%20and%20Setup/jenkins-config-6.png)

- **11.To get the status of Jenkins-Server:**
```
ubuntu@balasenapathi:~$ sudo systemctl status jenkins
[sudo] password for ubuntu:balasenapathi 
● jenkins.service - Jenkins Continuous Integration Server
     Loaded: loaded (/lib/systemd/system/jenkins.service; enabled; vendor preset: enabled)
     Active: active (running) since Fri 2023-05-26 17:27:03 IST; 49min ago
   Main PID: 1007 (java)
      Tasks: 44 (limit: 4572)
     Memory: 435.4M
     CGroup: /system.slice/jenkins.service
             └─1007 /usr/bin/java -Djava.awt.headless=true -jar /usr/share/java/jenkins.war --webroot=/var/cache/jenkins/war --
             httpPort=8080
```
- **12.To stop the Jenkins-Server:**
```
ubuntu@balasenapathi:~$ sudo systemctl stop jenkins
ubuntu-dsbda@ubuntudsbda-virtual-machine:~$ sudo systemctl status jenkins
● jenkins.service - Jenkins Continuous Integration Server
     Loaded: loaded (/lib/systemd/system/jenkins.service; enabled; vendor preset: enabled)
     Active: inactive (dead) since Fri 2023-05-26 18:20:03 IST; 5s ago
    Process: 1007 ExecStart=/usr/bin/jenkins (code=exited, status=143)
   Main PID: 1007 (code=exited, status=143)
     Status: "Jenkins stopped"
```
- **13.To start the Jenkins-Server:**
```
ubuntu@balasenapathi:~$ sudo systemctl start jenkins
```
### 4.Add jenkins user to docker to integrate jenkins and docker for CI/CD with github,jenkins,docker
```
ubuntu@balasenapathi:~$ sudo usermod -aG docker jenkins
```



