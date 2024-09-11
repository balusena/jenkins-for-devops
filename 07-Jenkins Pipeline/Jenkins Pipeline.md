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

## 10.Using Credentials with Wrapper Syntax in Jenkinsfile

### 1.Localizing Credentials to Specific Stages

When you need to use credentials only within a specific build stage, you can use the `withCredentials` method. This approach
confines the use of credentials to a particular stage, making it more secure and easier to manage.

### 2.Example Jenkinsfile with Wrapper Syntax:

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
                withCredentials([usernamePassword(credentialsId: 'server-credentials', usernameVariable: 'USER', passwordVariable: 'PWD')]) {
                    sh "some script ${USER} ${PWD}"
                }
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
- **withCredentials Block:** This block is used to bind credentials to local environment variables (USER and PWD in this case)
  for the duration of the block. It ensures that credentials are only exposed within the specific stage.
- **usernamePassword Method:** Defines the type of credentials (username and password) and maps them to environment variables.
  The credentialsId refers to the ID of the credentials stored in Jenkins.

### 4.Required Plugins
**To use credentials in Jenkins, you need to install the following plugins:**
- **Credentials Plugin:** Allows you to store credentials in Jenkins.
- **Credentials Binding Plugin:** Enables binding of credentials to environment variables for use in build steps.

## 11.Using Tools Attribute for Making Build Tools Available

The `tools` attribute in a Jenkinsfile provides access to build tools necessary for your project. For example, if you have
frontend and backend tools such as Maven, Gradle, and JDK for Java projects, or npm and Yarn for JavaScript projects, you 
need these tools available in Jenkins.

**Supported Build Tools:**
- **Gradle**
- **Maven**
- **JDK**

If you need other build tools, you must install and configure them according to the required versions for your project.

**Note:** You use these tools in your Jenkinsfile by specifying their names as configured in Jenkins.

### 1.Example Jenkinsfile

```
pipeline {
    agent any
    
    tools {
        maven 'Maven'
        gradle 'Gradle'
        jdk 'JDK'
    }

    stages {
        stage('Build') {
            steps {
                echo 'Building the application'
                sh "mvn install"
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying the application'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing the application'
            }
        }
        stage('Release') {
            steps {
                echo 'Releasing the application'
            }
        }
    }
}
```
**Note:** Ensure that the tools are pre-installed in Jenkins.

### 2.To check:
- Go to Manage Jenkins.
- Click on Global Tool Configuration.
- Verify the installation of the required tools.

Use the name of the tool installation in Jenkins and reference it in the Jenkinsfile.

## 12.Using Parameters for a Parameterized Build

Parameters in Jenkins allow you to provide external configurations to your build process. This is useful for scenarios 
where you need to change the behavior of your build, such as deploying a specific version of an application to a staging
server.

### 1.Types of Parameters

- **string(name, defaultValue, description)**: Allows users to input a string value.
- **choice(name, choices, description)**: Provides a dropdown list of options for the user to select from.
- **booleanParam(name, defaultValue, description)**: Provides a checkbox for users to select true or false, useful for conditional steps.

### 2.Example Jenkinsfile

```
pipeline {
    agent any
    
    parameters {
        string(name: 'VERSION', defaultValue: '', description: 'Version to deploy on prod')
        choice(name: 'VERSION', choices: ['1.1.0', '1.2.0', '1.3.0'], description: 'Version choice to use in prod')
        booleanParam(name: 'executeTests', defaultValue: true, description: 'Skip the Test Stage')  // If false, tests are skipped
    }

    stages {
        stage('Build') {
            steps {
                echo 'Building the application'
                sh "mvn install"
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying the application'
                echo "Deploying version ${params.VERSION}"
            }
        }
        stage('Test') {
            when {
                expression {
                    params.executeTests == true
                }
            }
            steps {
                echo 'Testing the application'
            }
        }
        stage('Release') {
            steps {
                echo 'Releasing the application'
            }
        }
    }
}
```
### 3.Usage:
After saving and committing the Jenkinsfile, go to your pipeline job and click on Build Now. You will see the option to 
Build with Parameters.

- Select the VERSION from the dropdown list.
- Check or uncheck the executeTests checkbox to determine if tests should be run.
- Click on Build to start the pipeline with the specified parameters.

### 4.Dashboard Navigation:

- Go to my-pipeline.
- Select main branch.
- Click Build with Parameters.

**Note:** Parameters can be applied to all branches used in your Jenkins pipeline.

## 13.Using External Groovy Script

Imagine a scenario where you have multiple pipeline steps—building the front end, running tests, building the backend, 
creating a Docker image, and pushing the repository. With many stages and extensive logic, your Jenkinsfile can become 
cluttered. To manage this, you can extract the Groovy scripts into an external file, which helps keep your Jenkinsfile 
clean and maintainable.

### 1.Groovy Script: `script.groovy`

```
def buildApp() {
    echo 'building the application'
}

def deployApp() {
    echo 'deploying the application'
    echo "deploying version ${params.VERSION}"
}

def testApp() {
    echo 'testing the application'
}    

def releaseApp() {
    echo 'releasing the application'
}    

return this
```

### 2.Jenkinsfile: Jenkinsfile

```
def gv

pipeline {
    agent any
    parameters {
        string(name: 'VERSION', defaultValue: '', description: 'version to deploy on prod')
        choice(name: 'VERSION', choices: ['1.1.0', '1.2.0', '1.3.0'], description: 'version_choice to use in prod')
        booleanParam(name: 'executeTests', defaultValue: true, description: 'Skip the Test Stage') // If set to false, it gets skipped
    }
    stages {
        stage('Init') {
            steps {
                script {
                    gv = load "script.groovy"
                }
            }
        }       
        stage('Build') {
            steps { 
                script {
                    gv.buildApp()
                }
            }
        }   
        stage('Deploy') {
            steps {
                script {
                    gv.deployApp()
                }   
            }
        }
        stage('Test') {
            when {
                expression {
                    params.executeTests == true
                }
            }
            steps {
                script {
                    gv.testApp()
                }
            }
        }
        stage('Release') {
            steps {
                script {
                    gv.releaseApp()
                }
            }
        }
    }
}
```
### 3.Explanation
- **withCredentials Block:** Used to bind credentials to local environment variables (USER and PWD) for the duration of 
  the block. This ensures that credentials are only exposed within the specific stage.
- **usernamePassword Method:** Defines the type of credentials (username and password) and maps them to environment 
  variables. The credentialsId refers to the ID of the credentials stored in Jenkins.

### 4.Required Plugins
To use credentials in Jenkins, you need to install the following plugins:

- **Credentials Plugin:** Allows you to store credentials in Jenkins.
- **Credentials Binding Plugin:** Enables binding of credentials to environment variables for use in build steps.

**Note:** First, create credentials in the Jenkins GUI using the Credentials Plugin. Then, use the Credentials Binding 
Plugin to bind these credentials to environment variables in your Jenkinsfile.















