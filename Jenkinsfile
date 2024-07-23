pipeline {
    agent any

    stages {
        stage('Setup') {
            steps {
                sh 'curl -LO "https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl"'
                sh 'chmod +x ./kubectl'
                sh 'mv ./kubectl /usr/local/bin/kubectl'
            }
        }
        stage('Build') {
            steps {
                echo "Clonning repository"
                checkout scm
            }
        }
        stage('Test') {
            steps {
                echo "Folder"
                sh 'pwd'
            }
        }
        stage('Deploy') {
            steps {
                echo "Applying manifest"
                sh 'kubectl apply -f docs/03-Proyecto_Final/proyecto/server/kubernetes/streamlit'
                sh 'sleep 40'
                sh 'kubectl get pod -l io.kompose.service=streamlit -n tcp-ip -o jsonpath="{.items[0].status.podIP}"'
            }
        }
    }
}

