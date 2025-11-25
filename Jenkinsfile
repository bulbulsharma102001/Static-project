pipeline {
    agent any

    environment {
        DOCKERHUB_USER = credentials('bbsharma102001')     // Jenkins Credential ID
        DOCKERHUB_PASS = credentials('Dockerhub@123')     // Jenkins Credential ID
        IMAGE = "bulbulsharma102001/static-website:latest"      // Your Docker Image
        YAML_FILE = "k8s.yaml"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/bulbulsharma102001/Static-project.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${IMAGE} ."
                }
            }
        }

        stage('Login to DockerHub') {
            steps {
                script {
                    sh "echo ${DOCKERHUB_PASS} | docker login -u ${DOCKERHUB_USER} --password-stdin"
                }
            }
        }

        stage('Push Image to DockerHub') {
            steps {
                script {
                    sh "docker push ${IMAGE}"
                }
            }
        }

        stage('Deploy to Kubernetes (Minikube)') {
            steps {
                powershell """
                echo "Checking Kubernetes Access..."
                kubectl get nodes

                echo "Deploying to Minikube..."
                kubectl apply --validate=false -f "${WORKSPACE}\\${YAML_FILE}"

                echo "Deployment Status:"
                kubectl get pods
                kubectl get svc
                """
            }
        }
    }
}
