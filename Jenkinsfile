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
        
        stage('Delete existing deployment') {
            steps {
                script {
                    try {
                        echo "Checking if Streamlit deployment exists"
                        def deploymentExists = sh(script: 'kubectl get deployment streamlit -n tcp-ip', returnStatus: true) == 0
                        if (deploymentExists) {
                            echo "Deleting existing Streamlit deployment"
                            sh 'kubectl delete deployment streamlit -n tcp-ip'
                            echo "Waiting for deployment to be fully deleted..."
                            sh 'kubectl wait --for=delete deployment/streamlit -n tcp-ip --timeout=60s'
                        } else {
                            echo "Streamlit deployment does not exist, proceeding with creation"
                        }
                    } catch (Exception e) {
                        echo "Error checking or deleting deployment: ${e.message}"
                    }
                }
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
