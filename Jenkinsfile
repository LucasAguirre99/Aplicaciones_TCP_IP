pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo "clonning repository"
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
                echo "applying manifiest"
                sh 'apt update'
                sh 'apt install snapd'
                sh 'snap install kubectl --classic'
                sh 'kubectl version --client'
                //sh 'kubectl apply -f docs/03-Proyecto_Final/proyecto/server/kubernetes/streamlit'
                //sh 'sleep 40'
                //sh 'kubectl get pod -l app=streamlit -n tcp-ip -o jsonpath="{.items[0].status.podIP}" '
            }
        }
    }
}
