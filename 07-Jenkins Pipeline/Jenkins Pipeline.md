# Jenkins Pipeline

## 1.What is Jenkinsfile?

A Jenkinsfile is a script that defines the Jenkins pipeline "as code." It is written in Groovy, and it describes the stages
(jobs) to be executed as part of the pipeline. The Jenkinsfile is stored in the version control system, which enables better
collaboration and traceability. By treating pipelines as code, Jenkinsfile allows for repeatability, versioning, and easier
maintenance.

## 2.Key Features of Jenkinsfile:
- **Declarative Syntax**: Easier to write and understand for simpler pipelines.
- **Scripted Syntax**: Provides greater flexibility and control for advanced pipelines.
- **Version Control**: The Jenkinsfile can be versioned, tracked, and managed alongside the source code, ensuring consistency in the pipeline.

**Note:** We will create a Jenkinsfile in our SCM(GitHub) Repository with code.

## 3.From Scripted to Declarative Pipeline Syntax

- Basic Structure:

BiUld code in GitHub(SCM) ===> */main ---> devops_balu_github/jenkinsfile
```
pipeline {
    
    agent any

    stages {
        
        stage("build") {
            
            steps {
                
            }
        }
    }
}      
```
## 4.Basic Structure of Jenkinsfile

- Pipeline Syntax:

**Note:** A Jenkinsfile can be written using either Scripted Pipeline or Declarative Pipeline.

### Scripted Pipeline vs. Declarative Pipeline

|   Scripted Pipeline                                                | Declarative Pipeline                                              |
| -------------------------------------------------------------------| ----------------------------------------------------------------- |
| 1. First syntax introduced                                         | 1. More recent addition                                           |
| 2. Uses the Groovy engine                                          | 2. Easier to get started, but less powerful                       |
| 3. Allows for advanced scripting capabilities and high flexibility | 3. Has a pre-defined structure                                    |
| 4. More difficult to start                                         | 4. Easier to start, but requires attention to pipeline structure  |
| 5. No pre-defined structure                                        | 5. Has a pre-defined structure                                    |

- **Scripted Pipeline Example:**
```
node {
    // Groovy Script
    // Entire configuration is written here
}
```

- **Declarative Pipeline Example:**
```
pipeline {
    agent any

    stages {
        stage("build") {
            steps {
                // Steps to execute in the build stage
            }
        }
    }
}
```
## 5.Required fields of Jenkinsfile

- "pipeline" must be top level

- "agent" where to execute here we are specifying the different nodes or clusters to be used such as windows, linux , cloud etc

- "stages" where the actual work happens for different stages used in the pipeline

- "stage"-"steps" here we define different stage and steps that are used in the stages for the pipeline used to build
```
pipeline {
    
    agent any

    stages {
        
        stage('Build') {
            steps {
                echo 'building the application'
            }
        }
        stage('Deploy') {
            steps {
                echo 'deploying the application'
            }
        }
        stage('Test') {
            steps {
                echo 'testing the application'
            }
        }
        stage('Release') {
            steps {
                echo 'releasing the application'
            }
        }
    }
}
```

**Note:** The script we write in step will actually execute or run in Jenkins server/Jenkins agent

- Build a Multibranch Pipeline in Jenkins with GitHub Integration

To build a Declarative pipeline in Jenkins using a Jenkinsfile stored in a GitHub repository, follow these steps:

1. **Save the Jenkinsfile**: First, save your Jenkinsfile in the GitHub repository. Commit the changes so that Jenkins can
     access the Jenkinsfile from the SCM (GitHub).

2. **Configure Jenkins Multibranch Pipeline**:
   - Go to Jenkins and create a new **Multibranch Pipeline** project.
   - Under **Build Configuration**, provide your GitHub credentials to connect Jenkins with your repository.

3. **Build Process in Jenkins**:

   - **Step 1: Checkout Code**:
     - Jenkins automatically checks out the code from the **SCM (GitHub)** repository, as defined in the **Build Sources**
       configuration. This step is implicitly handled by Jenkins when it pulls the code from the repository using your 
       GitHub credentials.

   - **Step 2: Build Configuration**:
     - Jenkins uses the **Jenkinsfile** from the repository to configure the pipeline.
     - Set the **Mode** to **by Jenkinsfile**.
     - In the **Script Path**, specify the path to the Jenkinsfile (usually the root of the repository), for example, `Jenkinsfile`.

