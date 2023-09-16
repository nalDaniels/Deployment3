# Purpose:
To familiarize myself with setting up a Jenkins server on an EC2 and deploying to AWS Elastic Beanstalk using the command line rather than the graphical user interface. I also passed along the application files that were successfully built and tested in Jenkins to AWS EB instead of manually uploading files from the repository. Using AWS Elastic Beanstalk helps me understand what types of infrastructure are required to run an application. Also, this deployment demonstrated the purpose of setting up a github web hook to trigger an event after making commits to the github repository. 

## Steps to Production
### 1. Provision a Jenkins Server
#### a. Goal
Create Jenkins server in our EC2 instance to execute a multibranch pipeline. 
#### b. Process
I ran a script to install Jenkins, then installed python3.10-venv to create a virtual environment, pip, the python package manager, and unzip, to decompress files and directories. 
#### c. Optimization
On Jenkins, I created a multibranch pipeline, which would allow Jenkins to build, test, and deploy all branches in the repository. This would be beneficial for building and testing new feature code in different branches and deploying previous versions if testing fails on newer versions. 
#### d. Console Output
Jenkins used git to clone the repository and fetch the latest commits. The build stage created a virtual environment using python3, activated that test environment, installed python's package manager and all of the packages in the requirements.txt that are necessary for the application to run. Then, it told Flask that the application code was in the application.py file. The test stage activated the test environment, executed tests, and saved the results in test-reports/results.xml

### 2. Configure AWS Elastic Beanstalk Command Line Interface
#### a. Goal
To use the command line to set up and manage AWS resources and deploy an application.
#### b. Process
I created an access key to enable my EC2 terminal to interact with AWS resources. I downloaded and unzipped the AWS CLI zip file onto my Jenkins server. Then, I installed and configured AWS on the command line with my access key. 
I created a password for the jenkins user and signed into that profile. Then, I installed AWS EB CLI and added the programs in the jenkins user to the shell. The IAM roles were created for EB to configure AWS infrastructure and for the EC2 to host the application and use other AWS resources 
Signing into the Jenkins user gave me access to the files and pipelines created by the build. In the pipeline with the application and test files, I initialized that directory to be the input files for Elastic Beanstalk to create the application. Instead of using the GUI, I used the terminal to create an enivronment for the application. The terminal, then, printed out the event logs, which showed the creation of the infrastructure. It also produced the URL to the deployed application.
<img width="1211" alt="Screen Shot 2023-09-15 at 1 33 38 PM" src="https://github.com/nalDaniels/Deployment3/assets/135375665/de44f0a6-4d8d-4b8e-9b7d-4d293c519d88">
#### c. Issue
When running `pip install awsebcli --upgrade --user`, I ran into an issue using the pip command. The CLI suggested I run the sudo apt install python3-pip. I assume this means that in order to install awsebcli required the pip package manager specifically for python3. However, I was unable to do this within the jenkins user and had to execute it using the ubuntu user profile.
#### d. Optimization
Instead of running each command separately, I could have created a script to install AWS CLI, AWS EB CLI, and create the application. 

### 3. Adding a Deploy Stage to the Jenkinsfile
#### a. Goal
To be able to automatically deploy new changes/commits to the application files to the environment I created in the previous step.
#### b. Process
I added the deploy stage to the jenkinsfile, reran the build with the new jenkinsfile commit, and now Jenkins displayed a 'Deploy' stage.

<img width="833" alt="Screen Shot 2023-09-15 at 4 08 59 PM" src="https://github.com/nalDaniels/Deployment3/assets/135375665/ae48a598-2cd9-48e7-9cc2-06a829d2b267">
#### c. Optimization
The webhook could have been created before the change made to the jenkinsfile. 

### 3. Creating a Webhook
#### a. Goal
To trigger Jenkins to rebuild, test, and deploy the application code and files each time there is a new commit.

### 3. Modifying the Application Code
#### a. Goal
To create a new commit in the repository to trigger the webhook and rerun the build.
#### b. Process
I changed the names of the fields in the home.html file, and this prompted a new build in Jenkins. The new deployment had the updated changes. 


<img width="331" alt="Screen Shot 2023-09-15 at 4 11 39 PM" src="https://github.com/nalDaniels/Deployment3/assets/135375665/40b01614-4b89-4d0b-9849-8829faabf333">
<img width="853" alt="Screen Shot 2023-09-15 at 4 12 25 PM" src="https://github.com/nalDaniels/Deployment3/assets/135375665/fed5b2e9-5fb1-4d1b-b615-a33bd6671a32">
<img width="1211" alt="Screen Shot 2023-09-15 at 4 12 37 PM" src="https://github.com/nalDaniels/Deployment3/assets/135375665/091f9746-d4d2-4cec-b854-3f91b6920f67">

## Resources
Please find my system design for this deployment here:
https://github.com/nalDaniels/Deployment3/blob/main/SystemDesign.md


