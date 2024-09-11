# Jenkins Types of Projects

1. **Freestyle Project**:  
   A Freestyle project in Jenkins is the simplest form of a job configuration, ideal for single or straightforward tasks.
   It allows users to build, test, or run scripts without complex workflows or automation. These jobs are configured via
   the Jenkins UI, making it easy to set up with minimal technical knowledge. While it works for tasks like running tests,
   compiling code, or building binaries, it lacks the flexibility and power of more advanced Jenkins jobs, especially when
   it comes to CI/CD pipelines.

2. **Pipeline Project**:  
   A Pipeline project in Jenkins is a more advanced and flexible solution compared to Freestyle jobs. Pipelines automate
   the entire software development lifecycle, from building, testing, deploying, and releasing software. Pipelines are 
   defined through a Jenkinsfile, written using Groovy, which allows version control and advanced configuration. They are
   commonly used in single-branch setups and enable the implementation of continuous integration (CI) and continuous 
   delivery (CD) practices. Pipelines can be customized with stages, parallel steps, and integrations with external tools,
   offering better control over complex automation processes.

3. **Multibranch Pipeline Project**:  
   The Multibranch Pipeline project extends the capabilities of the standard pipeline by supporting multiple branches in
   a single repository. Jenkins automatically detects branches and creates individual pipelines for each. This is particularly
   useful in projects where different branches represent different stages of development (e.g., feature, development, production).
   The Multibranch Pipeline allows for automated CI/CD processes across all branches, and dynamically configures pipelines 
   as new branches are added or removed. This is ideal for teams using Git-based workflows like GitFlow or feature branching,
   simplifying the management of pipelines across multiple branches in a single repository.
   
   
## 1.Freestyle Project:  

**Creating a freestyle Jenkins project by using hello.py file present in git and github(Freestyle Project):**

### 1.First push the code [hello.py] into the github repository.
```
ubuntu@balasenapathi:~$ cd devops_balu_github/devops_balu_github/

ubuntu@balasenapathi:~/devops_balu_github/devops_balu_github$ ls
hello.py  README.md  test.txt

ubuntu@balasenapathi:~/devops_balu_github/devops_balu_github$ git status
On branch main
Your branch is up to date with 'origin/main'.

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	hello.py

nothing added to commit but untracked files present (use "git add" to track)

ubuntu@balasenapathi:~/devops_balu_github/devops_balu_github$ git add hello.py

ubuntu@balasenapathi:~/devops_balu_github/devops_balu_github$ git status
On branch main
Your branch is up to date with 'origin/main'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
	new file:   hello.py

ubuntu@balasenapathi:~/devops_balu_github/devops_balu_github$ git commit -m "first commit" hello.py
[main b636a13] first commit
 1 file changed, 1 insertion(+)
 create mode 100644 hello.py
 
ubuntu@balasenapathi:~/devops_balu_github/devops_balu_github$ git push -u origin main
Enumerating objects: 4, done.
Counting objects: 100% (4/4), done.
Delta compression using up to 2 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 341 bytes | 170.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0)
To https://github.com/balusena/devops_balu_github.git
   5cbba4b..b636a13  main -> main
Branch 'main' set up to track remote branch 'main' from 'origin'.

ubuntu@balasenapathi:~/devops_balu_github/devops_balu_github$ git status
On branch main
Your branch is up to date with 'origin/main'.

nothing to commit, working tree clean
```

### 2.Next login into the jenkins dashboard and register GitHub Credentials in Jenkins

**Follow these steps to permanently register your GitHub credentials in Jenkins:**

