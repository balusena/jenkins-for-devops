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

Build Now ===> click on it

Now click on #1

Build #1 (26 May 2023, 21:16:33)
Add description
No changes.
Started by user balusena

	Revision: b636a12bcd9586c7d3efc674b8b4e563c65f5bb5
Repository: https://github.com/balusena/devops_balu_github.git
refs/remotes/origin/main

Console Output ===> Click on it 

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
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
