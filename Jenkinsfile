pipeline {
    agent {
        docker { image 'node:16.13.1-alpine' }
    }
    stages {
        stage('NPM') {
            steps {
                sh 'node --version'
                sh 'npm ci'
            }
        }
        stage('Test') {
            steps {
                sh 'npm test'
            }
        }     
        stage('Build') {
            steps {
                sh 'npm build'
            }
        }    
    }
}
