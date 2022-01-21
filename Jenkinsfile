pipeline {
	agent any
    stages {
    	stage('minikube-Deployment') {
    		steps {
    			bat 'kubectl apply -f react-deployment.yaml'
    		}
    	}
    }
}
