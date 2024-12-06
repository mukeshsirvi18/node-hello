pipeline {
    agent any
    stages {
        stage('Test Docker Login') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker_creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh """
                    echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                    docker logout
                    """
                }
            }
        }
    }
}
