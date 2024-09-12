# Jenkins CI-CD Pipeline for ML-App (Money Authentication)

The **ML_APP_Docker_Jenkins_Automation** project integrates continuous integration and continuous deployment (CI/CD) using
**Linux, GitHub, Jenkins, and Docker** for a machine learning-based money authentication application. This pipeline automates
the process from code commit to deployment, ensuring efficient software delivery and maintaining consistency across environments.

## 1.Tools Overview

- **Linux**: Provides the underlying infrastructure for Jenkins and Docker, ensuring a reliable and stable environment.
- **GitHub**: Serves as the version control system, allowing developers to collaboratively work on the ML application’s codebase. The pipeline is triggered automatically via GitHub webhooks when code is pushed.
- **Jenkins**: Acts as the orchestrator for CI/CD, pulling the latest code from GitHub, building the application inside a Docker container, and running tests. Jenkins ensures that every commit is verified with automated builds and tests.
- **Docker**: Is used to package the ML application into a container. This ensures that the application runs consistently across different environments, from testing to production.

Once the Docker image is built, it is pushed to DockerHub, ready for deployment. This automated pipeline improves efficiency,
minimizes human error, and accelerates the release cycle of the ML-based money authentication app.

## 2.ML-App Project folder structure
```
ML_APP_Docker_Jenkins_Automation/
├── BankNote_Authentication.csv        # Dataset for training the model
├── Dockerfile                         # Dockerfile to build the Docker image
├── Jenkinsfile                        # Jenkins pipeline configuration for CI/CD
├── ModelTraining.ipynb                # Jupyter notebook for model training
├── TestFile.csv                       # Test dataset for validating the model
├── classifier.pkl                     # Serialized trained model (pickle file)
├── flask_api.py                       # Flask application to serve the ML model
├── requirements.txt                   # Python dependencies for the project
```
**Note:** See all the Credentials are configured correctly:

