
1. Jenkins CI CD pipeline for flask application
Objective:
Set up a Jenkins pipeline that automates the testing and deployment of a simple Python web application.
Requirements:
1. Setup:
   - Install Jenkins on a virtual machine or use a cloud-based Jenkins service.
   - Configure Jenkins with Python and any necessary libraries.
2. Source Code:
  - Fork the provided Python web application repository on GitHub (provide a link to a sample Python web application repository).
  - Clone the forked repository into your Jenkins server.
3. Jenkins Pipeline:
   - Create a Jenkinsfile in the root of your Python application repository.
   - Define a pipeline with the following stages:
    - Build: Install dependencies using pip.
    - Test: Run unit tests using a testing framework like pytest.
    - Deploy: If tests pass, deploy the application to a staging environment.
4. Triggers:
   - Configure the pipeline to trigger a new build whenever changes are pushed to the main branch of the repository.
5. Notifications:
   - Set up a notification system to alert via email when the build process fails or succeeds.
6. Documentation:
   - Document the pipeline process and any prerequisites needed for the setup in a README.md file in the repository.
7. Submission:
   - Provide the URL to the GitHub repository with the Jenkinsfile and updated README.md.
   - Include screenshots of the Jenkins pipeline showing the build, test, and deployment stages.
Deliverables:
- Forked GitHub repository with Jenkinsfile.
- Documentation in README.md.
- Screenshots of the Jenkins pipeline execution.


Project solution:
Setting up local vm through vagrant
Downloaded vagrant file from vagrant cloud and than setting up locally ubuntu linux

<img width="979" height="552" alt="image" src="https://github.com/user-attachments/assets/d37dc764-fa5c-43b7-9436-f424ff103a77" />


Starting ubuntu vm:
Vagrant up
<img width="979" height="552" alt="image" src="https://github.com/user-attachments/assets/f6e61cd1-58ac-4072-b2c6-1eee0b27746a" />

Checking status of ubuntu vm:
Vagrant status
<img width="979" height="552" alt="image" src="https://github.com/user-attachments/assets/14645832-af26-4763-9f74-5e72337ec40b" />

Ssh into the vm:
Vagrant ssh
<img width="979" height="552" alt="image" src="https://github.com/user-attachments/assets/08c38bb9-beb3-4c0d-95e7-9989e4ffe6b7" />

Installing Jenkins into vm:
sudo wget -O /etc/yum.repos.d/jenkins.repo \
    https://pkg.jenkins.io/redhat-stable/jenkins.repo
sudo dnf upgrade
# Add required dependencies for the Jenkins package
sudo dnf install fontconfig java-21-openjdk
sudo dnf install jenkins
sudo systemctl daemon-reload

sudo apt update
sudo apt install -y jenkins
sudo systemctl start jenkins
sudo systemctl enable Jenkins

accessing Jenkins server in browser
<img width="979" height="552" alt="image" src="https://github.com/user-attachments/assets/d603e64c-a376-4fb9-b033-6971a54abd9e" />

Setting password:
<img width="979" height="552" alt="image" src="https://github.com/user-attachments/assets/64ec9c35-c152-4517-80ae-ee325548d05a" />
<img width="979" height="552" alt="image" src="https://github.com/user-attachments/assets/d579df0d-fd06-4f59-a107-08ba5e2af51e" />
<img width="979" height="552" alt="image" src="https://github.com/user-attachments/assets/500edb15-02e2-4765-87b2-c8cc40fb0d34" />
<img width="979" height="552" alt="image" src="https://github.com/user-attachments/assets/75b0acbc-f10a-45f7-9239-e36c61116e74" />
<img width="979" height="552" alt="image" src="https://github.com/user-attachments/assets/594577f6-ac40-4f7b-a11c-0c4b9602000c" />
<img width="979" height="552" alt="image" src="https://github.com/user-attachments/assets/c06d9b28-002d-4fbf-9fe1-36b4f0b599eb" />
<img width="979" height="552" alt="image" src="https://github.com/user-attachments/assets/5486a5e7-495d-4fea-bf41-edfda3b3b750" />
<img width="979" height="552" alt="image" src="https://github.com/user-attachments/assets/e6482bcd-bde1-4a7c-8388-18095d424ce3" />
Clone Repository on Jenkins Server:
cd /home/vagrant
<img width="979" height="552" alt="image" src="https://github.com/user-attachments/assets/936ac9e8-fe06-433b-a4f2-528a3788ba02" />

git clone https://github.com/mohanDevOps-arch/flask_practice
<img width="979" height="552" alt="image" src="https://github.com/user-attachments/assets/7ebb65aa-19a4-4e93-8a01-92945c6ae635" />


