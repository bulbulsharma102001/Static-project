pipeline {
    agent any

    environment {
        DOCKERHUB_USER = credentials('dockerhub-username')   // Jenkins Credential ID
        DOCKERHUB_PASS = credentials('dockerhub-password')   // Jenkins Credential ID
        IMAGE = "bulbulsharma102001/static-website:latest"
        YAML_PATH = "${WORKSPACE}\\k8s.yaml"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/bulbulsharma102001/Static-project.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                powershell """
                docker build -t ${env.IMAGE} .
                """
            }
        }

        stage('Login to DockerHub') {
            steps {
                powershell """
                echo ${env.DOCKERHUB_PASS} | docker login -u ${env.DOCKERHUB_USER} --password-stdin
                """
            }
        }

        stage('Push Docker Image') {
            steps {
                powershell """
                docker push ${env.IMAGE}
                """
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                powershell """
                echo "Checking Kubernetes access..."
                kubectl get nodes

                echo "Deploying application..."
                kubectl apply --validate=false -f "${env.YAML_PATH}"

                echo "Deployment status:"
                kubectl get pods
                kubectl get svc
                """
            }
        }
    }
}