## 3.GitHub credentials configured in Jenkins: First register your github credentials in jenkins permanently
```
Dashboard > Manage Jenkins > Security(Credentials) > Stores scoped to Jekins(System)(Global Credentials Unrestricted) > Add Credentials

New Credentials?
kind?
[username with password]----> select this in list

Scope?
[Global(jenkins,nodes,items,child items,etc]

Username?
[github_username]

Password?
[Concealed[github_password]

ID? 
[devops_balu_github]

Description?
[My GitHub Credentials]

Create/save

Global Credentials(Unrestricted)
Credentials that should be available irrespective of domain specification to requirements matching.
| ID                 | Name                         | Kind                    | Description             |
|--------------------|------------------------------|-------------------------|-------------------------|
|`devops_balu_github`| `github_username/******`     | `Username with password`| `My GitHub Credentials` |
```

## 4.Trigger Jenkins Build Automatically: (Git Integration)

When changes are made to the repository (`devops_balu_github`), manually scanning the Multibranch Pipeline is not ideal.
Instead, Jenkins should automatically trigger builds. This can be achieved through two primary methods: 
**Push Notification** and **Polling for Changes**.

### 1.Jenkins and GitHub Integration Workflow

- **User** → Commits changes to **Git Repository** → Jenkins triggers build automatically.

### 2.Trigger Methods

1. **Push Notification**: Version control (GitHub) notifies Jenkins of new commit changes.
2. **Polling for Changes**: Jenkins polls the repository at regular intervals to detect changes.

**Note:** Push notifications are more efficient than polling because they communicate updates only when changes occur, 
unlike polling, which checks for changes continuously at set intervals.

### 3.Push Notification (Using Webhooks)

To set up **Push Notifications**, we configure both Jenkins and the SCM (GitHub) to work together.

### 4.Jenkins Setup for GitHub Push Notifications

1. **Install Jenkins Plugin**: Install the relevant plugin based on your SCM (e.g., GitHub).
2. **Configure Repository Server Hostname**.
3. **Access Token or Credentials**: Add your GitHub credentials (username, password, or access token).

### 5.Jenkins Configuration for GitHub Webhooks

We need to provide the configuration credentials of SCM such as (username, password, accesses token, webhooks etc...)

#### 1.Jenkins side connection with SCM(GitHub):
```
Go to Manage Jenkins ---> System

Gihub?

GitHub Servers?

[Add GitHub Server]

Advanced 

Override Hook URL

[/] Specify another hook URL for GitHub Configuration

[http://127.0.0.1:8080/github-webhook/]

Shared Secrets

[Add Shared Secrets]

Additional Actions?

[Manage additional GitHub actions]

[Save] ---> Click on it
```
**Note:** By doing this we can connect Jenkins with the GitHub(SCM) webhook.

#### 2.SCM(GitHub) side connection with Jenkins-webhook URL:
```
Go to your SCM(GitHub) official site

Repo ---> balusena/devops_balu_github

settings ---> click on it
|
 - Code and automation - Webhooks

Webhooks:

Webhooks allow external services to be notified when certain events happen. When the specified events happen, we’ll send a POST request 
to each of the URLs you provide. Learn more in our Webhooks Guide.

Webhooks / Add webhook
We’ll send a POST request to the URL below with details of any subscribed events. You can also specify which data format you’d like to 
receive (JSON, x-www-form-urlencoded, etc). More information can be found in our developer documentation.

Payload URL
#https://example.com/postreceive
[Provide your organization Payload URL] http://127.0.0.1:8080/github-webhook/ ---> this is not acceptable but looks similar to this

Content type
[application/x-ww-form-urlencoded]
[application/json]

Secret
[jenkinsbaluwebhook]

Which events would you like to trigger this webhook?
[*]Just the push event.
[ ]Send me everything.
[ ]Let me select individual events.

Active
We will deliver event details when this hook is triggered

[Add webhook] ---> click on it 
```
**Note:** Here [http://127.0.0.1:8080/github-webhook/] ---> Jenkinslistens for code changes done in GitHub(SCM)

**Caution:** Always use the organization URL/Webhook in Payload URL bcz the localhost URL is not accepted here.

```
Developer 
|
-------------------------------> GitHub                                      [Jenkins: Triggers that build for us]
       COMMITS TO                  |
                                   ---------------------------> Jenkins Webhook URL 
                                             PUSHES TO       
```

### 6.Polling for Changes

In this method, Jenkins polls the repository at regular intervals to detect any changes.

#### 1.Jenkins Polling Setup
```
Now go to my-pipeline(jenkins job)
 go to configure
 |
 - Scan Repository Triggers
 [/] Periodically if not otherwise run?
     interval?
     [Select the peroid from the list of options ]
     
     How often jenkins should pool the changes
[Save] ---> click on it
```
## 5.DockerHub credentials configured in Jenkins: First register your DockerHub credentials in jenkins permanently
```
Dashboard > Manage Jenkins > Security(Credentials) > Stores scoped to Jekins(System)(Global Credentials Unrestricted) > Add Credentials

New Credentials?
kind?
[username with password]----> select this in list

Scope?
[Global(jenkins,nodes,items,child items,etc]

Username?
[docker_username]

Password?
[Concealed[docker_password]

ID? 
[balusena_dockerhub]

Description?
[My DockerHub Credentials]

Create/save  --------> Click on it

Global Credentials(Unrestricted)
Credentials that should be available irrespective of domain specification to requirements matching.
| ID                    | Name                                               | Kind                     | Description               |
|-----------------------|----------------------------------------------------|--------------------------|---------------------------|
| `balusena_dockerhub`  | `docker_username/****** (My DockerHub Credentials)`| `Username with password` | `My DockerHub Credentials`|
```
## 6.Push all the files present in devops_balu_github into github repository.
```
ubuntu@balasenapathi:~$ cd devops_balu_github/devops_balu_github

ubuntu@balasenapathi:~/devops_balu_github/devops_balu_github$ pwd
/home/ubuntu/devops_balu_github/devops_balu_github

ubuntu@balasenapathi:~/devops_balu_github/devops_balu_github$ ls
hello.py  list.py  ML_APP_Docker_Jenkins_Automation  README.md  test.txt

ubuntu@balasenapathi:~/devops_balu_github/devops_balu_github$ git pull
Already up to date.

ubuntu@balasenapathi:~/devops_balu_github/devops_balu_github$ git status
On branch main
Your branch is up to date with 'origin/main'.

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	.ipynb_checkpoints/
	ML_APP_Docker_Jenkins_Automation/

nothing added to commit but untracked files present (use "git add" to track)

ubuntu@balasenapathi:~/devops_balu_github/devops_balu_github$ git add ML_APP_Docker_Jenkins_Automation

git add ML_APP_Docker_Jenkins_Automation/Jenkinsfile

ubuntu@balasenapathi:~/devops_balu_github/devops_balu_github$ git status
On branch main
Your branch is up to date with 'origin/main'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
	new file:   ML_APP_Docker_Jenkins_Automation/BankNote_Authentication.csv
	new file:   ML_APP_Docker_Jenkins_Automation/Dockerfile
	new file:   ML_APP_Docker_Jenkins_Automation/Jenkinsfile
	new file:   ML_APP_Docker_Jenkins_Automation/ModelTraining.ipynb
	new file:   ML_APP_Docker_Jenkins_Automation/TestFile.csv
	new file:   ML_APP_Docker_Jenkins_Automation/classifier.pkl
	new file:   ML_APP_Docker_Jenkins_Automation/flask_api.py
	new file:   ML_APP_Docker_Jenkins_Automation/requirements.txt

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	.ipynb_checkpoints/

ubuntu@balasenapathi:~/devops_balu_github/devops_balu_github$ git commit -m "ML_APP_Docker_Jenkins_Automation 
commit" ML_APP_Docker_Jenkins_Automation
[main 9918616] ML_APP_Docker_Jenkins_Automation commit
 8 files changed, 2023 insertions(+)
 create mode 100644 ML_APP_Docker_Jenkins_Automation/BankNote_Authentication.csv
 create mode 100644 ML_APP_Docker_Jenkins_Automation/Dockerfile
 create mode 100644 ML_APP_Docker_Jenkins_Automation/Jenkinsfile
 create mode 100644 ML_APP_Docker_Jenkins_Automation/ModelTraining.ipynb
 create mode 100644 ML_APP_Docker_Jenkins_Automation/TestFile.csv
 create mode 100644 ML_APP_Docker_Jenkins_Automation/classifier.pkl
 create mode 100644 ML_APP_Docker_Jenkins_Automation/flask_api.py
 create mode 100644 ML_APP_Docker_Jenkins_Automation/requirements.txt
 
 git commit -m "ML_APP_Docker_Jenkins_Automation/Jenkinsfile commit1" ML_APP_Docker_Jenkins_Automation
 
ubuntu@balasenapathi:~/devops_balu_github/devops_balu_github$ git push -u origin main
Enumerating objects: 12, done.
Counting objects: 100% (12/12), done.
Delta compression using up to 2 threads
Compressing objects: 100% (11/11), done.
Writing objects: 100% (11/11), 96.89 KiB | 3.88 MiB/s, done.
Total 11 (delta 1), reused 0 (delta 0)
remote: Resolving deltas: 100% (1/1), completed with 1 local object.
To https://github.com/balusena/devops_balu_github.git
   185b5bc..9918616  main -> main
Branch 'main' set up to track remote branch 'main' from 'origin'.
```
## 7.Create a Jenkins Pipeline job using Jenkinsfile:
```
1.Go to New Item

Enter an item name

[  This job will build ML APP Docker Jenkins Automation pipeline  ]

Now select Pipeline

click -------> ok

2.Now create your job in jenkins by using create new job/new item

Configure?
General?
Description? 
[ This job will build ML APP Docker Jenkins Automation pipeline ]

Build Triggers?

Advanced Project Options?

3.Create or get Jenkinsfile in Pipeline section

pipeline?
Definition? -----> we have two options 
1.Pipeline script 
2.Pipeline script from SCM ---> Select this for now

4.Add repo and Jenkinsfile location in the job under Pipeline section.

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
ML_APP_Docker_Jenkins_Automation/Jenkinsfile
[/]Lightweight Checkout?
Pipeline Syntax

Now Apply and Save
```
## 8.Now click on Build in your Jenkins Dashboard:
```
Build Now ===> click on it

Build #7 (19 Jun 2023, 15:39:26)
Add description
Changes
Update Jenkinsfile1 (details / githubweb)
Started by user balusena

	Revision: 967105ac0adaaa8d44a9708b12cf721b5b15dd80
Repository: https://github.com/balusena/devops_balu_github.git
refs/remotes/origin/main
	The following steps that have been detected may have insecure interpolation of sensitive variables (click here for an 
	explanation):
sh: [DOCKER_PASSWORD]
```
## 9.To get the console output from your Jenkins Dashboard:
```
Console output ===> click on it

Console Output
Started by user balusena
Obtained ML_APP_Docker_Jenkins_Automation/Jenkinsfile from git https://github.com/balusena/devops_balu_github.git
[Pipeline] Start of Pipeline
[Pipeline] node
Running on Jenkins in /var/lib/jenkins/workspace/ML_app pipeline
[Pipeline] {
[Pipeline] stage
[Pipeline] { (Declarative: Checkout SCM)
[Pipeline] checkout
Selected Git installation does not exist. Using Default
The recommended git tool is: NONE
using credential devops_balu_github
 > git rev-parse --resolve-git-dir /var/lib/jenkins/workspace/ML_app pipeline/.git # timeout=10
Fetching changes from the remote Git repository
 > git config remote.origin.url https://github.com/balusena/devops_balu_github.git # timeout=10
Fetching upstream changes from https://github.com/balusena/devops_balu_github.git
 > git --version # timeout=10
 > git --version # 'git version 2.25.1'
using GIT_ASKPASS to set credentials My GitHub Credentials
 > git fetch --tags --force --progress -- https://github.com/balusena/devops_balu_github.git +refs/heads/*:refs/remotes/origin/* # 
 timeout=10
 > git rev-parse refs/remotes/origin/main^{commit} # timeout=10
Checking out Revision 967105ac0adaaa8d44a9708b12cf721b5b15dd80 (refs/remotes/origin/main)
 > git config core.sparsecheckout # timeout=10
 > git checkout -f 967105ac0adaaa8d44a9708b12cf721b5b15dd80 # timeout=10
Commit message: "Update Jenkinsfile"
 > git rev-list --no-walk 7b2229037516c5699d35916470edd35dda71a387 # timeout=10
[Pipeline] }
[Pipeline] // stage
[Pipeline] withEnv
[Pipeline] {
[Pipeline] stage
[Pipeline] { (Checkout)
[Pipeline] checkout
Selected Git installation does not exist. Using Default
The recommended git tool is: NONE
using credential devops_balu_github
 > git rev-parse --resolve-git-dir /var/lib/jenkins/workspace/ML_app pipeline/.git # timeout=10
Fetching changes from the remote Git repository
 > git config remote.origin.url https://github.com/balusena/devops_balu_github.git # timeout=10
Fetching upstream changes from https://github.com/balusena/devops_balu_github.git
 > git --version # timeout=10
 > git --version # 'git version 2.25.1'
using GIT_ASKPASS to set credentials My GitHub Credentials
 > git fetch --tags --force --progress -- https://github.com/balusena/devops_balu_github.git +refs/heads/*:refs/remotes/origin/* # 
 timeout=10
 > git rev-parse refs/remotes/origin/main^{commit} # timeout=10
Checking out Revision 967105ac0adaaa8d44a9708b12cf721b5b15dd80 (refs/remotes/origin/main)
 > git config core.sparsecheckout # timeout=10
 > git checkout -f 967105ac0adaaa8d44a9708b12cf721b5b15dd80 # timeout=10
Commit message: "Update Jenkinsfile"
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (Build)
[Pipeline] dir
Running in /var/lib/jenkins/workspace/ML_app pipeline/ML_APP_Docker_Jenkins_Automation
[Pipeline] {
[Pipeline] git
Selected Git installation does not exist. Using Default
The recommended git tool is: NONE
using credential devops_balu_github
 > git rev-parse --resolve-git-dir /var/lib/jenkins/workspace/ML_app pipeline/ML_APP_Docker_Jenkins_Automation/.git # timeout=10
Fetching changes from the remote Git repository
 > git config remote.origin.url https://github.com/balusena/devops_balu_github.git # timeout=10
Fetching upstream changes from https://github.com/balusena/devops_balu_github.git
 > git --version # timeout=10
 > git --version # 'git version 2.25.1'
using GIT_ASKPASS to set credentials My GitHub Credentials
 > git fetch --tags --force --progress -- https://github.com/balusena/devops_balu_github.git +refs/heads/*:refs/remotes/origin/* # 
 timeout=10
 > git rev-parse refs/remotes/origin/main^{commit} # timeout=10
Checking out Revision 967105ac0adaaa8d44a9708b12cf721b5b15dd80 (refs/remotes/origin/main)
 > git config core.sparsecheckout # timeout=10
 > git checkout -f 967105ac0adaaa8d44a9708b12cf721b5b15dd80 # timeout=10
 > git branch -a -v --no-abbrev # timeout=10
 > git branch -D main # timeout=10
 > git checkout -b main 967105ac0adaaa8d44a9708b12cf721b5b15dd80 # timeout=10
Commit message: "Update Jenkinsfile1"
[Pipeline] sh
+ docker build -t money_api:latest .
Sending build context to Docker daemon  1.125MB

Step 1/7 : FROM python:3.8-slim-buster
 ---> 52456bc7bf3e
Step 2/7 : COPY . /usr/app/
 ---> bf83c562d5cb
Step 3/7 : EXPOSE 5000
 ---> Running in 597296caa58b
Removing intermediate container 597296caa58b
 ---> f5999c8c934a
Step 4/7 : WORKDIR /usr/app/
 ---> Running in a6de6fadbc84
Removing intermediate container a6de6fadbc84
 ---> bd82207d3445
Step 5/7 : RUN pip install --upgrade pip
 ---> Running in 1c3a5946cbd6
Requirement already satisfied: pip in /usr/local/lib/python3.8/site-packages (23.0.1)
Collecting pip
  Downloading pip-23.1.2-py3-none-any.whl (2.1 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 2.1/2.1 MB 3.6 MB/s eta 0:00:00
Installing collected packages: pip
  Attempting uninstall: pip
    Found existing installation: pip 23.0.1
    Uninstalling pip-23.0.1:
      Successfully uninstalled pip-23.0.1
Successfully installed pip-23.1.2
[91mWARNING: Running pip as the 'root' user can result in broken permissions and conflicting behaviour with the system package 
manager. It is recommended to use a virtual environment instead: https://pip.pypa.io/warnings/venv
[0mRemoving intermediate container 1c3a5946cbd6
 ---> 5d05fbf7e1cf
Step 6/7 : RUN pip install -r requirements.txt
 ---> Running in ed8fc6c8a1e1
Collecting flask==2.0.2 (from -r requirements.txt (line 1))
  Downloading Flask-2.0.2-py3-none-any.whl (95 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 95.2/95.2 kB 657.0 kB/s eta 0:00:00
Collecting gunicorn==20.1.0 (from -r requirements.txt (line 2))
  Downloading gunicorn-20.1.0-py3-none-any.whl (79 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 79.5/79.5 kB 471.6 kB/s eta 0:00:00
Collecting itsdangerous==2.0.1 (from -r requirements.txt (line 3))
  Downloading itsdangerous-2.0.1-py3-none-any.whl (18 kB)
Collecting Jinja2==3.0.1 (from -r requirements.txt (line 4))
  Downloading Jinja2-3.0.1-py3-none-any.whl (133 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 133.7/133.7 kB 979.5 kB/s eta 0:00:00
Collecting MarkupSafe==2.1.1 (from -r requirements.txt (line 5))
  Downloading MarkupSafe-2.1.1-cp38-cp38-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (25 kB)
Collecting Werkzeug==2.0.1 (from -r requirements.txt (line 6))
  Downloading Werkzeug-2.0.1-py3-none-any.whl (288 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 288.2/288.2 kB 3.5 MB/s eta 0:00:00
Collecting numpy==1.21.0 (from -r requirements.txt (line 7))
  Downloading numpy-1.21.0-cp38-cp38-manylinux_2_12_x86_64.manylinux2010_x86_64.whl (15.7 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 15.7/15.7 MB 3.6 MB/s eta 0:00:00
Collecting scipy==1.7.0 (from -r requirements.txt (line 8))
  Downloading scipy-1.7.0-cp38-cp38-manylinux_2_5_x86_64.manylinux1_x86_64.whl (28.4 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 28.4/28.4 MB 3.3 MB/s eta 0:00:00
Collecting scikit-learn==0.24.2 (from -r requirements.txt (line 9))
  Downloading scikit_learn-0.24.2-cp38-cp38-manylinux2010_x86_64.whl (24.9 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 24.9/24.9 MB 3.4 MB/s eta 0:00:00
Collecting matplotlib==3.4.3 (from -r requirements.txt (line 10))
  Downloading matplotlib-3.4.3-cp38-cp38-manylinux1_x86_64.whl (10.3 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 10.3/10.3 MB 4.5 MB/s eta 0:00:00
Collecting pandas==1.3.0 (from -r requirements.txt (line 11))
  Downloading pandas-1.3.0-cp38-cp38-manylinux_2_5_x86_64.manylinux1_x86_64.whl (10.6 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 10.6/10.6 MB 4.3 MB/s eta 0:00:00
Collecting flasgger==0.9.5 (from -r requirements.txt (line 12))
  Downloading flasgger-0.9.5-py2.py3-none-any.whl (3.8 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 3.8/3.8 MB 4.3 MB/s eta 0:00:00
Collecting click>=7.1.2 (from flask==2.0.2->-r requirements.txt (line 1))
  Downloading click-8.1.3-py3-none-any.whl (96 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 96.6/96.6 kB 616.7 kB/s eta 0:00:00
Requirement already satisfied: setuptools>=3.0 in /usr/local/lib/python3.8/site-packages (from gunicorn==20.1.0->-r requirements.txt (line 2)) (57.5.0)
Collecting joblib>=0.11 (from scikit-learn==0.24.2->-r requirements.txt (line 9))
  Downloading joblib-1.2.0-py3-none-any.whl (297 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 298.0/298.0 kB 4.1 MB/s eta 0:00:00
Collecting threadpoolctl>=2.0.0 (from scikit-learn==0.24.2->-r requirements.txt (line 9))
  Downloading threadpoolctl-3.1.0-py3-none-any.whl (14 kB)
Collecting cycler>=0.10 (from matplotlib==3.4.3->-r requirements.txt (line 10))
  Downloading cycler-0.11.0-py3-none-any.whl (6.4 kB)
Collecting kiwisolver>=1.0.1 (from matplotlib==3.4.3->-r requirements.txt (line 10))
  Downloading kiwisolver-1.4.4-cp38-cp38-manylinux_2_5_x86_64.manylinux1_x86_64.whl (1.2 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 1.2/1.2 MB 2.7 MB/s eta 0:00:00
Collecting pillow>=6.2.0 (from matplotlib==3.4.3->-r requirements.txt (line 10))
  Downloading Pillow-9.5.0-cp38-cp38-manylinux_2_28_x86_64.whl (3.4 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 3.4/3.4 MB 2.9 MB/s eta 0:00:00
Collecting pyparsing>=2.2.1 (from matplotlib==3.4.3->-r requirements.txt (line 10))
  Downloading pyparsing-3.1.0-py3-none-any.whl (102 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 102.6/102.6 kB 719.9 kB/s eta 0:00:00
Collecting python-dateutil>=2.7 (from matplotlib==3.4.3->-r requirements.txt (line 10))
  Downloading python_dateutil-2.8.2-py2.py3-none-any.whl (247 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 247.7/247.7 kB 128.8 kB/s eta 0:00:00
Collecting pytz>=2017.3 (from pandas==1.3.0->-r requirements.txt (line 11))
  Downloading pytz-2023.3-py2.py3-none-any.whl (502 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 502.3/502.3 kB 240.2 kB/s eta 0:00:00
Collecting PyYAML>=3.0 (from flasgger==0.9.5->-r requirements.txt (line 12))
  Downloading PyYAML-6.0-cp38-cp38-manylinux_2_5_x86_64.manylinux1_x86_64.manylinux_2_12_x86_64.manylinux2010_x86_64.whl (701 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 701.2/701.2 kB 345.1 kB/s eta 0:00:00
Collecting jsonschema>=3.0.1 (from flasgger==0.9.5->-r requirements.txt (line 12))
  Downloading jsonschema-4.17.3-py3-none-any.whl (90 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 90.4/90.4 kB 982.0 kB/s eta 0:00:00
Collecting mistune (from flasgger==0.9.5->-r requirements.txt (line 12))
  Downloading mistune-3.0.1-py3-none-any.whl (47 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 48.0/48.0 kB 1.7 MB/s eta 0:00:00
Collecting six>=1.10.0 (from flasgger==0.9.5->-r requirements.txt (line 12))
  Downloading six-1.16.0-py2.py3-none-any.whl (11 kB)
Collecting attrs>=17.4.0 (from jsonschema>=3.0.1->flasgger==0.9.5->-r requirements.txt (line 12))
  Downloading attrs-23.1.0-py3-none-any.whl (61 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 61.2/61.2 kB 611.7 kB/s eta 0:00:00
Collecting importlib-resources>=1.4.0 (from jsonschema>=3.0.1->flasgger==0.9.5->-r requirements.txt (line 12))
  Downloading importlib_resources-5.12.0-py3-none-any.whl (36 kB)
Collecting pkgutil-resolve-name>=1.3.10 (from jsonschema>=3.0.1->flasgger==0.9.5->-r requirements.txt (line 12))
  Downloading pkgutil_resolve_name-1.3.10-py3-none-any.whl (4.7 kB)
Collecting pyrsistent!=0.17.0,!=0.17.1,!=0.17.2,>=0.14.0 (from jsonschema>=3.0.1->flasgger==0.9.5->-r requirements.txt (line 12))
  Downloading pyrsistent-0.19.3-py3-none-any.whl (57 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 57.5/57.5 kB 1.6 MB/s eta 0:00:00
Collecting zipp>=3.1.0 (from importlib-resources>=1.4.0->jsonschema>=3.0.1->flasgger==0.9.5->-r requirements.txt (line 12))
  Downloading zipp-3.15.0-py3-none-any.whl (6.8 kB)
Installing collected packages: pytz, zipp, Werkzeug, threadpoolctl, six, PyYAML, pyrsistent, pyparsing, pkgutil-resolve-name, pillow, 
numpy, mistune, MarkupSafe, kiwisolver, joblib, itsdangerous, gunicorn, cycler, click, attrs, scipy, python-dateutil, Jinja2, importlib-
resources, scikit-learn, pandas, matplotlib, jsonschema, flask, flasgger
Successfully installed Jinja2-3.0.1 MarkupSafe-2.1.1 PyYAML-6.0 Werkzeug-2.0.1 attrs-23.1.0 click-8.1.3 cycler-0.11.0 flasgger-0.9.5 
flask-2.0.2 gunicorn-20.1.0 importlib-resources-5.12.0 itsdangerous-2.0.1 joblib-1.2.0 jsonschema-4.17.3 kiwisolver-1.4.4 
matplotlib-3.4.3 mistune-3.0.1 numpy-1.21.0 pandas-1.3.0 pillow-9.5.0 pkgutil-resolve-name-1.3.10 pyparsing-3.1.0 pyrsistent-0.19.3 
python-dateutil-2.8.2 pytz-2023.3 scikit-learn-0.24.2 scipy-1.7.0 six-1.16.0 threadpoolctl-3.1.0 zipp-3.15.0
[91mWARNING: Running pip as the 'root' user can result in broken permissions and conflicting behaviour with the system package 
manager. It is recommended to use a virtual environment instead: https://pip.pypa.io/warnings/venv
[0mRemoving intermediate container ed8fc6c8a1e1
 ---> 9e5011c8961f
Step 7/7 : CMD ["python", "flask_api.py"]
 ---> Running in fa06a13b9500
Removing intermediate container fa06a13b9500
 ---> 1665bedba94e
Successfully built 1665bedba94e
Successfully tagged money_api:latest
[Pipeline] }
[Pipeline] // dir
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (Push to Docker Hub)
[Pipeline] withCredentials
Masking supported pattern matches of $DOCKER_PASSWORD
[Pipeline] {
[Pipeline] sh
Warning: A secret was passed to "sh" using Groovy String interpolation, which is insecure.
		 Affected argument(s) used the following variable(s): [DOCKER_PASSWORD]
		 See https://jenkins.io/redirect/groovy-string-interpolation for details.
+ docker login -u balusena -p ****
WARNING! Using --password via the CLI is insecure. Use --password-stdin.
WARNING! Your password will be stored unencrypted in /var/lib/jenkins/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credentials-store

Login Succeeded
[Pipeline] sh
+ docker tag money_api:latest balusena/money_api:latest
[Pipeline] sh
+ docker push balusena/money_api:latest
The push refers to repository [docker.io/balusena/money_api]
4b343f5aa9d2: Preparing
fae7aa368f0b: Preparing
e2779ef14bda: Preparing
d86c0aff1fa4: Preparing
601c9269e496: Preparing
ea402ef6d6a8: Preparing
37b14643f733: Preparing
d85b356ec3b5: Preparing
37b14643f733: Waiting
ea402ef6d6a8: Waiting
601c9269e496: Mounted from library/python
d86c0aff1fa4: Mounted from library/python
e2779ef14bda: Pushed
ea402ef6d6a8: Mounted from library/python
37b14643f733: Mounted from library/python
d85b356ec3b5: Mounted from library/python
fae7aa368f0b: Pushed
4b343f5aa9d2: Pushed
latest: digest: sha256:da6be083559a8dad336f86e1dd9428ad8a1a225afb4d18769f39e1c11713306c size: 2004
[Pipeline] }
[Pipeline] // withCredentials
[Pipeline] }
[Pipeline] // stage
[Pipeline] }
[Pipeline] // withEnv
[Pipeline] }
[Pipeline] // node
[Pipeline] End of Pipeline
Finished: SUCCESS
```
## 10.Now go to your DockerHub and verify that the image is pushed into your DockerHub registery:
```
DockerHub Credentials: login

username: docker_username

password: docker_password

balusena                                                Repository	

balusena/money_api
contains:Image | Last pushed: 3 minutes ago             Public


balusena/ubuntu
contains:Image | Last pushed: 2 years ago               Public
```
**Note:** Docker images are automatically pushed to DockerHub using Jenkins CI/CD pipeline configuration.

## 11.Running the money autenticator app using docker
```
ubuntu@balasenapathi:~$ docker run -p 5000:5000 money_api:latest
 * Serving Flask app 'flask_api' (lazy loading)
 * Environment: production
   WARNING: This is a development server. Do not use it in a production deployment.
   Use a production WSGI server instead.
 * Debug mode: off
/usr/local/lib/python3.8/site-packages/sklearn/base.py:310: UserWarning: Trying to unpickle estimator DecisionTreeClassifier from 
version 1.2.2 when using version 0.24.2. This might lead to breaking code or invalid results. Use at your own risk.
  warnings.warn(
/usr/local/lib/python3.8/site-packages/sklearn/base.py:310: UserWarning: Trying to unpickle estimator RandomForestClassifier from 
version 1.2.2 when using version 0.24.2. This might lead to breaking code or invalid results. Use at your own risk.
  warnings.warn(
 * Running on all addresses.
   WARNING: This is a development server. Do not use it in a production deployment.
 * Running on http://172.17.0.2:5000/ (Press CTRL+C to quit)
172.17.0.1 - - [10/Jun/2023 14:48:54] "GET / HTTP/1.1" 200 -
172.17.0.1 - - [10/Jun/2023 14:48:58] "GET /favicon.ico HTTP/1.1" 404 -
172.17.0.1 - - [10/Jun/2023 14:49:05] "GET /apidocs/ HTTP/1.1" 200 -
172.17.0.1 - - [10/Jun/2023 14:49:06] "GET /flasgger_static/swagger-ui.css HTTP/1.1" 200 -
172.17.0.1 - - [10/Jun/2023 14:49:06] "GET /flasgger_static/swagger-ui-bundle.js HTTP/1.1" 200 -
172.17.0.1 - - [10/Jun/2023 14:49:06] "GET /flasgger_static/swagger-ui-standalone-preset.js HTTP/1.1" 200 -
172.17.0.1 - - [10/Jun/2023 14:49:06] "GET /flasgger_static/lib/jquery.min.js HTTP/1.1" 200 -
172.17.0.1 - - [10/Jun/2023 14:49:07] "GET /apispec_1.json HTTP/1.1" 200 -
172.17.0.1 - - [10/Jun/2023 14:49:08] "GET /flasgger_static/favicon-32x32.png HTTP/1.1" 200 -
172.17.0.1 - - [10/Jun/2023 14:49:53] "GET /predict?variance=2&skewness=4&curtosis=2&entropy=1 HTTP/1.1" 200 -
```