4. **Automatic Build**:
   Once the Jenkinsfile is properly configured and integrated with Jenkins, the server will automatically carry out all the
   necessary steps in the pipeline, such as build, test, and deploy. These processes are defined within the Jenkinsfile, 
   and Jenkins uses this file to orchestrate the entire CI/CD process.

5. **GitHub and Jenkins Integration**:
   The pipeline is built by integrating both Jenkins and GitHub, with the credentials provided, ensuring that Jenkins can
   access and execute the pipeline defined in the Jenkinsfile stored in GitHub.

By following these steps, you will have successfully built a Multibranch Pipeline in Jenkins, integrated with GitHub using
the Jenkinsfile for automation.
  
## 6.Post Attribute in Jenkinsfile

The **post** section defines one or more additional steps that are executed after the completion of a Pipeline or a stage's run.

### 1.Purpose:

- **Post** is used to execute logic after all stages are completed.
- It ensures certain actions are taken regardless of the pipeline's success or failure.

### 2.Conditions in the Post Section:

- `always`: Always runs, no matter the outcome of the pipeline.
- `success`: Executes only if the pipeline succeeds.
- `failure`: Executes only if the pipeline fails.

**Note:** The post section is typically used to notify users about build statuses or status changes.

### 3.Example Jenkinsfile with Post Section:

```
pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'building the application'
            }
        }
        stage('Deploy') {
            steps {
                echo 'deploying the application'
            }
        }
        stage('Test') {
            steps {
                echo 'testing the application'
            }
        }
        stage('Release') {
            steps {
                echo 'releasing the application'
            }
        }
    }

    post {
        always {
            echo 'This will always run regardless of the result.'
        }

        success {
            echo 'Pipeline completed successfully.'
        }

        failure {
            echo 'Pipeline failed.'
        }
    }
}
```
**Note:**
- The conditions defined in the post section will always execute, whether the pipeline succeeds or fails.
- It is commonly used to post messages or communicate build status changes to users or systems.

## 7.Define Conditionals/When Expression

**Use Case:** If you want to run a build only on specific branches (e.g., `dev` branch) and skip the build for others 
(e.g., `pre-prod` or `prod` branches), you can achieve this using the `when` directive in your Jenkinsfile. 

The `when` directive allows you to conditionally execute stages based on environment variables or other conditions. Jenkins
provides environment variables like the current branch name, which you can use to control execution. 

### 1.Conditional Execution with `when`:

- **Expression:** Use expressions to define conditions for running stages. 
- **Groovy Script:** You can also use Groovy scripts to evaluate complex conditions.

### 2.Example Jenkinsfile:

```
CODE_CHANGES = getGitChanges()

pipeline {
    agent any

    stages {
        stage('Build') {
            when {
                expression {
                    // Condition to check if the branch is 'dev' and CODE_CHANGES is true
                    BRANCH_NAME == 'dev' && CODE_CHANGES == true
                }
            }
            steps {
                echo 'building the application'
            }
        }
        stage('Deploy') {
            steps {
                echo 'deploying the application'
            }
        }
        stage('Test') {
            steps {
                echo 'testing the application'
            }
        }
        stage('Release') {
            steps {
                echo 'releasing the application'
            }
        }
    }
}
```
### 3.Explanation

- **`when` Directive:** Used to define the conditions under which the stage will execute.
- **Expression Example:** In the `Build` stage, the `when` directive checks if the `BRANCH_NAME` is `'dev'` and if `CODE_CHANGES` is `true`.
- **Groovy Script:** `CODE_CHANGES = getGitChanges()` is used to determine if there were code changes. This script should be defined to return `true` or `false` based on the presence of changes.

**Note:**

- **Environment Variables:** You can use Jenkins environment variables to dynamically control the execution of stages based on branch names or other parameters.
- **Complex Conditions:** Use Groovy scripts to handle more complex logic and decisions.

## 8.Using Environmental Variables in Jenkinsfile

### 1.What Variables Are Available?

