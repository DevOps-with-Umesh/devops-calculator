pipeline {

    agent any

    environment {
        DOCKER_IMAGE = 'afeece/devops-calculator'
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        // stage('SonarQube Scan') {
        //     steps {
        //         withSonarQubeEnv('SonarQube') {
        //             sh 'sonar-scanner'
        //         }
        //     }
        // }

        stage('Build Docker Image') {
            steps {
                sh '''
                    docker build -t ${DOCKER_IMAGE}:${BUILD_NUMBER} .
                    docker tag ${DOCKER_IMAGE}:${BUILD_NUMBER} ${DOCKER_IMAGE}:latest
                '''
            }
        }

        stage('Trivy Scan') {
            steps {
                sh '''
                    trivy image \
                    --severity HIGH,CRITICAL \
                    --exit-code 1 \
                    ${DOCKER_IMAGE}:${BUILD_NUMBER}
                '''
            }
        }

        // stage('Push to Docker Hub') {
        //     steps {
        //         withCredentials([
        //             usernamePassword(
        //                 credentialsId: 'dockerhub-credentials',
        //                 usernameVariable: 'DOCKER_USER',
        //                 passwordVariable: 'DOCKER_PASS'
        //             )
        //         ]) {
        //             sh '''
        //                 echo "$DOCKER_PASS" | docker login \
        //                 -u "$DOCKER_USER" \
        //                 --password-stdin

        //                 docker push ${DOCKER_IMAGE}:${BUILD_NUMBER}
        //                 docker push ${DOCKER_IMAGE}:latest

        //                 docker logout
        //             '''
        //         }
            // }
        // }
    }

    post {
        success {
            echo '================================='
            echo ' CI/CD PIPELINE SUCCESSFUL'
            echo ' Docker image pushed successfully'
            echo '================================='
        }

        failure {
            echo 'Pipeline failed. Check the logs.'
        }
    }
}
