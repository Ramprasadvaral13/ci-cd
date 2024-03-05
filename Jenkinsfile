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

        stage('Push the artifacts') {
           steps {
                script {
                    sh '''
                    echo 'Push to Repo'
                    docker push ramprasadv7/cal-e2e:${BUILD_NUMBER}
                    '''
                }
            }
        }
    }
}

