pipeline {
    agent {
        docker {
            image 'maven:3-alpine' 
            args '-v /root/.m2:/root/.m2' 
        }
    }
    //As a general principle, it’s a good idea to keep your Pipeline code (i.e. the Jenkinsfile) as tidy as possible and place more 
    //complex build steps (particularly for stages consisting of 2 or more steps) into separate shell script files like the deliver.sh 
    //file. This ultimately makes maintaining your Pipeline code easier, especially if your Pipeline gains more complexity
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
                    input message: 'Testing Recorded. Please review results. Ready to run deliver.sh and create the Java application? (Click "Proceed" to continue)' 
                }
            }
        }
        //runs the shell script deliver.sh located in the jenkins/scripts directory from the root of the simple-java-maven-app repository.
        stage('Deliver') {
            steps {
                sh './jenkins/scripts/deliver.sh'
            }
        }

    }
}