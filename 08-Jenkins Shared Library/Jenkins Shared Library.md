# Jenkins Shared Library

## 1.Shared Library in Jenkins|Re-Usability in jenkins pipeline(Beginners):

### 1.Why we need Jenkins Shared Library?
A Jenkins Shared Library is a concept where common pipeline code is stored in a version control system and can be used by
any number of pipelines simply by referencing it. This approach allows multiple teams to utilize the same library across 
their pipelines.

### 2.Example
Consider a scenario where you have ten Java microservices pipelines. If each pipeline includes a Maven build step, this 
step will be duplicated across all ten pipelines. Whenever a new service is added, you would need to copy and paste the 
pipeline code into the new pipeline. Additionally, if you need to change a parameter in the Maven build step, you would 
have to update each pipeline manually.

By using a shared library, you can define the Maven build step once in the shared library and reference it in each 
pipeline. This reduces duplication, simplifies maintenance, and ensures consistency across all pipelines.

### 3.Folder structure for shared library:
```
jenkins-shared-library
├── vars
│     
├── src
│   
└── resources
``` 
- **`vars`**: Contains global shared library code callable from a pipeline. Files have a `.groovy` extension.
- **`src`**: Regular Java source directory included in the classpath during script compilation. You can add custom Groovy code here to extend your shared library.
- **`resources`**: Stores non-Groovy files (e.g., XML, JSON) required for your pipelines.

- **Scenario:** After the maven build if specified word found console, for specified number of times or more, then set build to unstable

**Demo:** Create a repo called shared-library in SCM(GitHub)

### 4.create a groovy script named "filterLogs.groovy" in vars folder ===> shared-library/vars/filterLogs.groovy
```
#!/usr/bin/env groovy

import org.apache.commons.lang.StringUtils

def call(String filter_string, int occurrence) {
    def logs = currentBuild.rawBuild.getLog(10000).join('\n')
    int count = StringUtils.countMatches(logs, filter_string);
    if (count > occurrence -1) {
        currentBuild.result='UNSTABLE'
    }
}
```
### 5.Global Pipeline Libraries configuration for shared-library setup:
```
Now go to Dasboard ---> Mange Jenkins ----> System

Global Pipeline Libraries?

Sharable libraries available to any Pipeline jobs running on this system. These libraries will be trusted, meaning they run without 
“sandbox” restrictions and may use @Grab.

Library?

Name?
shared-library ===> your SCM(GitHub) repo name used to store shared-library files
You must enter a name.

Default version?

main ===> provide your working branch name

[ ]Load implicitly?

[/]Allow default version to be overridden?

[/]Include @Library changes in job recent changes?

[ ]Cache fetched versions on controller for quick retrieval?

Retrieval method?
[/]Modern SCM
Loads a library from an SCM plugin using newer interfaces optimized for this purpose. The recommended option when available.

Source Code Management
[/]Git

Project Repository?
https://github.com/balusena/devops_balu_github.git

Credentials?
github_username/github_password(My Github Credentials)

Behaviours?
Discover branches?
Discovers branches on the repository.
(from Git plugin)

Fresh clone per build?
If checked, every build performs a fresh clone of the SCM rather than locking and updating a common copy. No changelog will be computed. 
For Git, you are advised to add Advanced clone behaviors and then check Shallow clone and Honor refspec on initial clone and uncheck 
Fetch tags to make the clone much faster. You may still enable Cache fetched versions on controller for quick retrieval if you prefer.
(from Pipeline: Groovy Libraries)

Library Path (optional)?
A relative path from the root of the SCM to the root of the library. Leave this field blank if the root of the library is the root of 
the SCM. Note that ".." is not permitted as a path component to avoid security issues.

[Save]|[Apply]
```
### 6.Create a jenkinsfile 
```
#!/usr/bin/env groovy

@Library('shared-library@master') _ //master or whatever branch

pipeline{

      agent {
                docker {
                image 'maven:3-openjdk-11'
                args '-v $HOME/.m2:/root/.m2'
                }
            }
        
        stages{

              stage('maven build'){
                  steps{
                      script{
		    	                sh "mvn clean install"
                      	  }
               	     }  
                 }	
                 
                 stage ('Check logs') {
                    steps {
                        filterLogs ('WARNING', 50)
                    }
                }
		
           }	       	     	         
}
```
### 7.Run the Pipeline
Create a Jenkins pipeline project named shared-library-pipeline and run the build. The build will finish with an UNSTABLE
status if the condition is met. You can use the Replay option to modify and test the Groovy script without committing 
changes to the SCM.

