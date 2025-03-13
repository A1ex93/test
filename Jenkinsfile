pipeline {
    agent any

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
                sh 'CGO_ENABLED=0 GOOS=linux go build -o myapp .'
            }
        }

        stage('Upload to Nexus') {
            steps {
                // Загружаем бинарный файл в Nexus
                script {
                    def serverUrl = "http://0.0.0.0:8081"
                    def repositoryName = "testrepo"
                    def filePath = "myapp"
                    def artifactPath = "com/example/myapp/${BUILD_NUMBER}/myapp"

                    withCredentials([usernamePassword(credentialsId: 'nexus-credentials', usernameVariable: 'admin', passwordVariable: 'admin')]) {
                        sh """
                            curl -u ${NEXUS_USER}:${NEXUS_PASSWORD} --upload-file ${filePath} \
                            "${serverUrl}/repository/${repositoryName}/${artifactPath}"
                        """
                    }
                }
            }
        }
    }
}
