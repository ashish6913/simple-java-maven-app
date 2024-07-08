pipeline {
    agent {
        label 'agent1'
    }
    options {
        skipStagesAfterUnstable()
        onMonit()
    }
    environment {
        MAVEN_HOME = '/opt/apache-maven-3.9.5'
        PATH = "${env.MAVEN_HOME}/bin:${env.PATH}"
    }
    stages {
        stage('Verify Maven') {
            steps {
                sh 'echo $MAVEN_HOME'
                sh 'mvn -v'
                sh 'which mvn'
            }
        }
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
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Deliver') { 
            steps {
                sh './jenkins/scripts/deliver.sh' 
            }
        }
    }
}