### Step 1: Log in to the Jenkins Dashboard
```
Open your Jenkins dashboard in a web browser (`127.0.0.1:8080`).

Log in with your credentials.
```
### Step 2: Add GitHub Credentials
```
From the Jenkins Dashboard, navigate to:

Dashboard > Manage Jenkins > Security (Credentials) > Stores Scoped to Jenkins (System) > Global Credentials (Unrestricted)

Click on Add Credentials.
```
### Step 3: Fill in the New Credentials Form
```
Kind:  
Select `Username with password` from the dropdown list.

Scope:  
Select `Global (Jenkins, nodes, items, child items, etc)`.

Username:  
Enter `github_username` (GitHub username).

Password:  
Enter `github_password` (GitHub password).

ID:  
Enter `devops_balu_github`.

Description:  
Enter `My GitHub Credentials`.
```
### Step 4: Save the Credentials
```
Click **Create** or **Save** to register the credentials.
```
## Global Credentials Overview
```
After saving, the credentials will appear in the **Global Credentials (Unrestricted)** section:

| ID                 | Name                         | Kind                    | Description             |
|--------------------|------------------------------|-------------------------|-------------------------|
|`devops_balu_github`| `github_username/******`     | `Username with password`| `My GitHub Credentials` |

These global credentials can be used across Jenkins for any jobs, nodes, or child items without needing to define them separately.
```
### 3.Now create your job in Jenkins by using create new job/new item

**Follow these steps to create a Freestyle job from your Jenkins Dashboard:**

```
Go to New Item

Enter an item name?
HelloWorld

Now select FreeStyleproject -----> click ok

General?
Description? 
This job will run the python code present in GitHub repository

Source Code Management?
Git

Repositories?
Repositories URL?
https://github.com/balusena/devops_balu_github.git

Credentials?
github_username/github_password(My GitHub Credentials)

Brances to build?
Branch Specifier (blank for 'any')

*/main

Repository browser? 
(Auto)

Build Triggers?

Build Environment?

Build steps?
Execute Shell?
command [sudo python3 hello.py]

[Save]

Build Now -----> click on it 

Now click on #1

Build #1 (26 May 2023, 21:16:33)
Add description
No changes.
Started by user balusena

	Revision: b636a12bcd9586c7d3efc674b8b4e563c65f5bb5
Repository: https://github.com/balusena/devops_balu_github.git
refs/remotes/origin/main

Console Output -----> click on it 

Started by user balusena
Running as SYSTEM
Building in workspace /var/lib/jenkins/workspace/HelloWorld
The recommended git tool is: NONE
using credential devops_balu_github
Cloning the remote Git repository
Cloning repository https://github.com/balusena/devops_balu_github.git
 > git init /var/lib/jenkins/workspace/HelloWorld # timeout=10
Fetching upstream changes from https://github.com/balusena/devops_balu_github.git
 > git --version # timeout=10
 > git --version # 'git version 2.25.1'
