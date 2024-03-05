pipeline {
    agent any 
    
    environment {
        IMAGE_TAG = "${BUILD_NUMBER}"
        credentials = 'git-cred'
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
                    docker build -t ramprasadv7/cal-e2e:${BUILD_NUMBER} .
                    '''
                }
            }
        }

        stage('Push Docker Image') {
            environment {
                REGISTRY_CREDENTIALS = credentials('docker-cred')
            }
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', "docker-cred") {
                        dockerImage.push("ramprasadv7/cal-e2e:${BUILD_NUMBER}")
                    }
                }
            }
        }
    }
}
