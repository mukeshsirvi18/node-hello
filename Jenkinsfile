pipeline {
    agent any
    stages {
        stage('Test Docker Login') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'mukesh18s', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh """
                    echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                    docker logout
                    """
                }
            }
        }
    }
}
