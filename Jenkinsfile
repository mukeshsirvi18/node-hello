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
        withCredentials([usernamePassword(credentialsId: 'mukesh18s', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
            script {
                // Deploy to EC2 using sshpass with stored password credentials
                sh """
                sshpass -p $SSH_PASSWORD ssh -o StrictHostKeyChecking=no $REMOTE_SERVER "
                # Login to Docker on EC2 using Jenkins stored credentials
                echo \$DOCKER_PASS | docker login -u \$DOCKER_USER --password-stdin;
                # Pull Docker image from Docker Hub
                docker pull $DOCKER_IMAGE;
                # Stop and remove the container if it exists
                docker stop node-hello || true;
                docker rm node-hello || true;
                # Run the container
                docker run -d --name node-hello -p 80:3000 $DOCKER_IMAGE
                "
                """
            }
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
