# Jenkins Email Notification Setup

## 1.Prerequisites
1. **Jenkins Email Extension Plugin**: Ensure that the "Email Extension" plugin is installed.
   - Go to **Manage Jenkins** -> **Manage Plugins** -> **Available** tab.
   - Search for **"Email Extension"** and install it.
  
2. **SMTP Server Configuration**: Access to an SMTP server (e.g., Gmail, Office 365, or company’s mail server) is required to send emails.

## 2.Step-by-Step Guide

### 1. Configure SMTP in Jenkins

1. **Go to "Manage Jenkins" > "Configure System"**:
   - Scroll to the **"E-mail Notification"** section.
   
2. **SMTP Server Setup**:
   - **SMTP Server**: Enter your SMTP server (e.g., `smtp.gmail.com` for Gmail).
   - **Default user E-mail suffix**: (Optional) Add a default email domain, such as `@gmail.com` or `@company.com`.

3. **Advanced Settings**:
   - Click **"Advanced"**.
   - **SMTP Port**: Enter the port number (`587` for Gmail/Office 365).
   - **Use SMTP Authentication**: Check if required.
     - **User Name**: Enter your email address (e.g., `example@gmail.com`).
     - **Password**: Enter your email password (or App Password if using Gmail).
   - **Use SSL**: Check this box if your SMTP server requires SSL (e.g., Gmail).
   - **SMTP Timeout**: Leave default unless experiencing timeout issues.

4. **Test Email**:
   - Enter an email in the "Test E-mail Recipient" field.
   - Click **"Test Configuration by sending test e-mail"** and verify that you received the test email.

### 2. Set Up Email Notification for a Specific Job

1. **Open Job Configuration**:
   - Navigate to the Jenkins job and click **"Configure"**.

2. **Post-Build Actions**:
   - Scroll to **"Post-build Actions"**.
   - Click **"Add post-build action"** and choose **"Editable Email Notification"**.

3. **Email Recipients**:
   - **Project Recipient List**: Enter email addresses (comma-separated for multiple recipients).

4. **Trigger Conditions**:
   - Expand the "Advanced Settings".
   - Under **Triggers**, select:
     - **"Failure"**: Sends an email if the build fails.
     - **"Success"**: Sends an email if the build succeeds.
   - You can also select other options like "Unstable", "Always", or "First failure".

5. **Configure Content**:
   - Customize the subject and body of the email notification (Optional):
     - **Subject Example**: `Build ${BUILD_STATUS} - ${JOB_NAME}`
     - **Body Example**: 
       ```
       Build ${BUILD_STATUS}: Job ${JOB_NAME} (#${BUILD_NUMBER})
       ```

6. **Save**: Click **"Save"** to apply the job configuration.

### 3. Jenkins Pipeline Example for Email Notifications

For Jenkins Pipeline jobs, use the following configuration in your `Jenkinsfile`:

```
pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                // Simulate build process
                echo 'Building...'
            }
        }
    }

    post {
        success {
            emailext (
                subject: "SUCCESS: Job '${JOB_NAME} [${BUILD_NUMBER}]'",
                body: """Job '${JOB_NAME} [${BUILD_NUMBER}]' succeeded.
                         Check console output at ${BUILD_URL}""",
                to: 'recipient@example.com'
            )
        }
        failure {
            emailext (
                subject: "FAILED: Job '${JOB_NAME} [${BUILD_NUMBER}]'",
                body: """Job '${JOB_NAME} [${BUILD_NUMBER}]' failed.
                         Check console output at ${BUILD_URL}""",
                to: 'recipient@example.com'
            )
        }
    }
}
```