
pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/a1ex93/test.git'
            }
        }
        stage('Build') {
            steps {
                sh 'docker build . -t ubuntu-bionic:8082/hello-world:v$BUILD_NUMBER'
            }
        }
    }
}
