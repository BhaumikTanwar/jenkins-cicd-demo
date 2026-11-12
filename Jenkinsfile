
pipeline {
    agent any

    environment {
        KUBECTL = 'C:\\Users\\bhaum\\AppData\\Local\\Programs\\DockerDesktop\\resources\\bin\\kubectl.exe'
        KUBECONFIG = 'C:\\Users\\bhaum\\.kube\\config'
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Jenkins checks out the Jenkinsfile repository automatically.'
                bat 'dir'
                bat 'dir kubernetes'
            }
        }

        stage('Validate Manifests') {
            steps {
                bat '"%KUBECTL%" apply --dry-run=client --validate=false -f kubernetes\\deployment.yaml'
                bat '"%KUBECTL%" apply --dry-run=client --validate=false -f kubernetes\\service.yaml'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                bat '"%KUBECTL%" apply --validate=false -f kubernetes\\deployment.yaml'
                bat '"%KUBECTL%" apply --validate=false -f kubernetes\\service.yaml'
            }
        }

        stage('Verify Deployment') {
            steps {
                bat '"%KUBECTL%" rollout status deployment/flask-demo --timeout=120s'
                bat '"%KUBECTL%" get deployments'
                bat '"%KUBECTL%" get services'
                bat '"%KUBECTL%" get pods'
            }
        }
    }

    post {
        success {
            echo 'Kubernetes deployment completed successfully.'
        }
        failure {
            echo 'Deployment failed. Check Console Output.'
        }
        always {
            echo 'Pipeline execution finished.'
        }
    }
}