using GIT_ASKPASS to set credentials My GitHub Credentials
 > git fetch --tags --force --progress -- https://github.com/balusena/devops_balu_github.git +refs/heads/*:refs/remotes/origin/* # 
 timeout=10
 > git config remote.origin.url https://github.com/balusena/devops_balu_github.git # timeout=10
 > git config --add remote.origin.fetch +refs/heads/*:refs/remotes/origin/* # timeout=10
Avoid second fetch
 > git rev-parse refs/remotes/origin/main^{commit} # timeout=10
Checking out Revision b636a12bcd9586c7d3efc674b8b4e563c65f5bb5 (refs/remotes/origin/main)
 > git config core.sparsecheckout # timeout=10
 > git checkout -f b636a12bcd9586c7d3efc674b8b4e563c65f5bb5 # timeout=10
Commit message: "first commit"
First time build. Skipping changelog.
[HelloWorld] $ /bin/sh -xe /tmp/jenkins16054789987406362221.sh
+ sudo python3 hello.py
Hello Balu Welcome to Jenkins
Finished: SUCCESS   
```

## 2.Pipeline Project

### 1. What is a Pipeline?

A pipeline is a series of automated steps (jobs) that are linked together and executed sequentially, with automatic triggers.
It is used to streamline processes like continuous integration (CI), continuous delivery (CD), and continuous deployment. 
Each step in the pipeline corresponds to a stage in the software development lifecycle.

### Example of CI/CD Pipeline:

1. **Continuous Integration**:  
   Developers commit code, which is then tested and integrated automatically:
   - Dev Stage → Application Test → Integration Test

2. **Continuous Delivery**:  
   After code integration, it continues through further testing to ensure readiness for release:
   - Dev Stage → Application Test → Integration Test → Acceptance Test

3. **Continuous Deployment**:  
   In this scenario, the code goes all the way to production automatically:
   - Dev Stage → Application Test → Integration Test → Acceptance Test → Production

### 2. What is Jenkins Pipeline?

A Jenkins Pipeline is a mechanism to define and automate a CI/CD pipeline within Jenkins. It enables the chaining of stages
(jobs) such as building, deploying, testing, and releasing software. Jenkins Pipeline simplifies automating the entire 
lifecycle of a project from start to finish, with each stage executing after the previous one is successful. 

### Example of a Jenkins Pipeline Flow:

- **Build** → **Deploy** → **Test** → **Release**

Using a pipeline allows for enhanced flexibility, control, and integration with external tools, making it a popular choice
for implementing CI/CD practices.

### 3. What is Jenkinsfile?

A Jenkinsfile is a script that defines the Jenkins pipeline "as code." It is written in Groovy, and it describes the stages
(jobs) to be executed as part of the pipeline. The Jenkinsfile is stored in the version control system, which enables better
collaboration and traceability. By treating pipelines as code, Jenkinsfile allows for repeatability, versioning, and easier
maintenance.

### Key Features of Jenkinsfile:
- **Declarative Syntax**: Easier to write and understand for simpler pipelines.
- **Scripted Syntax**: Provides greater flexibility and control for advanced pipelines.
- **Version Control**: The Jenkinsfile can be versioned, tracked, and managed alongside the source code, ensuring consistency in the pipeline.

### Creating Jenkins Pipeline job "Helloworld" (Pipeline Project) without using Jenkinsfile:**

### Step 1: Start Jenkins
```
Now go to the web browser and run url :- 127.0.0.1:8080

Username: jenbalu	
Password: balu	
```

### Step 2: Install pipeline plugin
```
Go to Manage Jenkins > Manage Plugins > Installed > (Type pipeline in filter search)

If we have installed you will find out in the Available section.

If we dont find then go to Manage Jenkins > Manage Plugins > Avaialbe > (Type pipeline in filter search)

click on > install without restart
```

### Step 3: Create a new job where we are running a Jenkis job by creating a jenkins file from the script
```
Go to New Item

Enter an item name

[  PipelineOne  ]

Now select Pipeline

click -------> ok

Now create your job in jenkins by using create new job/new item

General?
Description? 
[This job will build a python pipeline from git and github repository]

Note:-Advanced Project Options in this we can set our project name manually, if not set default pipeline project name will be considered.

Advanced Project Options?
Display Name?
[PipelineOne]

If set,the optional display name is shown for the project throughout the Jenkins web GUI.As it is for display purposes only,the display
name is not required to be unique amongst projects.
If the display name is not set, the Jenkins web GUI will default to showing the project name.

Pipeline

Here we can create our own Jenkinsfile manually or we can fetch/use the Jenkinsfile from SCM like Git/Github/GitLab/cloud_repository.
```
### Step 4: Create or get Jenkinsfile in Pipeline section
```
pipeline?
Definition? -----> we have two options 
1.Pipeline script ---> Select this for now
2.Pipeline script from SCM
                                
Note: A Jenkinsfile can be written in two types of syntax:-
1.Declarative structure
2.Scripted structure
Both are written with Groovy but have a different structure.

Note: The Scripted structure is old one and Descriptive structure is the new way to represent a Jenkinsfile 

Pipeline?
Definition?
Pipeline script?
script? 
v Hello World

pipeline {
    agent any

    stages {
        stage('Hello') {
            steps {
                echo 'Hello World'
            }
        }
    }
}

[/] Use Groovy Sandbox

Pipeline Syntax

click on [Apply] [Save]

Pipeline PipelineOne
This job will build a python pipeline from git and github repository.

Stage View
No data available. This Pipeline has not yet run.

Build Now ===> Click on it

Stage View

Hello
Average stage times:
(Average full run time: ~3s)
300ms
#1
May 28
17:48
No Changes
300ms
Permalinks
Last build (#1), 1 day 2 hr ago
Last stable build (#1), 1 day 2 hr ago
Last successful build (#1), 1 day 2 hr ago
Last completed build (#1), 1 day 2 hr ago

Note: Now we can able to see the Stage view.

Now go to > Build History   trend v ------> click on it

#1 28 May 2023, 17:48:00 ------> click on it

Build #1 (28 May 2023, 17:48:00)
Started by user balusena

Console Output --------> click on it

Console Output
Started by user balusena
[Pipeline] Start of Pipeline
[Pipeline] node
Running on Jenkins in /var/lib/jenkins/workspace/PipelineOne
[Pipeline] {
[Pipeline] stage
[Pipeline] { (Hello) (hide)
[Pipeline] echo
Hello World
[Pipeline] }
[Pipeline] // stage
[Pipeline] }
[Pipeline] // node
[Pipeline] End of Pipeline
Finished: SUCCESS

Now adding some more stages [Build,Deploy,Test,Release] to the existing PipelineOne project.

Now Go to > Back to Project > Configure > Pipeline

pipeline {
    agent any

    stages {
        stage('Hello') {
            steps {
                echo 'Hello World'
            }
        }
        stage('Build') {
            steps {
                echo 'Building'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing'
            }
        }
        stage('Release') {
            steps {
                echo 'Releasing'
            }
        }
    }
}

[/] Use Groovy Sandbox

click on [Apply] [Save]

Build Now ===> click on it

Pipeline Python_Pipeline_Project
This job will build a python pipeline from git and github.
Edit description
Disable Project
Stage View
Hello	Build	Deploy	Test	Release
Average stage times:
(Average full run time: ~19s)
1s
842ms
550ms
215ms
255ms
#2
May 29
23:10
No Changes
1s
842ms
550ms
215ms
255ms
Permalinks
Last build (#2), 1 min 41 sec ago
Last stable build (#2), 1 min 41 sec ago
Last successful build (#2), 1 min 41 sec ago
Last completed build (#2), 1 min 41 sec ago

Note: Now we can able to see the Stage view.

Build History   trend v ------> click on it

Build #2 (29 May 2023, 23:10:45)
Add description
Started by user balusena

Console Output --------> click on it

Console Output
Started by user balusena
[Pipeline] Start of Pipeline
[Pipeline] node
Running on Jenkins in /var/lib/jenkins/workspace/Python_Pipeline_Project
[Pipeline] {
[Pipeline] stage
[Pipeline] { (Hello)
[Pipeline] echo
Hello World
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (Build)
[Pipeline] echo
Building
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (Deploy)
[Pipeline] echo
Deploying
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (Test)
[Pipeline] echo
Testing
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (Release)
[Pipeline] echo
Releasing
[Pipeline] }
[Pipeline] // stage
[Pipeline] }
[Pipeline] // node
[Pipeline] End of Pipeline
Finished: SUCCESS
```
### 2.Creating Jenkins job by using list.py file present in git and github (Pipeline Project) without using Jenkinsfile

### 1.Create a file list.py in github
```
ubuntu@balasenapathi:~/devops_balu_github/devops_balu_github$ git status
On branch main
Your branch is up to date with 'origin/main'.

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	list.py

nothing added to commit but untracked files present (use "git add" to track)

ubuntu@balasenapathi:~/devops_balu_github/devops_balu_github$ git add list.py

ubuntu-dsbda@ubuntudsbda-virtual-machine:~/devops_balu_github/devops_balu_github$ git status
On branch main
Your branch is up to date with 'origin/main'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
	new file:   list.py

ubuntu@balasenapathi:~/devops_balu_github/devops_balu_github$ git commit -m "listpy commit" list.py
[main 85c2146] listpy commit
 1 file changed, 18 insertions(+)
 create mode 100644 list.py

ubuntu@balasenapathi:~/devops_balu_github/devops_balu_github$ git push -u origin main
Enumerating objects: 4, done.
Counting objects: 100% (4/4), done.
Delta compression using up to 2 threads
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 538 bytes | 269.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0)
To https://github.com/balusena/devops_balu_github.git
   b636a13..85c2146  main -> main
Branch 'main' set up to track remote branch 'main' from 'origin'.
```

### 2.Now create your job in jenkins by using create new job/new item
```
Go to New Item

Enter an item name?
Python_Pipeline_Project

Now select Pipeline -----> click ok

General?
Description? 
This job will build a python pipeline from git and github.

Advanced Project Options?

Pipeline?
Definition?
Pipeline script ---> select this option
script?

pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId:
                 'devops_balu_github', url: 'https://github.com/balusena/devops_balu_github.git']]])
            }
        }
        stage('Build') {
            steps {
                git branch: 'main', credentialsId: 'devops_balu_github', url: 'https://github.com/balusena/devops_balu_github.git'
                sh 'sudo python3 list.py'
            }
        }
        stage('Test') {
            steps {
                echo 'The job has been tested'
            }
        }
    }
}


[/] Use Groovy Sandbox?

Pipeline Syntax

click on Save | Apply 

Build Now ===> Click on it

Now click on #1

Build #1 (27 May 2023, 02:34:17)
Add description
Started by user balusena

	Revision: 85c2146e42e3c82d649387072c9fed1773fe0989
Repository: https://github.com/balusena/devops_balu_github.git
refs/remotes/origin/main

Console output ===> Click on it
Console Output
Started by user balusena
[Pipeline] Start of Pipeline
[Pipeline] node
Running on Jenkins in /var/lib/jenkins/workspace/Python_Pipeline_Project
[Pipeline] {
[Pipeline] stage
[Pipeline] { (Checkout)
[Pipeline] checkout
The recommended git tool is: NONE
using credential devops_balu_github
Cloning the remote Git repository
Cloning repository https://github.com/balusena/devops_balu_github.git
 > git init /var/lib/jenkins/workspace/Python_Pipeline_Project # timeout=10
Fetching upstream changes from https://github.com/balusena/devops_balu_github.git
 > git --version # timeout=10
 > git --version # 'git version 2.25.1'
using GIT_ASKPASS to set credentials My GitHub Credentials
 > git fetch --tags --force --progress -- https://github.com/balusena/devops_balu_github.git +refs/heads/*:refs/remotes/origin/* # 
 timeout=10
 > git config remote.origin.url https://github.com/balusena/devops_balu_github.git # timeout=10
 > git config --add remote.origin.fetch +refs/heads/*:refs/remotes/origin/* # timeout=10
Avoid second fetch
 > git rev-parse refs/remotes/origin/main^{commit} # timeout=10
Checking out Revision 85c2146e42e3c82d649387072c9fed1773fe0989 (refs/remotes/origin/main)
 > git config core.sparsecheckout # timeout=10
 > git checkout -f 85c2146e42e3c82d649387072c9fed1773fe0989 # timeout=10
Commit message: "listpy commit"
First time build. Skipping changelog.
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (Build)
[Pipeline] git
The recommended git tool is: NONE
using credential devops_balu_github
 > git rev-parse --resolve-git-dir /var/lib/jenkins/workspace/Python_Pipeline_Project/.git # timeout=10
Fetching changes from the remote Git repository
 > git config remote.origin.url https://github.com/balusena/devops_balu_github.git # timeout=10
Fetching upstream changes from https://github.com/balusena/devops_balu_github.git
 > git --version # timeout=10
 > git --version # 'git version 2.25.1'
using GIT_ASKPASS to set credentials My GitHub Credentials
 > git fetch --tags --force --progress -- https://github.com/balusena/devops_balu_github.git +refs/heads/*:refs/remotes/origin/* # 
 timeout=10
 > git rev-parse refs/remotes/origin/main^{commit} # timeout=10
Checking out Revision 85c2146e42e3c82d649387072c9fed1773fe0989 (refs/remotes/origin/main)
 > git config core.sparsecheckout # timeout=10
 > git checkout -f 85c2146e42e3c82d649387072c9fed1773fe0989 # timeout=10
 > git branch -a -v --no-abbrev # timeout=10
 > git checkout -b main 85c2146e42e3c82d649387072c9fed1773fe0989 # timeout=10
Commit message: "listpy commit"
[Pipeline] sh
+ sudo python3 list.py
The reversed list is [10, 1, 6, 3, 9, 8]
The sorted list is [34, 54, 56, 64, 67, 76, 78, 87, 91, 120, 345]
List3 = [10, 1, 6, 3, 9, 8]
The index values are [56, 64, 67, 76]
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (Test)
[Pipeline] echo
The job has been tested
[Pipeline] }
[Pipeline] // stage
[Pipeline] }
[Pipeline] // node
[Pipeline] End of Pipeline
Finished: SUCCESS
```

### 3.How to get Jenkinsfile from Git SCM.

### 1.Now create a jenkinsfile in your local directory and push to your repository (devops_balu_github)
```
ubuntu@balasenapathi:~/devops_balu_github/devops_balu_github$ git status
On branch main
Your branch is up to date with 'origin/main'.

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	jenkinsfile

nothing added to commit but untracked files present (use "git add" to track)

ubuntu@balasenapathi:~/devops_balu_github/devops_balu_github$ git add jenkinsfile

ubuntu@balasenapathi:~/devops_balu_github/devops_balu_github$ git commit -m "jenkinsfile commit" jenkinsfile
[main 185b5bc] jenkinsfile commit
 1 file changed, 31 insertions(+)
 create mode 100644 jenkinsfile

ubuntu@balasenapathi:~/devops_balu_github/devops_balu_github$ git push -u origin main
Enumerating objects: 4, done.
Counting objects: 100% (4/4), done.
Delta compression using up to 2 threads
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 396 bytes | 198.00 KiB/s, done.
Total 3 (delta 1), reused 0 (delta 0)
remote: Resolving deltas: 100% (1/1), completed with 1 local object.
To https://github.com/balusena/devops_balu_github.git
   85c2146..185b5bc  main -> main
Branch 'main' set up to track remote branch 'main' from 'origin'.
```
### 2.This is the Jenkinsfile file that was pushed to GitHub Repository
```
jenkinsfile:

pipeline {
    agent any

    stages {
        stage('Hello') {
            steps {
                echo 'Hello World'
            }
        }
        stage('Build') {
            steps {
                echo 'Building'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing'
            }
        }
        stage('Release') {
            steps {
                echo 'Releasing'
            }
        }
    }
}
```
### 3.Now create your Jenkins Pipeline job by using Jenkinsfile from GitHub.
```
Under Jenkins job> Pipeline section > Select Definition > Pipeline script from SCM

Pipeline?

Definition [ Pipeline script from SCM ]

Step 5:- Add repo and Jenkinsfile location in the job under Pipeline section.

SCM?
Git?

Repositories?
Repository URL?
[ https://github.com/balusena/devops_balu_github.git ]

Credentials?
github_username/github_password(My GitHub Credentials)

Branches to build?
Branch Specifier (blank for 'any')?
[*/main]

