

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
						sh 'rm -f build.zip; rm -rf build'
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
				stage('Build') {
					steps {
						sh 'npm run build'
						zip archive: true, dir: 'build', glob: '', zipFile: 'build.zip'
						archiveArtifacts artifacts: 'build.zip', followSymlinks: false
						stash(includes: 'build.zip', name: 'build')
					}
				}   
			}
		}
		stage('Docker Image') {
			steps {
				unstash 'build'
				unzip dir: 'build', glob: '', zipFile: 'build.zip'
				sh 'docker build -f Dockerfile -t mitesh51/bmi-calc:1.0 . && docker images'

				withDockerRegistry([ credentialsId: "dockerhub-cred", url: "" ]) {
					sh 'docker push mitesh51/bmi-calc:1.0'
				}
			}
		}
		stage('Trivy Image Scanner') {
			agent {
				dockerfile {
					filename 'Dockerfile.docker'
					args '--entrypoint='
				}
			}
			steps {
				sh "trivy help && trivy --cache-dir /tmp i 'mitesh51/bmi-calc:1.0'"
			}
		}
	}
}

