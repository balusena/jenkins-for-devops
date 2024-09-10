# Jenkins and Git Credential Integration

Integrating GitHub credentials into Jenkins allows seamless interaction between Jenkins and Git repositories for automated
builds and deployments. By securely storing GitHub credentials in Jenkins, users can automate cloning, fetching, and pushing
code during job execution without re-entering credentials. The credentials are stored globally, making them available for 
any job, node, or item in Jenkins. This integration simplifies continuous integration (CI) and continuous delivery (CD) 
pipelines by securely managing GitHub authentication across multiple Jenkins projects.

## Register GitHub Credentials in Jenkins

Follow these steps to permanently register your GitHub credentials in Jenkins:

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