Jenkins provides a range of default environment variables which can be viewed at: [Jenkins Environment Variables]
(http://127.0.0.1:8080/env-vars.html). These variables are available for shell and batch build steps and can be utilized
in our Jenkinsfile.

### 2.Defining Custom Environment Variables

In addition to the default variables, you can define your own custom environment variables within the Jenkinsfile. This 
is done using the `environment` block. Variables defined here are accessible throughout all stages of the pipeline.

**Note:** To use an environment variable in your script, you should use the Groovy syntax with `${}`. For ordinary strings,
          you can use single quotes `''`.

### 3.Example Jenkinsfile:

```
pipeline {
    agent any
    
    environment {
        NEW_VERSION = '1.3.0'
    }

    stages {
        stage('Build') {
            steps {
                echo 'building the application'
                
                // Groovy syntax for using variables
                echo "building version ${NEW_VERSION} ---> This is written in Groovy syntax where NEW_VERSION is used as a variable"
                
                // Ordinary string representation
                echo 'building version ${NEW_VERSION} ---> This is an ordinary representation where NEW_VERSION is not used as a variable'
            }
        }
        stage('Deploy') {
            steps {
                echo 'deploying the application'
            }
        }
        stage('Test') {
            steps {
                echo 'testing the application'
            }
        }
        stage('Release') {
            steps {
                echo 'releasing the application'
            }
        }
    }
}
```
### 4.Explanation:
- **Environment Block:** Used to define environment variables that will be available to all stages in the pipeline.
- **Groovy Syntax:** ${VARIABLE_NAME} is used to reference environment variables within a string in Groovy.
- **Ordinary String Representation:** 'variable_name' is treated as a regular string without variable interpolation

## 9.Using Credentials in Jenkinsfile

### 1.Use Case

If you have a stage in your Jenkins pipeline that deploys a newly built application to a development server, you will need
credentials to connect to the server and perform actions such as copying the build artifact. 

- Here’s how you can use credentials in Jenkinsfile:

1. **Define Credentials in Jenkins GUI:** 
   - Go to the Jenkins GUI and define your credentials. For example, you might have "My GitHub Credentials" with a username and password.
   - Assign an ID to these credentials, such as `server-credentials`.

2. **Bind Credentials to Environment Variable:**
   - Use the `credentials` function to bind these credentials to an environment variable in your Jenkinsfile.
   - Example: `SERVER_CREDENTIALS = credentials('server-credentials')`.

3. **Install and Use the "Credentials Binding" Plugin:**
   - Ensure that the "Credentials Binding" plugin is installed. This plugin allows you to use the credentials stored in Jenkins as environment variables.

**Note:** The credential environment variable `SERVER_CREDENTIALS` is globally scoped and can be used across different stages in your pipeline.

### 2.Example Jenkinsfile:

```
pipeline {
    agent any
    
    environment {
        NEW_VERSION = '1.3.0'
        SERVER_CREDENTIALS = credentials('server-credentials')
    }

    stages {
        stage('Build') {
            steps {
                echo 'building the application'
                
                // Groovy syntax for using variables
                echo "building version ${NEW_VERSION} ---> This is written in Groovy syntax where NEW_VERSION is used as a variable"
                
                // Ordinary string representation
                echo 'building version ${NEW_VERSION} ---> This is an ordinary representation where NEW_VERSION is not used as a variable'
            }
        }
        stage('Deploy') {
            steps {
                echo 'deploying the application'
                
                // Use the credentials variable
                echo "deploying with ${SERVER_CREDENTIALS}"
                sh "${SERVER_CREDENTIALS}"
            }
        }
        stage('Test') {
            steps {
                echo 'testing the application'
            }
        }
        stage('Release') {
            steps {
                echo 'releasing the application'
            }
        }
    }
}
```
### 3.Explanation:
- **Environment Block:** Defines global environment variables for the pipeline, including credentials.
- **Credentials Binding:** SERVER_CREDENTIALS = credentials('server-credentials') binds the credentials to the environment variable.
- **Usage in Stages:** The credentials can be used in different stages of the pipeline, such as in the Deploy stage, where 
    SERVER_CREDENTIALS is referenced to perform actions that require authentication.




















