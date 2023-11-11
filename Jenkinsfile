pipeline {
	agent {
		docker {
			image 'composer:latest'
		}
	}
	stages {
		stage('Build') {
			steps {
				sh 'composer install'
			}
		}
		stage('Test') {
			steps {
                sh './vendor/bin/phpunit tests'
            }
		}
		stage('OWASP-DC') {
		    steps {
			dependencyCheck additionalArguments: ''' 
				    --disableYarnAudit
			            -o './'
			            -s './'
			            -f 'ALL' 
			            --prettyPrint''', odcInstallation: 'OWASP-DC'
			
			dependencyCheckPublisher pattern: 'dependency-check-report.xml'
		    }
		}
	}
}
