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
		stage('Deploy Backend'){
			steps {
				deploy adapters: [tomcat8(credentialsId: 'TomcatLogin', path: '', url: 'http://localhost:8001/')], contextPath: 'tasks-backend', war: 'target/tasks-backend.war'
			}
		}
		stage('API Test'){
			steps {
				dir('api-test') {
					git credentialsId: 'gitHubLogig', url: 'https://github.com/LucasBonanno/tasks-api-test'
					bat 'mvn test'
				}
			}
		}
		stage('Deploy FrontEnd') {
			steps {
				dir('frontend') {
					git credentialsId: 'gitHubLogig', url: 'https://github.com/LucasBonanno/tasks-frontend'
					bat 'mvn clean package'
					deploy adapters: [tomcat8(credentialsId: 'TomcatLogin', path: '', url: 'http://localhost:8001/')], contextPath: 'tasks', war: 'target/tasks.war'
				}
			}
		}
		stage('Funcional Test'){
			steps {
				dir('funcional-test') {
					git credentialsId: 'gitHubLogig', url: 'https://github.com/LucasBonanno/tasks-functional-tests'
					bat 'mvn test'
				}
			}
		}
	}
}