Creating Jenkinsfile pipeline {
pipeline {
    agent any

    environment {
        VENV = "venv"
         MONGO_URI = "mongodb+srv://admin:admin@cluster0.stznivp.mongodb.net/sms"
    }

    stages {
        stage('Build') {
            steps {
                sh '''
                python3 -m venv $VENV
                . $VENV/bin/activate
                pip install --upgrade pip
                pip install -r requirements.txt
                '''
            }
        }

        stage('Test') {
            steps {
                sh '''
                . $VENV/bin/activate
                pytest
                '''
            }
        }

        stage('Deploy to Staging') {
            steps {
                sh '''
                echo "Deploying Flask app to staging..."
                pkill -f app.py || true
                nohup venv/bin/python app.py > app.log 2>&1 &
                '''
            }
        }
    }

    post {
        success {
            mail to: 'saiyedin786@gmail.com',
                 subject: 'Jenkins Build SUCCESS',
                 body: 'The Flask pipeline completed successfully.'
        }
        failure {
            mail to: 'saiyedin786@example.com',
                 subject: 'Jenkins Build FAILED',
                 body: 'The Flask pipeline failed. Please check Jenkins.'
        }
    }
}

Creting an github repo:
<img width="979" height="552" alt="image" src="https://github.com/user-attachments/assets/7cc259c3-fae3-429e-8d1a-132029876721" />
<img width="979" height="552" alt="image" src="https://github.com/user-attachments/assets/b39573f8-4cdb-4329-b474-4536ee802c9b" />
<img width="979" height="552" alt="image" src="https://github.com/user-attachments/assets/151493ff-1447-40cd-b456-3835e85227a9" />
Creating staging repo:
<img width="979" height="552" alt="image" src="https://github.com/user-attachments/assets/3e6ed623-af55-45ec-bdcf-06c5bf0ec71c" />
<img width="979" height="552" alt="image" src="https://github.com/user-attachments/assets/d82e18fb-5318-4df1-9ef7-0d45693e34f8" />

Configure Jenkins Pipeline Job
1.	Jenkins Dashboard → New Item
2.	Name: Jenkins-ci-cd
3.	Type: Pipeline
4.	Under Pipeline:
o	Definition: Pipeline script from SCM
o	SCM: Git
o	Repository URL:
o	https://github.com/mohanDevOps-arch/flask_practice
o	Branch: */main
o	Script Path: Jenkinsfile
5.	Save
<img width="979" height="552" alt="image" src="https://github.com/user-attachments/assets/55fd81e7-0bbc-4176-aff3-38602567f014" />
<img width="979" height="552" alt="image" src="https://github.com/user-attachments/assets/2599c168-4773-4c3a-80ad-d0398280e884" />
<img width="979" height="552" alt="image" src="https://github.com/user-attachments/assets/e625f9c3-fe5e-4a3a-af0b-ae1a7eb401dc" />

Configuring github webhook:
<img width="979" height="552" alt="image" src="https://github.com/user-attachments/assets/5f49c865-7908-45b5-9af8-58538cce440b" />
<img width="979" height="552" alt="image" src="https://github.com/user-attachments/assets/de5625b9-f7d8-43c0-84ee-db59458dbdf0" />
Pushing data into github repo from local pc:
<img width="979" height="552" alt="image" src="https://github.com/user-attachments/assets/f977e7c2-4e17-46b5-b71e-cf1f3aa447f2" />
<img width="979" height="552" alt="image" src="https://github.com/user-attachments/assets/6bc327a3-8bb2-4b39-9e89-b8686dfc9a8f" />
Jenkinfile runs code is deployed on ubuntu vm
<img width="979" height="552" alt="image" src="https://github.com/user-attachments/assets/5ed133f4-e279-4f4a-8d5f-31fcf957e3bb" />

<img width="979" height="552" alt="image" src="https://github.com/user-attachments/assets/ce1ece49-2670-426a-b8a1-177a8cd6825d" />
<img width="979" height="552" alt="image" src="https://github.com/user-attachments/assets/0c8be5a5-d8ff-46a7-b1d0-5f7411c7ca01" />
<img width="979" height="552" alt="image" src="https://github.com/user-attachments/assets/480a1d8f-c23c-49b7-b2d2-b44275a75591" />
<img width="979" height="552" alt="image" src="https://github.com/user-attachments/assets/793a6786-8fa3-4f91-899c-d00f9eda18b0" />
<img width="979" height="552" alt="image" src="https://github.com/user-attachments/assets/c4d8fd8d-4e73-4370-b802-4e3b2606ba8f" />
<img width="979" height="552" alt="image" src="https://github.com/user-attachments/assets/ce33949a-bc6e-43cd-91ec-34b57373ffac" />
<img width="979" height="552" alt="image" src="https://github.com/user-attachments/assets/2769225d-ef22-4c39-bbd4-126f7dd875a4" />
<img width="979" height="552" alt="image" src="https://github.com/user-attachments/assets/aea61c95-f2e7-4b11-9df8-6d3b634b3c0a" />
<img width="979" height="552" alt="image" src="https://github.com/user-attachments/assets/f611278b-ef42-4cca-af46-c45a2f6886b9" />
Email notification received in my email id:
<img width="979" height="552" alt="image" src="https://github.com/user-attachments/assets/40c40c3e-2b65-44d6-bf4d-07c5c2d680a4" />




















