pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo "Cloning repository"
                checkout scm
            }
        }
        stage('Test') {
            steps {
                echo "Current directory:"
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
                        echo "Applying manifests"
                        sh 'kubectl apply -f docs/03-Proyecto_Final/proyecto/server/kubernetes/streamlit'
                    } catch (Exception e) {
                        echo "Error al aplicar los manifiestos. Intentando sin validación..."
                        sh 'kubectl apply -f docs/03-Proyecto_Final/proyecto/server/kubernetes/streamlit --validate=false'
                    }
                }
                
                echo "Waiting for pods to be ready..."
                sh 'sleep 40'
                
                echo "Getting Streamlit pod IP:"
                sh 'kubectl get pod -l app=streamlit -n tcp-ip -o jsonpath="{.items[0].status.podIP}"'
            }
        }
    }
    
    post {
        always {
            echo "Pipeline execution completed"
        }
        success {
            echo "Pipeline executed successfully"
        }
        failure {
            echo "Pipeline execution failed"
        }
    }
}
