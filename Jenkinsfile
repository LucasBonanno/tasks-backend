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
				deploy adapters: [tomcat8(credentialsId: 'TomcatLogin', path: '', url: 'http://192.168.0.102:8001/')], contextPath: 'tasks-backend', war: 'target/tasks-backend.war'
			}
		}
	}
}


