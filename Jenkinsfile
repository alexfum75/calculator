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
        stage ('Docker build') {
            steps {
                sh "/tmp/docker build -t alexfum/calculator ."
            }
        }
        stage ('Docker push') {
            steps {
                sh "/tmp/docker push alexfum/calculator"
            }
        }
        stage ('Deploy to staging') {
            steps {
                sh "/tmp/docker run -d --rm -p:8765:8080 --name calculator alexfum/calculator"
            }
        }
    }
}

