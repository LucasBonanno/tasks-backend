pipeline {
	agent any
	stages {
		stage('Build Backend') {
			steps {
			# comando mvn para limpar e empacotar porém não executar os testes
				bat 'mvn clean package -DskipTests=true'
			}
		}
		stage('Unit Tests') {
			steps {
			# aqui sim executa os testes
				bat 'mvn test'
			}
		}
		stage('Deploy Backend'){
			steps {
				deploy adapters: [tomcat8(credentialsId: 'TomcatLogin', path: '', url: 'http://localhost:8001/')], contextPath: 'tasks-backend', war: 'target/tasks-backend.war'
			}
		}
	}
}


