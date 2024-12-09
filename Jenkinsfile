pipeline {
    agent any

    stages {
        stage('Verify Docker Hub Login') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'mukesh18s', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh """
                    echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                    echo "Docker login successful!"
                    docker logout
                    """
                }
            }
        }
    }

    post {
        success {
            echo 'Docker Hub credentials are correct!'
        }
        failure {
            echo 'Failed to log in to Docker Hub. Check credentials!'
        }
    }
}