### 8.Use Case
Shared libraries improve reusability and maintainability. Instead of modifying the Jenkinsfile for each pipeline, you can
update the shared library and apply changes across all pipelines. This is particularly useful in development environments
for testing and refining pipeline logic.

## 2.Shared Library in Jenkins|Re-Usability in jenkins pipeline(Advanced):

### 1.Agenda:
- Jenkins Shared Library
- Closures
- Maven Build
- Maven Deploy

### 2.Folder Structure:
```
+---src                           # Groovy source files
|   └── MavenBuilder.groovy       # For building maven files class
+---vars
|   ├── config.groovy             # For global 'config' variable to read config.yml configurations
|   ├── filterLogs.groovy         # 'filterLogs' variable to get the console output and check a condition string
|   └── config.txt                # Help for 'config' variable
+---resources                     # Resource files (external libraries only)
    └── globalconfig.yml          # Static file which contains all the global configuration
```
- Shared Library in Jenkins | Re-Usability in Jenkins Pipeline (Advanced)

1. **The vars directory** contains the `greeting.groovy` file with two functions, `sayHello()` and `sayGoodbye()`.

2. **The src directory** contains the `StringUtils.groovy` file, defining a utility class with a static method to reverse a string.

3. **The resources directory** contains the `config.properties` file, which can be used as a resource file.

4. **The Jenkinsfile** is the pipeline script that uses the shared library.

5. **The pipeline has three stages**:

   - **Greeting**: Uses the `greeting` variable from the shared library to call the `sayHello()` and `sayGoodbye()` functions.
   
   - **StringUtils**: Demonstrates the usage of the `StringUtils` class from the shared library's `src` directory.
   
   - **Resource File**: Shows how to access a resource file (`config.properties`) using the `libraryResource()` function.

### 3.Directory Structure:

```
jenkins-shared-library
+-- vars
|   +-- greeting.groovy
+-- src
|   +-- com
|       +-- example
|           +-- utils
|               +-- StringUtils.groovy
+-- resources
|   +-- config.properties
+-- Jenkinsfile
```
1. **vars/greeting.groovy: Create Groovy Script**
```
def sayHello(String name) {
  echo "Hello, ${name}!"
}

def sayGoodbye(String name) {
  echo "Goodbye, ${name}!"
}
```
2. **src/com/example/utils/StringUtils.groovy: Create Groovy Script**
```
package com.example.utils

class StringUtils {
  static String reverse(String input) {
    return input.reverse()
  }
}
```
3. **resources/config.properties: Create a File**
```
key=value
```
4. Jenkinsfile: Create a Groovy Script
```
@Library('jenkins-shared-library') _

pipeline {
  agent any
  stages {
    stage('Greeting') {
      steps {
        script {
          greeting.sayHello("John")
          greeting.sayGoodbye("Jane")
        }
      }
    }
    stage('StringUtils') {
      steps {
        script {
          def reversed = com.example.utils.StringUtils.reverse("Hello")
          echo "Reversed string: ${reversed}"
        }
      }
    }
    stage('Resource File') {
      steps {
        script {
          def configFile = libraryResource('config.properties')
          def config = readProperties file: configFile
          echo "Value from config.properties: ${config.key}"
        }
      }
    }
  }
}
```

























  