Repository browser?
[ Auto ]

Script Path?
Jenkinsfile
[/]Lightweight Checkout?
Pipeline Syntax

Now Apply and Save

Build Now ===> click on it

Build #4 (30 May 2023, 01:50:09)
Add description
Changes
jenkinsfile commit (details / githubweb)
Started by user balusena

	Revision: 185b5bce33d15932939b1c56ef2d123b95ec6173
Repository: https://github.com/balusena/devops_balu_github.git
refs/remotes/origin/main

Console output ===> click on it

Console Output
Started by user balusena
Obtained jenkinsfile from git https://github.com/balusena/devops_balu_github.git
[Pipeline] Start of Pipeline
[Pipeline] node
Running on Jenkins in /var/lib/jenkins/workspace/Python_Pipeline_Project
[Pipeline] {
[Pipeline] stage
[Pipeline] { (Declarative: Checkout SCM)
[Pipeline] checkout
Selected Git installation does not exist. Using Default
The recommended git tool is: NONE
using credential devops_balu_github
 > git rev-parse --resolve-git-dir /var/lib/jenkins/workspace/Python_Pipeline_Project/.git # timeout=10
Fetching changes from the remote Git repository
 > git config remote.origin.url https://github.com/balusena/devops_balu_github.git # timeout=10
Fetching upstream changes from https://github.com/balusena/devops_balu_github.git
 > git --version # timeout=10
 > git --version # 'git version 2.25.1'
using GIT_ASKPASS to set credentials My GitHub Credentials
 > git fetch --tags --force --progress -- https://github.com/balusena/devops_balu_github.git +refs/heads/*:refs/remotes/origin/* # 
 timeout=10
 > git rev-parse refs/remotes/origin/main^{commit} # timeout=10
Checking out Revision 185b5bce33d15932939b1c56ef2d123b95ec6173 (refs/remotes/origin/main)
 > git config core.sparsecheckout # timeout=10
 > git checkout -f 185b5bce33d15932939b1c56ef2d123b95ec6173 # timeout=10
Commit message: "jenkinsfile commit"
 > git rev-list --no-walk 85c2146e42e3c82d649387072c9fed1773fe0989 # timeout=10
[Pipeline] }
[Pipeline] // stage
[Pipeline] withEnv
[Pipeline] {
[Pipeline] stage
[Pipeline] { (Hello)
[Pipeline] echo
Hello World
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (Build)
[Pipeline] echo
Building
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (Deploy)
[Pipeline] echo
Deploying
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (Test)
[Pipeline] echo
Testing
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (Release)
[Pipeline] echo
Releasing
[Pipeline] }
[Pipeline] // stage
[Pipeline] }
[Pipeline] // withEnv
[Pipeline] }
[Pipeline] // node
[Pipeline] End of Pipeline
Finished: SUCCESS
```
   
   
   
   
   
   
   
   
   
   
   
   
   
