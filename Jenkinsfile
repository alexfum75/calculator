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
                sh "/home/docker/bin/docker build -t alexfum/calc /var/jenkins_home/workspace/calculator"
            }
        }
        stage ('Docker push') {
            steps {
                sh "echo '/home/docker/bin/docker push alexfum/calc'"
            }
        }
        stage ('Deploy to staging') {
            steps {
                sh "echo '/home/docker/bin/docker run -d --rm -p:8765:8080 --name calc alexfum/calc'"
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
 	    sh "/home/docker/bin/docker stop calc"		
	}
    }
}

