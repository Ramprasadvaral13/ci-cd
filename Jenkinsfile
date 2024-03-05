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

        stage('Setup') {
            steps {
                script {
                    sh 'pip install flake8 pylint mypy'
                }
            }
        }

        stage('Code Analysis') {
            steps {
                script {
                    sh 'flake8 app.py'
                    sh 'pylint app.py'
                    sh 'mypy app.py'
                }
            }
        }

        stage('Build Docker') {
            steps {
                script {
                    dockerImage = docker.build("ramprasadv7/cal-e2e:${BUILD_NUMBER}", ".")
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

