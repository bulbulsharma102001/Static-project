pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo 'Pulling website code...'
                git branch: 'main', url: 'https://github.com/bulbulsharma102001/Static-project.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                powershell """
                    docker build -t static-website:latest .
                """
            }
        }

        stage('Run Container') {
            steps {
                powershell """
                    docker stop static-web 2>\$null
                    docker rm static-web 2>\$null
                    docker run -d --name static-web -p 9090:80 static-website:latest
                """
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                echo "Applying Kubernetes manifests..."
                powershell """
                    kubectl apply -f k8s.yaml
                """
            }
        }
    }

    post {
        success {
            echo "Website running at: http://localhost:9090"
            echo "Kubernetes deployment applied successfully."
        }
    }
}
