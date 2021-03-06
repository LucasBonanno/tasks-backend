pipeline {
	agent any
	stages {
		stage('Build Backend') {
			steps {
				bat 'mvn clean package -DskipTests=true'
			}
		}
		stage('Unit Tests') {
			steps {
				bat 'mvn test'
			}
		}
		stage('Deploy Backend') {
			steps {
				deploy adapters: [tomcat8(credentialsId: 'TomcatLogin', path: '', url: 'http://localhost:8001/')], contextPath: 'tasks-backend', war: 'target/tasks-backend.war'
			}
		}
		stage('API Test') {
			steps {
				dir('api-test') {
					git credentialsId: 'gitHubLogig', url: 'https://github.com/LucasBonanno/tasks-api-test'
					bat 'mvn test'
				}
			}
		}
		stage('Deploy Frontend') {
			steps {
				dir('frontend') {
					git credentialsId: 'gitHubLogig', url: 'https://github.com/LucasBonanno/tasks-frontend'
					bat 'mvn clean package'
					deploy adapters: [tomcat8(credentialsId: 'TomcatLogin', path: '', url: 'http://localhost:8001/')], contextPath: 'tasks', war: 'target/tasks.war'
				}
			}
		}
		stage('Functional Test') {
			steps {
				dir('functional-test') {
					git credentialsId: 'gitHubLogig', url: 'https://github.com/LucasBonanno/tasks-functional-tests'
					bat 'mvn test'
				}
			}
		}
		stage('Deploy Prod') {
			steps {
				dir('functional-test') {
					bat 'docker-compose build'
					bat 'docker-compose up -d'
				}
			}
		}
		stage('Health Check') {
			steps {
				sleep(5) 
				dir('functional-test') {
					bat 'mvn verify -Dskip.surefire.tests'
				}
			}
		}
	}
	post {
		always {
			junit allowEmptyResults: true, testResults: 'target/surefire-reports/*.xml, APITest/target/surefire-reports/*.xml, FuncionalTest/target/surefire-reports/*.xml, FuncionalTest/target/failsafe-reports/*.xml'
			archiveArtifacts artifacts: 'target/tasks-backend.war, frontend/target/tasks.war', onlyIfSuccessful: true
		}
	}
}


