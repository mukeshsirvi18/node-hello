pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "mukesh18s/node-hello-world" // Replace with your Docker Hub repo
        SSH_PASSWORD = "ubuntu"
        REMOTE_SERVER = "root@16.170.226.199"
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'master', url: 'https://github.com/mukeshsirvi18/node-hello.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh """
                    docker build -t $DOCKER_IMAGE .
                    """
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'mukesh18s', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh """
                    echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                    docker push $DOCKER_IMAGE
                    docker logout
                    """
                }
            }
        }

       stage('Deploy to EC2') {
            steps {
                script {
                    // Using sshpass for password-based SSH login
                    sh """
                        sshpass -p '${SSH_PASSWORD}' ssh -o StrictHostKeyChecking=no ${REMOTE_SERVER} << EOF
                        docker pull $DOCKER_IMAGE
                        docker stop node-hello || true
                        docker rm node-hello || true
                        docker run -d --name node-hello -p 80:3000 $DOCKER_IMAGE
                        EOF
                    """
                }
            }
        }

    }

    post {
        success {
            echo "Application deployed successfully!"
        }
        failure {
            echo "Deployment failed. Check the logs."
        }
    }
}
