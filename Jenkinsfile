pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    sh '''
                    docker build -t mypythonapp .
                    '''
                }
            }
        }
        stage('Run Docker Container') {
            steps {
                script {
                    // Stop y remove el contenedor si ya está ejecutándose
                    sh '''
                    docker stop pythonapp || true
                    docker rm pythonapp || true
                    docker run -d -p 5000:5000 --name=pythonapp mypythonapp
                    '''
                }
            }
        }
    }
    post {
        always {
            sh "docker rm -f pythonapp || true"
        }
    }
}
