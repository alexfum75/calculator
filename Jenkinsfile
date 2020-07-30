pipeline {
    agent any
     triggers {
        pollSCM ('* * * * *')
     }
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
                sh "/home/docker/bin/docker build -t alexfum/calc:${BUILD_TIMESTAMP} /var/jenkins_home/workspace/calculator"
            }
        }
        stage ('Docker push') {
            steps {
                sh "/home/docker/bin/docker push alexfum/calc:${BUILD_TIMESTAMP}"
            }
        }
        stage ('Update version') {
            steps {
                sh "echo 'sed -i ''s/{{VERSION}}/${BUILD_TIMESTAMP}/g'' calc.yaml'"
            }
        }
        stage ('Deploy to staging') {
            steps {
                sh "echo '/home/docker/bin/docker run -d --rm -p:8765:8080 --name calc alexfum/calc:${BUILD_TIMESTAMP}'"
            }
        }
        stage ('Parallelization') {
            steps {
		parallel {
			one: {echo "Parallel step 1"},
			two: {echo "Parallel step 2"}
		}
            }
        }
        stage ('Acceptance test') {
            steps {
		sleep 60
                sh "echo 'chmod +x /var/jenkins_home/workspace/calculator/acceptance_test.sh && /var/jenkins_home/workspace/calculator/acceptance_test.sh'"
            }
        }
    }
    post {
        always {
 	    sh "/home/docker/bin/docker stop alexfum/calc"		
	}
    }
}

