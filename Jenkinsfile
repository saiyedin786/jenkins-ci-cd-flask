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
                nohup python app.py &
                '''
            }
        }
    }

    post {
        success {
            mail to: 'admin@example.com',
                 subject: 'Jenkins Build SUCCESS',
                 body: 'The Flask pipeline completed successfully.'
        }
        failure {
            mail to: 'admin@example.com',
                 subject: 'Jenkins Build FAILED',
                 body: 'The Flask pipeline failed. Please check Jenkins.'
        }
    }
}
