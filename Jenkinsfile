

pipeline {
	agent any
	
    stages {
		stage('CI') {
			agent {
				dockerfile {
					filename 'Dockerfile.build'
					args '-v /root/.npm:/.npm'
				}
			}
			stages {
				stage('NPM') {
					steps {
						sh 'ls -a; node --version'
						sh 'npm ci --cache .npm'
					}
				}
				stage('Test') {
					steps {
						sh 'npm test'
						archiveArtifacts artifacts: 'coverage/*.*', followSymlinks: false
						cobertura autoUpdateHealth: false, autoUpdateStability: false, coberturaReportFile: 'coverage/cobertura-coverage.xml', conditionalCoverageTargets: '70, 0, 0', failUnhealthy: false, failUnstable: false, lineCoverageTargets: '80, 0, 0', maxNumberOfBuilds: 0, methodCoverageTargets: '80, 0, 0', onlyStable: false, sourceEncoding: 'ASCII', zoomCoverageChart: false
					}
				}     
			}
		}
	}
}

