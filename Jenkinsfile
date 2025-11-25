pipeline {
    agent any

    environment {
        DOCKERHUB_USER = credentials('bbsharma102001')     // Jenkins Credentials
        DOCKERHUB_PASS = credentials('Dockerhub@123')
        IMAGE = "bulbulsharma102001/static-website:latest"
        YAML_PATH = "C:\\ProgramData\\Jenkins\\.jenkins\\workspace\\Static-Dockerhub-Img\\k8s.yaml"
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
                echo "Checking Kubernetes Access..."
                kubectl get nodes

                echo "Applying Kubernetes YAML..."
                kubectl apply --validate=false -f "${env.YAML_PATH}"

                echo "Deployment Status..."
                kubectl get pods
                kubectl get svc
                """
            }
        }
    }
}
