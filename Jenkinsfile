pipeline {
    agent { label 'agent-1' }
    environment {
        AZURE_VM = 'rkydvnci@20.215.208.75'
        IMAGE = 'rickydavinci/simple-api'
    }
    stages {
        stage('Checkout') {
            steps {
                echo 'Récupération du code source...'
                checkout scm
            }
        }
        stage('Build Image') {
            steps {
                echo 'Construction de l\'image Docker...'
                sh 'docker build -t ${IMAGE}:latest .'
            }
        }
        stage('Push Image') {
            steps {
                echo 'Publication sur DockerHub...'
                sh 'docker push ${IMAGE}:latest'
            }
        }
        stage('Test') {
            steps {
                echo 'Exécution des tests...'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Déploiement sur Azure...'
                sh """
                    ssh -o StrictHostKeyChecking=no ${AZURE_VM} '
                        docker pull ${IMAGE}:latest &&
                        docker stop simple-api || true &&
                        docker rm simple-api || true &&
                        docker run -d --name simple-api -p 3000:3000 ${IMAGE}:latest
                    '
                """
            }
        }
    }
}
