
# CI/CD

Jenkins is used to automate the deployment to test and production environments. When a new commit is pushed to github, the images are built and containers will be created on test environment. If a tag is pushed to github, a release is automatically triggered and the new version is started on production environment

### Install Jenkins
1. Transfer docker-compose and Dockerfile for jenkins to the server
2. Run `docker compose up -d --build to install Jenkins`

### How to set up Jenkins
1. Create a freestyle project
2. In "General section", choose GitHub Project and paste the url of the repo
3. In "Source Code Management", choose Git
4. In credentials, create one if you don't have any. The type is "Username and Password", get the password from github access token
5. For production environment, click advanced and paste "+refs/tags/*:refs/remotes/origin/tags/*" into Refspec. Next, paste "refs/tags/*" into Branch Specifier.
6. For test environment, you can ignore the advance setting and type "*/main" into Branch Specifier
7. In Build Triggers, choose "Github hook trigger for GITScm polling"
8. In Build Environment, set VARIABLE to "SECRET_FILE_PATH" and credential create a secret file and upload "server.env" or "frontend.env" to it
9. In Build Steps, choose Execute Shell and paste the content inside Jenkinsfile in the repo to it. For test environment, you only need 
`cp ${SECRET_FILE_PATH} /var/jenkins_home/workspace/${JOB_NAME} 
docker compose up -d --build`

### How to set up GitHub Webhook
1. Click on the setting in your repo, go to webhook. Paste "http://ip:8080/github-webhook/" to the payload url
2. For test environment, it does not allow inbound connections. Therefore, we need to use Smee and use the ip address of Smee