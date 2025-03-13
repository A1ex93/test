pipeline {
    agent any

    environment {
        // Переменные окружения для удобства
        NEXUS_URL = 'http://0.0.0.0:8081'
        REPOSITORY_NAME = 'testrepo'
        ARTIFACT_PATH = "com/example/myapp/${BUILD_NUMBER}/myapp"
        FILE_PATH = 'myapp'
    }

    stages {
        stage('Checkout') {
            steps {
                // Клонируем репозиторий с исходным кодом
                git 'https://github.com/a1ex93/test.git'
            }
        }

        stage('Build') {
            steps {
                // Собираем Go-приложение в бинарный файл
                sh '''
                    CGO_ENABLED=0 GOOS=linux go build -o myapp .
                '''
            }
        }

        stage('Upload to Nexus') {
            steps {
                // Загружаем бинарный файл в Nexus
                script {
                    sh """
                        curl -u admin:admin --upload-file ${FILE_PATH} \
                        "${NEXUS_URL}/repository/${REPOSITORY_NAME}/${ARTIFACT_PATH}"
                    """
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully. Binary file uploaded to Nexus.'
        }
        failure {
            echo 'Pipeline failed. Please check the logs for details.'
        }
    }
}
