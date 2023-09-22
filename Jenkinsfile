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
                    try {
                        sh 'docker build -t mypythonapp .'
                    } catch (Exception e) {
                        sh 'docker logs pythonapp'
                        throw e
                    }
                }
            }
        }
        stage('Run Docker Container') {
            steps {
                script {
                    try {
                        sh '''
                        docker stop pythonapp || true
                        docker rm pythonapp || true
                        docker run -d -p 5000:5000 --name=pythonapp mypythonapp
                        '''
                    } catch (Exception e) {
                        sh 'docker logs pythonapp'
                        throw e
                    }
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
