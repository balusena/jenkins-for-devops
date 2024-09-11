# Jenkins Automatic Build Triggers

## 1.Trigger Jenkins Build Automatically: (Git Integration)

When changes are made to the repository (`devops_balu_github`), manually scanning the Multibranch Pipeline is not ideal.
Instead, Jenkins should automatically trigger builds. This can be achieved through two primary methods: 
**Push Notification** and **Polling for Changes**.

## 2.Jenkins and GitHub Integration Workflow

- **User** → Commits changes to **Git Repository** → Jenkins triggers build automatically.

## 3.Trigger Methods

1. **Push Notification**: Version control (GitHub) notifies Jenkins of new commit changes.
2. **Polling for Changes**: Jenkins polls the repository at regular intervals to detect changes.

**Note:** Push notifications are more efficient than polling because they communicate updates only when changes occur, 
unlike polling, which checks for changes continuously at set intervals.

## 4.Push Notification (Using Webhooks)

To set up **Push Notifications**, we configure both Jenkins and the SCM (GitHub) to work together.

### 1.Jenkins Setup for GitHub Push Notifications

1. **Install Jenkins Plugin**: Install the relevant plugin based on your SCM (e.g., GitHub).
2. **Configure Repository Server Hostname**.
3. **Access Token or Credentials**: Add your GitHub credentials (username, password, or access token).

### 2.Jenkins Configuration for GitHub Webhooks

1. Go to **Manage Jenkins** → **Configure System**.
2. Under **GitHub** → **GitHub Servers**, click **Add GitHub Server**.
3. Select **Advanced** and configure the following:
   - **Override Hook URL**: Check this and enter the URL for your Jenkins webhook (e.g., `http://127.0.0.1:8080/github-webhook/`).
   - **Shared Secrets**: Add any shared secrets (optional but enhances security).
4. Click **Save**.

**Note:** This step connects Jenkins to GitHub using webhooks.

### 3.GitHub Setup for Webhook URL

1. Go to **GitHub** → Your Repository (`balusena/devops_balu_github`).
2. Go to **Settings** → **Code and automation** → **Webhooks** → **Add Webhook**.
3. Configure the webhook with the following details:
   - **Payload URL**: Set this to your Jenkins webhook URL (e.g., `http://127.0.0.1:8080/github-webhook/`).
   - **Content Type**: Choose either `application/json` or `application/x-www-form-urlencoded`.
   - **Secret**: Add a secret token to secure your webhook.
   - **Events**: Choose **Just the push event** to trigger builds on new commits.
4. Click **Add Webhook**.

**Note**: Use the actual Jenkins server URL in the Payload URL, not a localhost address.

## 5.Polling for Changes

In this method, Jenkins polls the repository at regular intervals to detect any changes.

### 1.Jenkins Polling Setup

1. Go to your Jenkins job (e.g., `my-pipeline`).
2. Click **Configure**.
3. Under **Scan Repository Triggers**, select **Periodically if not otherwise run**.
4. Set the **Polling Interval** (e.g., every 1 minute, 15 minutes, or 30 minutes).
5. Click **Save**.

Once configured, Jenkins will automatically scan the repository at the defined intervals for changes and trigger builds accordingly.

### 2.Example of Polling Workflow

- If changes are detected in the GitHub repository:
  - Jenkins polls every 1 minute → Detects changes → Automatically triggers the build.
  
**Note**: The typical interval is set between 15-30 minutes.

## 6.Common Practice and Backup Strategy

Sometimes, **webhooks** or **push notifications** may fail, often due to Jenkins being unavailable or firewall restrictions.
In such cases, it's a good practice to have **both** push notifications and polling enabled as a backup.

### Good Strategy:
- Configure both **Push Notifications** and **Polling**.
- Set polling to run every couple of hours as a backup, in case webhooks fail.

## 7.What is the Difference Between Webhooks and Polling in Jenkins?

- **Polling**: Requests are initiated by the client (Jenkins), and it checks for changes at regular intervals, regardless of whether there are new commits.
- **Webhooks**: Requests are initiated by the server (GitHub), and builds are automatically triggered whenever a new event (like a commit) occurs.

Webhooks are event-driven and more efficient, while polling consumes more resources by continuously checking for changes.

