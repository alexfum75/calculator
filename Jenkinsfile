pipeline {
    agent any

     stages {
        stage('Build') {
            steps {
                // Run Maven on a Unix agent.
                sh "/tmp/apache-maven-3.6.3/bin/mvn package"

                // To run Maven on a Windows agent, use
                // bat "mvn -Dmaven.test.failure.ignore=true clean package"
            }
        }
        stage ('Unit Test') {
            steps {
                sh "/tmp/apache-maven-3.6.3/bin/mvn test"
            }
        }
    }
}

