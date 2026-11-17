pipeline {
    agent any

    tools {
        jdk   'JDK21'
        maven 'M3'
    }

    triggers {
        githubPush()
    }

    stages {
        stage('Checkout GitHub') {
            steps {
                checkout scm
            }
        }

        stage('Tests unitaires') {
            steps {
                dir('backend') { sh 'mvn clean test' }
            }
            post {
                always {
                    junit allowEmptyResults: true,
                          testResults: 'backend/target/surefire-reports/*.xml'
                }
            }
        }

        stage('Livrable (target/)') {
            steps {
                dir('backend') {
                    sh 'mvn package -DskipTests'
                    sh 'ls -lh target/*.jar'
                }
            }
        }
    }

    post {
        success {
            archiveArtifacts artifacts: 'backend/target/*.jar', fingerprint: true
        }
    }
}
