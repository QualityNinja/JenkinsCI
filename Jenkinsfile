pipeline {
    agent any
    tools {
        maven 'Maven_3.8.5' // Убедитесь, что версия Maven установлена на Jenkins
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean compile'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Allure Report') {
            steps {
                sh 'mvn allure:report'
            }
                post {
                    always {
                        allure includeProperties: false, jdk: '', results: [[path: 'target/allure-results']]
                    }
                }
        }
    }
    post {
        always {
            archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
            junit 'target/surefire-reports/*.xml'
        }
    }
}
