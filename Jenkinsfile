pipeline {
    agent any

    tools {

        maven 'Maven3'
    }

    environment {

        ALLURE_RESULTS_DIR = 'target/allure-results'
        ALLURE_REPORT_DIR = 'target/allure-report'
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Cloning repository...'
                git branch: 'master', url: 'https://github.com/QualityNinja/JenkinsCI.git'
            }
        }

        stage('Build') {
            steps {
                echo 'Building project...'
                bat 'mvn clean compile'
            }
        }

        stage('Run Tests') {
            steps {
                echo 'Running tests...'
                script {
                    try {

                        bat 'mvn test'
                    } catch (Exception e) {
                        echo "Тесты не пройдены, но ошибки игнорируются"
                    }
                }
            }
            post {
                always {
                    echo 'Archiving test results...'
                    junit '**/target/surefire-reports/*.xml'
                }
            }
        }

        stage('Generate Allure Report') {
            steps {
                echo 'Generating Allure Report...'
                script {
                    try {

                        bat 'mvn allure:report'
                    } catch (Exception e) {
                        echo 'Allure report generation failed'
                    }
                }
                allure([results: [[path: "${ALLURE_RESULTS_DIR}"]]])
            }
        }
    }

    post {
        success {
            echo 'Build completed successfully!'
        }
        failure {
            echo 'Build failed. Check logs and test results.'
        }
        always {
            archiveArtifacts artifacts: '**/target/**/*', allowEmptyArchive: true
        }
    }
}