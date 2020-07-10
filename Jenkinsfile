pipeline {
    agent any

     stages {
        stage('Build') {
            steps {
                sh "/tmp/apache-maven-3.6.3/bin/mvn package"
            }
        }
        stage ('Unit Test') {
            steps {
                sh "/tmp/apache-maven-3.6.3/bin/mvn test"
            }
        }
        stage ('Code coverage') {
            steps {
                sh "/tmp/apache-maven-3.6.3/bin/mvn test"
		publishHTML (target: [
			reportDir: 'target/site/jacoco',
			reportFiles: 'index.html',
			reportName: 'Jacoco Report'
		])
            }
        }
    }
    }
}

