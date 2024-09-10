# Jenkins First Freestyle Project

This project demonstrates how to set up a Jenkins Freestyle job to automate the execution of a Python script (hello.py) 
located in a local Linux directory. By configuring Jenkins with proper sudo permissions and setting up periodic build 
triggers, the job runs the script at scheduled intervals, displaying output directly in Jenkins.

##  1.Creating First Jenkins Job Using `hello.py` (Freestyle Project)

### Step 1: Configure `sudoers` File
To allow the Jenkins user to execute commands as root without needing a password, follow these steps:

1. **Open the `sudoers` file for editing:**
```
ubuntu@balasenapathi:~$ sudo gedit /etc/sudoers
[sudo] password for balu:balu
```
2. **Add the following line after all other lines in the sudoers file:**
```
jenkins ALL=(ALL) NOPASSWD: ALL
```
This line allows the Jenkins user to execute all commands without entering a password.

### Step 2: Create Python File (hello.py) on the Linux Machine
1. **Navigate to the desired directory:**
```
ubuntu@balasenapathi:~$ cd /home/ubuntu/pythonprograms
```
2. **If the directory doesn't exist, create it:**
```
ubuntu@balasenapathi:~$ mkdir pythonprograms
```
3. **Create a Python file:**
```
ubuntu@balasenapathi:~/pythonprograms$ gedit hello.py
```
4. **Add the following content to the hello.py file:**
```
print("Hello Balu Welcome to Jenkins")
```
5. **Run the Python file to verify:**
```
ubuntu@balasenapathi:~/pythonprograms$ sudo python3 hello.py
Hello Balu Welcome to Jenkins
```

## Step 3: Create a Jenkins Job
```
Open your Jenkins dashboard in a web browser (127.0.0.1:8080).
Click on New Item.
Enter the item name as HelloWorld.
Select Freestyle Project and click OK.
Job Configuration
General Section:

Enter a description:
Build and Run Python Program

Build Triggers:
Check Build periodically.

Schedule:
* * * * *
(This runs the job every minute)

Build Section:
Click Add Build Step and select Execute Shell.

Enter the following script:
cd /home/ubuntu/pythonprograms
sudo python3 hello.py
Click Save to save the job configurations.
```

## Step 4: Build the Job
```
Go to the Jenkins Dashboard and locate the HelloWorld job.

The job should trigger automatically (based on the schedule you set).
```

## Step 5: View Console Output
```
Click on the job HelloWorld.

Click on #1 (the build number).

Select Console Output to see the build logs.

Started by timer
Running as SYSTEM
Building in workspace /var/lib/jenkins/workspace/HelloWorld
[HelloWorld] $ /bin/sh -xe /tmp/jenkins15870243666755136137.sh
+ cd /home/ubuntu/pythonprograms
+ sudo python3 hello.py
Hello Balu Welcome to Jenkins
Finished: SUCCESS
```
## Step 6: Deleting the Jenkins Job
```
To delete the project, click on the Delete Project option in the HelloWorld job page.

A confirmation dialog will appear:

Delete the project 'HelloWorld'

Click OK to confirm.
```

















