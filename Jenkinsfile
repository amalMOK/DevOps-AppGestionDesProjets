pipeline {
    agent any

    tools {
        jdk   'JDK21'
        maven 'M3'
    }

    triggers {
        githubPush()
    }

    environment {
        IMAGE = 'amalmok2004/gestion-projets-backend'
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

        stage('Build image Docker') {
            steps {
                dir('backend') {
                    sh "docker build -t ${IMAGE}:${BUILD_NUMBER} -t ${IMAGE}:latest ."
                }
            }
        }

        stage('Push Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds',
                                                  usernameVariable: 'DH_USER',
                                                  passwordVariable: 'DH_TOKEN')]) {
                    sh 'echo "$DH_TOKEN" | docker login -u "$DH_USER" --password-stdin'
                    retry(3) {
                        sh "docker push ${IMAGE}:${BUILD_NUMBER}"
                    }
                    sh "docker push ${IMAGE}:latest"
                }
            }
        }
    }

    post {
        success {
            archiveArtifacts artifacts: 'backend/target/*.jar', fingerprint: true
        }
        always {
            sh 'docker logout || true'
        }
    }
}
