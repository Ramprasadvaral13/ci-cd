pipeline {
    agent any 
    
    environment {
        IMAGE_TAG = "${BUILD_NUMBER}"
        SONAR_URL = "http://54.85.200.200:9000"
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

        stage('Static Code Analysis') {
            environment {
                SONAR_AUTH_TOKEN = credentials('sonarqube')
            }
            steps {
                script {
                    withSonarQubeEnv('SonarQube') {
                        sh '''
                        sonar-scanner \
                        -Dsonar.host.url=${SONAR_URL} \
                        -Dsonar.login=${SONAR_AUTH_TOKEN}
                        -Dsonar.sources=src/main/app.py
                        '''
                    }
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
