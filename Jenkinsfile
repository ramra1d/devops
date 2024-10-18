pipeline {
    agent any

    environment {
        KUBECONFIG_CREDENTIALS = 'kubeconfig'  // The ID of your kubeconfig credentials in Jenkins
        DEPLOYMENT_YAML = 'nginx-deployment.yaml'  // Path to the YAML file in the repo
    }

    stages {
        stage('Checkout Code') {
            steps {
                // Clone the repository containing the Kubernetes YAML file
                git url: 'https://github.com/ramra1d/devops.git'
                // List the files in the workspace to confirm presence
                sh 'ls -al'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                withCredentials([file(credentialsId: KUBECONFIG_CREDENTIALS, variable: 'KUBECONFIG')]) {
                    // Set the KUBECONFIG file for the session
                    sh 'export KUBECONFIG=$KUBECONFIG'

                    // Apply the Kubernetes deployment and service YAML
                    sh "kubectl apply -f ${DEPLOYMENT_YAML}"

                    // Verify the deployment
                    sh "kubectl get pods"
                }
            }
        }
    }

    post {
        success {
            echo 'NGINX has been deployed to Kubernetes successfully!'
        }
        failure {
            echo 'Deployment failed.'
        }
    }
}
