pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Check out source code from Git repository
                git 'https://github.com/Ramprasadvaral13/ci-cd'
            }
        }

        stage('Build Docker Image') {
            steps {
                // Build Docker image
                script {
                    docker.build('ramprasadv7/flaskcalculator:latest')
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                // Push Docker image to Docker registry
                script {
                    docker.withRegistry('https://hub.docker.com', 'docker-cred') {
                        dockerImage.push('ramprasadv7/flaskcalculator:latest')
                    }
                }
            }
        }

        stage('Static Code Analysis') {
            steps {
                // Install necessary dependencies
                sh 'pip install pylint'

                // Run static code analysis
                sh 'pylint ci-cd/app.py'
            }
        }
    }
