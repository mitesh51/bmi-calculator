pipeline {
	agent any
    stages {
		stage('CI') {
			agent {
				docker { image 'node:16.13.1-alpine' }
			}
			stages {
				stage('NPM') {
					steps {
						sh 'ls -a'
						sh 'rm build.zip'
						sh 'rm -r build'
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
						zip archive: true, dir: 'build', glob: '', zipFile: 'build.zip'
						archiveArtifacts artifacts: 'build.zip', followSymlinks: false
						stash(includes: 'build.zip', name: 'build')
					}
				}   
			}
		}
		stage('Docker Image') {
			agent {
				docker { image 'docker' }
			}  
			steps {
				unstash 'build'
				unzip dir: 'build', glob: '', zipFile: 'build.zip'
				sh 'docker info'
				sh 'ls -l'
				sh 'docker build -f Dockerfile -t mitesh51/bmi-calc:1.0 .'
				sh 'docker images'
				
				withDockerRegistry([ credentialsId: "dockerhub-cred", url: "" ]) {
					sh 'docker push mitesh51/bmi-calc:1.0'
				}
			}
		}
	    	stage('Trivy Image Scanner') {
			agent {
				docker { 
				    image 'aquasec/trivy:latest'
				    args '--entrypoint='
				    
				}
			}  
			steps {
				sh 'trivy help'
				sh "trivy --cache-dir /tmp i 'mitesh51/bmi-calc:1.0'"
			}
		}

	   	stage('EKS-Deployment') {
			steps {
				kubernetesDeploy configs: 'react-deployment.yaml', enableConfigSubstitution: false, kubeConfig: [path: ''], kubeconfigId: 'eks-kubeconfig', secretName: '', ssh: [sshCredentialsId: '*', sshServer: ''], textCredentials: [certificateAuthorityData: '', clientCertificateData: '', clientKeyData: '', serverUrl: 'https://']
			}
		}
    }
}
