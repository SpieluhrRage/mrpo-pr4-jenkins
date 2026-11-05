pipeline {
    agent any

    options {
        disableConcurrentBuilds()
        skipDefaultCheckout()
    }

    environment {
        PROJECT_NAME = 'MRPO Practice 4'
    }

    stages {
        stage('Получение проекта') {
            steps {
                checkout scm
                sh 'ls -la'
            }
        }

        stage('Сборка') {
            steps {
                echo "Проект: ${env.PROJECT_NAME}"
                echo "Номер сборки: ${env.BUILD_NUMBER}"
                echo 'Имитация сборки приложения'
            }
        }

        stage('Тестирование') {
            steps {
                sh 'test -f deploy && test -f smoke-tests'
                sh 'sh -n deploy && sh -n smoke-tests'
                echo 'Файлы найдены, синтаксис скриптов проверен'
                echo 'Имитация тестирования приложения завершена'
            }
        }

        stage('Деплой в staging') {
            steps {
                sh 'chmod +x deploy smoke-tests'
                sh './deploy staging'
                sh './smoke-tests'
            }
        }

        stage('Подтверждение выпуска') {
            steps {
                timeout(time: 30, unit: 'MINUTES') {
                    input message: 'Продолжить выпуск в production?',
                          ok: 'Продолжить'
                }
            }
        }

        stage('Деплой в production') {
            steps {
                sh './deploy prod'
            }
        }
    }

    post {
        always {
            echo 'Выполнение Pipeline завершено'
        }

        success {
            echo 'Все этапы выполнены успешно'
        }

        failure {
            echo 'Произошла ошибка. Посмотрите Console Output'
        }

        aborted {
            echo 'Выполнение отменено или истекло время ожидания'
        }

        cleanup {
            deleteDir()
        }
    }
}
