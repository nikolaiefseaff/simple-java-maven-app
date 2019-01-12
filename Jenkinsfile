pipeline {
    agent {
        docker {
            image 'maven:3-alpine' 
            args '-v /root/.m2:/root/.m2' 
        }
    }
    stages {
        stage('Build') { 
            steps {
                sh 'mvn -B -DskipTests clean package' 
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                //ensures that the step is always executed at the completion of the Test stage, regardless of the stage’s outcome.
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }

    }
}