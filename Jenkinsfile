pipeline {
    agent any 
    
    environment {
        IMAGE_TAG = "${BUILD_NUMBER}"
    }
    
    stages {
        stage('Checkout') {
            steps {
                git credentialsId: 'credentials', 
                url: 'https://github.com/Ramprasadvaral13/ci-cd',
                branch: 'main'
            }
        }

        stage('Build Docker') {
            steps {
                script {
                    sh '''
                    echo 'Build Docker Image'
                    docker build -t ramprasadv7/cal-e2e:${IMAGE_TAG} .
                    '''
                }
            }
        }

        stage('Push Docker Image') {
            environment {
                DOCKER_CREDENTIALS = credentials('docker-cred')
            }
            steps {
                script {
                    withDockerRegistry([credentialsId: 'docker-cred', url: "https://index.docker.io/v1/"]) {
                        docker.image("ramprasadv7/cal-e2e:${IMAGE_TAG}").push()
                    }
                }
            }
        }
    }
}
