pipeline {
    agent any
     stages {
        stage('Build') {
            steps {
                sh "mvn package"
            }
        }
        stage ('Unit Test') {
            steps {
                sh "mvn test"
            }
        }
        stage ('Code coverage') {
            steps {
                sh "mvn test"
		publishHTML (target: [
			reportDir: 'target/site/jacoco',
			reportFiles: 'index.html',
			reportName: 'Jacoco Report'
		])
            }
        }
        stage ('Docker build') {
            steps {
                sh "/home/docker/bin/docker build -t alexfum/calc ."
            }
        }
        stage ('Docker push') {
            steps {
                sh "/home/docker/bin/docker push alexfum/calc"
            }
        }
        stage ('Deploy to staging') {
            steps {
                sh "/home/docker/bin/docker run -d --rm -p:8765:8080 --name calc alexfum/calc"
            }
        }
        stage ('Acceptance test') {
            steps {
		sleep 60
                sh "chmod +x acceptance_test.sh && ./acceptance_test.sh"
            }
        }
    }
}

