pipeline {
	agent any
    stages {
    	stage('minikube-Deployment') {
    		steps {
    			bat 'kubectl apply -f react-deployment.yaml'
    		}
    	}
stage('Approval') {
input 'Ready for Load Testing?'
}

stage('load testing') {
    		steps {
    			bat 'cd F:\\1.DevOps\\2022\\apache-jmeter-5.4.3\\bin'
    			bat 'dir'
    			bat "F:/1.DevOps/2022/apache-jmeter-5.4.3/bin/jmeter.bat -Jjmeter.save.saveservice.output_format=xml -n -t C:/Users/Mitesh/Desktop/ReactJSAll.jmx -l JMeter.jtl"
    		   //perfReport filterRegex: '', showTrendGraphs: true, sourceDataFiles: 'JMeter.jtl'
    		    perfReport filterRegex: '', modeEvaluation: true, showTrendGraphs: true, sourceDataFiles: 'JMeter.jtl'
    		}
    	}
}

    }
