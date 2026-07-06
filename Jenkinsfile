pipeline {

    agent any

    environment {
        IMAGE_NAME = "shaheeradmin/gitops-demo"
    }

    stages {

        stage('Verify Workspace') {
            steps {
                sh '''
                    pwd
                    ls -la
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                    docker build -t $IMAGE_NAME:${BUILD_NUMBER} .
                '''
            }
        }

        stage('Docker Login') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub',
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_PASS'
                    )
                ]) {
                    sh '''
                        echo "$DOCKER_PASS" | docker login \
                        -u "$DOCKER_USER" \
                        --password-stdin
                    '''
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                sh '''
                    docker push $IMAGE_NAME:${BUILD_NUMBER}
                '''
            }
        }
    }

    post {
        success {
            echo "Docker image pushed successfully"
        }

        failure {
            echo "Pipeline failed"
        }
    }
}
