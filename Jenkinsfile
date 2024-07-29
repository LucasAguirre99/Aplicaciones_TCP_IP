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
        
        stage('Diagnóstico') {
            steps {
                sh 'kubectl version --client'
                sh 'kubectl config view'
                sh 'kubectl cluster-info'
            }
        }
        
        stage('Deploy') {
            steps {
                script {
                    try {
                        sh 'kubectl apply -f docs/03-Proyecto_Final/proyecto/server/kubernetes/streamlit/streamlit-deployment.yaml'
                    } catch (Exception e) {
                        echo "Error al aplicar el manifiesto. Intentando sin validación..."
                        sh 'kubectl apply -f docs/03-Proyecto_Final/proyecto/server/kubernetes/streamlit/streamlit-deployment.yaml --validate=false'
                    }
                }
            }
        }
                
        stage('Deploy') {
            steps {
                echo "applying manifiest"
                sh 'kubectl version --client'
                sh 'kubectl apply -f docs/03-Proyecto_Final/proyecto/server/kubernetes/streamlit'
                sh 'sleep 40'
                sh 'kubectl get pod -l app=streamlit -n tcp-ip -o jsonpath="{.items[0].status.podIP}" '
            }
        }
    }
}
