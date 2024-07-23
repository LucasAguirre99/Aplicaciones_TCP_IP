pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo "Building app"
                checkout scm
            }
        }
        stage('Test') {
            steps {
                echo "Testing code"
                sh 'ls'
                sh 'pwd' 
            }
        }
        stage('Deploy') {
            steps {
                echo "Deploying code"
            }
        }
    }
}
