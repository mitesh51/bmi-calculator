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
                sh 'ls -l'
                archiveArtifacts artifacts: 'coverage/*.*', followSymlinks: false
                cobertura autoUpdateHealth: false, autoUpdateStability: false, coberturaReportFile: 'coverage/cobertura-coverage.xml', conditionalCoverageTargets: '70, 0, 0', failUnhealthy: false, failUnstable: false, lineCoverageTargets: '80, 0, 0', maxNumberOfBuilds: 0, methodCoverageTargets: '80, 0, 0', onlyStable: false, sourceEncoding: 'ASCII', zoomCoverageChart: false
            }
        }     
        stage('Build') {
            steps {
                sh 'ls -l'
                sh 'npm run build'
                sh 'ls -l'
                //archiveArtifacts artifacts: 'build/*.*', followSymlinks: false
            }
        }    
        stage('Docker Image') {
            steps {
                sh 'docker login -u mitesh51 -p Bestcredit@121'
                sh 'docker build -t bmi-calc'
            }
        }
    }
}
