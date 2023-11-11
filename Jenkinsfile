pipeline {
	agent any
	
	stages {
		stage('Build') {
			agent {
				docker {
					image 'composer:latest'
				}
			}
			steps {
				sh 'composer install'
			}
		}
		stage('Test') {
			agent {
				docker {
					image 'composer:latest'
				}
			}
			steps {
				sh './vendor/bin/phpunit tests --log-junit logs/unitreport.xml -c tests/phpunit.xml tests'
            }
		}
		stage('OWASP-DC') {
			//agent { docker 'openjdk:17-jre' }
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
	post {
		always {
			junit testResults: 'logs/unitreport.xml'
		}
	}
}
