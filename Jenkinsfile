pipeline {
	agent any
	
    stages {
      
		stage('CI') {
			agent {
				dockerfile {
					filename 'Dockerfile.build'
					label 'my-ci-image'
					args '-v ~/.npm:.npm'
				}
			}
			stages {
				stage('NPM') {
					steps {
						sh 'ls -a'
						sh 'node --version'
						sh 'npm ci --cache .npm'
					}
				}

			}
		}

	}
}
