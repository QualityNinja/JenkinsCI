pipeline {
    agent any
    tools {
        maven 'Maven_3.8.5' // Название инструмента Maven, настроенного в Jenkins
    }
    stages {
        stage('Clone repository') {
            steps {
                git 'https://github.com/username/repository-name.git' // Ваш репозиторий
            }
        }
        stage('Build and Run Tests') {
            steps {
                sh 'mvn clean test'
            }
        }
        stage('Generate Allure Report') {
            steps {
                allure([
                    includeProperties: false,
                    jdk: '',
                    results: [[path: 'target/allure-results']]
                ])
            }
        }
    }
    post {
        always {
            junit '**/target/surefire-reports/*.xml' // Отчеты JUnit
            archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
        }
    }
}
