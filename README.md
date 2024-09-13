# Jenkins For DevOps

![Jenkins](https://github.com/balusena/jenkins-for-devops/blob/main/jenkins.png)

## 1.Introduction to Jenkins

## 2.Jenkins Features
1. **Ease of Use**
2. **Extensibility**
3. **Distributed Builds** 
4. **Pipeline as Code**
5. **Platform Agnostic** 
6. **Robust Reporting**
7. **Community Support**

## 3.Jenkins Installation and Setup on Ubuntu Linux
1. **Java Development Kit (JDK)**
2. **Check Java Version**
    - **Install Java openjdk11 version in your linux system**
    - **Install the web server like Tomcat or NGINX in your linux system**
    - **Install Jenkins on your linux system**
       - **Add Jenkins Repository**
       - **Install Jenkins**
       - **Start Jenkins**
       - **Enable Jenkins on Boot**
       - **Access Jenkins**
       - **Unlock Jenkins**
       - **Copy the passwor**
       - **Customize Jenkins Plugins**
       - **Create First Admin User**
       - **To get the status of Jenkins-Server**
       - **To stop the Jenkins-Server**
       - **To start the Jenkins-Server:**

## 4.Add Jenkins user to docker and integrate Jenkins and Docker for CI/CD using GitHub, Jenkins, Docker


## 2.Jenkins First Freestyle Project

1. **Creating First Jenkins Job Using `hello.py` (Freestyle Project)**
    - **Step 1: Configure `sudoers` File**
       - **Open the `sudoers` file for editing**
       - **Add the following line after all other lines in the sudoers file**
    - **Step 2: Create Python File (hello.py) on the Linux Machine**
       - **Navigate to the desired directory**
       - **If the directory doesn't exist, create it**
       - **Create a Python file**
       - **Add the following content to the hello.py file**
       - **Run the Python file to verify**
    - **Step 3: Create a Jenkins Job**
    - **Step 4: Build the Job**
    - **Step 5: View Console Output**
    - **Step 6: Deleting the Jenkins Job**


## 3.Jenkins and Git Credential Integration

1. **Register GitHub Credentials in Jenkins**
    - **Step 1: Log in to the Jenkins Dashboard**
    - **Step 2: Add GitHub Credentials**
    - **Step 3: Fill in the New Credentials Form**
    - **Step 4: Save the Credentials**

2.**Global Credentials Overview**


## 4.Jenkins Types of Projects

1. **Freestyle Project** 
    - **Creating a freestyle Jenkins project by using hello.py file present in git and github(Freestyle Project)**
       - **First push the code [hello.py] into the github repository**
       - **Next login into the jenkins dashboard and register GitHub Credentials in Jenkins**
          - **Follow these steps to permanently register your GitHub credentials in Jenkins**
             - **Step 1: Log in to the Jenkins Dashboard**
             - **Step 2: Add GitHub Credentials**
             - **Step 3: Fill in the New Credentials Form**
             - **Step 4: Save the Credentials**
             - **Global Credentials Overview**
       - **Now create your job in Jenkins by using create new job/new item**
          - **Follow these steps to create a Freestyle job from your Jenkins Dashboard**

2. **Pipeline Project**

1. **What is a Pipeline?**

2. **Example of CI/CD Pipeline**
    - **Continuous Integration**
    - **Continuous Delivery**
    - **Continuous Deployment**

3. **What is Jenkins Pipeline?**
    - **Example of a Jenkins Pipeline Flow**

4. **What is Jenkinsfile?**
    - **Key Features of Jenkinsfile**
       - **Declarative Syntax**
       - **Scripted Syntax**
       - **Version Control**

5. **Creating Jenkins Pipeline job "Helloworld" (Pipeline Project) without using Jenkinsfile**
    - **Step 1: Start Jenkins**
    - **Step 2: Install pipeline plugin**
    - **Step 3: Create a new job where we are running a Jenkis job by creating a jenkins file from the script**
    - **Step 4: Create or get Jenkinsfile in Pipeline section**

6. **Creating Jenkins job by using list.py file present in git and github (Pipeline Project) without using Jenkinsfile**
    - **Create a file list.py in github**
    - **Now create your job in jenkins by using create new job/new item**
    - **How to get Jenkinsfile from Git SCM**
       - **Now create a jenkinsfile in your local directory and push to your repository (devops_balu_github)**
       - **This is the Jenkinsfile file that was pushed to GitHub Repository**
       - **Now create your Jenkins Pipeline job by using Jenkinsfile from GitHub**
    - **Step 5: Add repo and Jenkinsfile location in the job under Pipeline section**

3. **Multibranch Pipeline Project**
    - **To create Multibranch pipeline project with Jenkinsfile present in GitHub repository**

## 5.GitHub Credentials Scope and Commit Status Update Error in Jenkins

## 6.Credentials Plugin ===> To store and manage them Globally or Centrally

1. **Credentials Scope**
    - 1.**System**
    - 2.**Global** 

2. **Credential Types**
    - **User name with password**
    - **SSH user name with private key**
    - **Sceret File**
    - **Sceret Text**
    - **X_509 Certificate**
    - **Certificate**

3. **To create a Global Credential**

4. **To create a System Credential**

5. **To create a folder Credential**


## 5.Jenkins Basics of Jenkinsfile

1. **What is Jenkinsfile?**

2. **Key Features of Jenkinsfile**
    - **Declarative Syntax**
    - **Scripted Syntax**
    - **Version Control**

3. **Basics of Jenkinsfile**
    - **Pipeline Syntax**
       - **Scripted Pipeline**
          - **Example**
       - **Declarative Pipeline**
          - **Example**
    - **Required Fields of Jenkinsfile**
       - **pipeline**
       - **agent** 
       - **stages**
       - **stage and steps** 
    - **Jenkinsfile Example (Basic Stages)**
       - **Example of a Jenkinsfile that includes several stages for building a simple pipeline**
    - **Jenkinsfile Example (Build with Checkout and Execution)**
    - **Example includes Git checkout, build, and testing stages**
    - **Testing Changes in a Jenkinsfile (Replay Feature)**
       - **Replay Steps**
    - **Example Replay Script**
    - **Restart from Stage**
       - **Steps to Restart from a Stage**


## 6.Jenkins Automatic Build Triggers

1. **Trigger Jenkins Build Automatically: (Git Integration)**

2. **Jenkins and GitHub Integration Workflow**

3. **Trigger Methods**
    - **Push Notification**
    - **Polling for Changes**

4. **Push Notification (Using Webhooks)**
    - **Jenkins Setup for GitHub Push Notifications**
       - **Install Jenkins Plugin**
       - **Configure Repository Server Hostname**
       - **Access Token or Credentials**
    - **Jenkins Configuration for GitHub Webhooks**
    - **GitHub Setup for Webhook URL**

5. **Polling for Changes**
    - **Jenkins Polling Setup**
    - **Example of Polling Workflow**

6. **Common Practice and Backup Strategy**
    - **Good Strategy**

7. **What is the Difference Between Webhooks and Polling in Jenkins?**


## 7.Jenkins Pipeline

1. **What is Jenkinsfile?**

2. **Key Features of Jenkinsfile**
    - **Declarative Syntax**
    - **Scripted Syntax**
    - **Version Control**

3. **From Scripted to Declarative Pipeline Syntax**

4. **Basic Structure of Jenkinsfile**
    - **Pipeline Syntax**
    - **Scripted Pipeline vs. Declarative Pipeline**
       - **Scripted Pipeline Example**
       - **Declarative Pipeline Example**

5. **Required fields of Jenkinsfile**
    - **pipeline**
    - **agent** 
    - **stages** 
    - **stage**
    - **Build a Multibranch Pipeline in Jenkins with GitHub Integration**

6. **Post Attribute in Jenkinsfile**
    - **Purpose**
       - **Post** 
    - **Conditions in the Post Section**
       - **always**
       - **success**
       - **failure**
    - **Example Jenkinsfile with Post Section**

7. **Define Conditionals/When Expression**
    - **Use Case** 
    - **Conditional Execution with `when**
       - **Expression** 
       - **Groovy Script** 
    - **Example Jenkinsfile**
       - **Explanation**
          - **when` Directive**
          - **Expression Example** 
          - **Groovy Script** 
          - **Environment Variables** 
          - **Complex Conditions** 

8. **Using Environmental Variables in Jenkinsfile**
    - **What Variables Are Available?**
    - **Defining Custom Environment Variables**
    - **Example Jenkinsfile**
       - **Explanation**
          - **Environment Block** 
          - **Groovy Syntax** 
          - **Ordinary String Representation** 

9. **Using Credentials in Jenkinsfile**
    - **Use Case**
       - **Define Credentials in Jenkins GUI** 
       - **Bind Credentials to Environment Variable**
       - **Install and Use the "Credentials Binding" Plugin**
    - **Example Jenkinsfile**
       - **Explanation**
          - **Environment Block**
          - **Credentials Binding** 
          - **Usage in Stages** 

10. **Using Credentials with Wrapper Syntax in Jenkinsfile**
     - **Localizing Credentials to Specific Stages**
     - **Example Jenkinsfile with Wrapper Syntax**
        - **Explanation**
           - **withCredentials Block**
           - **usernamePassword Method** 
     - **Required Plugins**
        - **Credentials Plugin**
        - **Credentials Binding Plugin** 

11. **Using Tools Attribute for Making Build Tools Available**
     - **Supported Build Tools**
        - **Gradle**
        - **Maven**
        - **JDK**
     - **Example Jenkinsfile**
     - **To check**
        - **Go to Manage Jenkins**
        - **Click on Global Tool Configuration**
        - **Verify the installation of the required tools**

12. **Using Parameters for a Parameterized Build**
     - **Types of Parameters**
        - **string**
        - **choice**
        - **booleanParam**
     - **Example Jenkinsfile**
     - **Usage**
     - **Dashboard Navigation**

13. **Using External Groovy Script**
     - **Groovy Script: `script.groovy`**
     - **Jenkinsfile: Jenkinsfile**
     - **Explanation**
        - **External Groovy Script (script.groovy)** 
        - **Jenkinsfile (jenkinsfile)**
           - **Load Script** 
           - **Use Functions** 












