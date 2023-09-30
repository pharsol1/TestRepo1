pipeline {
    agent any
    environment {
        AWS_ACCESS_KEY_ID = credentials('MY_AWS_ACCESS_KEY_ID_1')
        AWS_SECRET_ACCESS_KEY = credentials('MY_AWS_SECRET_ACCESS_KEY_1')
    }
    stages {
        stage('Build') {
            steps {
                sh 'echo "Building"'
                sh 'echo $AWS_ACCESS_KEY_ID'
                sh 'echo $AWS_SECRET_ACCESS_KEY'
            }
        }
        stage('Test') {
            steps {
                sh 'echo "Testing"'
            }
        }
        stage('Publish') {
            steps {
                sh 'echo "Publishing"'
            }
            post {
                success {
                    sh 'echo "Deploying to EB"'
                    sh 'sudo chmod 775 ./deploy_app.sh'
                    sh './deploy_app.sh'
                }
            }
        }
    }
}
