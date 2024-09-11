# Jenkins Basics of Jenkinsfile

## 1.What is Jenkinsfile?

A Jenkinsfile is a script that defines the Jenkins pipeline "as code." It is written in Groovy, and it describes the stages
(jobs) to be executed as part of the pipeline. The Jenkinsfile is stored in the version control system, which enables better
collaboration and traceability. By treating pipelines as code, Jenkinsfile allows for repeatability, versioning, and easier
maintenance.

## 2.Key Features of Jenkinsfile:
- **Declarative Syntax**: Easier to write and understand for simpler pipelines.
- **Scripted Syntax**: Provides greater flexibility and control for advanced pipelines.
- **Version Control**: The Jenkinsfile can be versioned, tracked, and managed alongside the source code, ensuring consistency in the pipeline.

## 3.Basics of Jenkinsfile

### 1.Pipeline Syntax

A Jenkinsfile can be written using two types of syntax:

### Scripted Pipeline
1. First syntax introduced for Jenkins pipelines.
2. Built on the Groovy engine, allowing the entire pipeline to be written as a Groovy script.
3. Offers advanced scripting capabilities and high flexibility, but is harder to start with.
4. No predefined structure, making it difficult to learn but powerful for complex workflows.

- Example:
```
node {
    // Groovy Script ===> Entire configuration can be written
}
```

### Declarative Pipeline
1. A recent addition, designed to make pipelines easier to get started with.
2. Less powerful than scripted pipelines but easier for beginners.
3. Follows a predefined structure, making it easier to write, but requiring attention to syntax.
4. Easy to start, but you need to focus on the correct pipeline structure.

- Example:
```
pipeline {
    agent any
    stages {
        stage("build") {
            steps {
                // build steps
            }
        }
    }
}
```
### 2.Required Fields of Jenkinsfile
1. **pipeline:** The top-level block that defines the Jenkinsfile.
2. **agent:** Specifies where to execute the pipeline (e.g., on specific nodes or clusters like Windows, Linux, cloud, etc.).
3. **stages:** Contains the actual work, with different stages representing different steps in the pipeline.
4. **stage and steps:** Used to define individual stages and their corresponding tasks.

### 3.Jenkinsfile Example (Basic Stages)

- Below is an example of a Jenkinsfile that includes several stages for building a simple pipeline.
```
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
### 4.Jenkinsfile Example (Build with Checkout and Execution)

- This example includes Git checkout, build, and testing stages.
```
pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'devops_balu_github', url: 'https://github.com/balusena/devops_balu_github.git']]])
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
```

### 5.Testing Changes in a Jenkinsfile (Replay Feature)

- To test changes in a Jenkinsfile without committing those changes to the repository, Jenkins offers a Replay feature.

- Replay Steps:
1. Go to the Build history of one of your branches (e.g., Build, Deploy, Test, Release).
2. Select the specific build you want to replay (e.g., #1. Jun 5, 2023 4:20 PM).
3. Click on Replay to adjust the last run Jenkinsfile and rerun it with modifications.

- Example Replay Script:
```
stage('Build') {
    steps {
        echo 'Building the application...'
        script {
            def test = 2 + 2 > 3 ? 'cool' : 'not cool'
            echo test
        }
    }
}
```
**Note:** After running this, check Build #2 to see the changes reflected with the updated Groovy script.

### 6.Restart from Stage
- In Jenkins, you can restart a pipeline from a specific stage if any errors occur.

- Steps to Restart from a Stage:
1. Go to Build #2.
2. Click on Restart from Stage.
3. Choose the stage from which you want to restart (e.g., Build, Deploy, Test, Release).
4. Click Run to restart the pipeline from the selected stage.

**Note:** If a stage fails, you can restart only that stage without impacting the others in the pipeline.

















