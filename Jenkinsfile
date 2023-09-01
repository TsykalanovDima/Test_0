pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Выполнить проверку кода из репозитория
                checkout scm
            }
        }
        stage('Build') {
            steps {
                // Сборка Docker-образа
                sh 'docker build -t dima_1 .'
            }
        }
        stage('Run Tests') {
            steps {
                // Запуск тестов внутри контейнера
                script {
                    docker.image('dima_1:latest').inside {
                        sh 'pytest -s /app/Test_0.py'
                    }
                }
            }
        }
        stage('Generate Allure Report') {
            steps {
                script {
                    sh 'allure generate /app/allure-results -o /app/allure-report --clean'
                    archiveArtifacts allowEmptyArchive: true, artifacts: 'allure-report/**'
                }
            }
        }
        stage('View Allure Report') {
            steps {
                script {
                    allure([
                        includeProperties: false,
                        jdk: '',
                        properties: [],
                        reportBuildPolicy: 'ALWAYS',
                        results: [[path: 'allure-results']]
                    ])
                }
            }
        }
    }
}
