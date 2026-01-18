pipeline {
    agent any
    parameters {
        string(name: 'TAG', defaultValue: 'latest')
    }

    environment {
        REGISRTY = 'anestesia01/blog-go'
        TOKEN = credentials('docker-token')
        PRJ_NAME = 'simple-go'
        GIT_URL = 'git@github.com:AnastasiyaGapochkina01/go-app-cicd.git'
        DOCKER_USER = 'anestesia01'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: "${env.GIT_URL}", credentialsId: 'jenkins-key'
            }
        }
        stage('Run tests') {
            steps {
                script {
                    sh """
                        go mod init simple-app
                        go mod tidy
                        go test ./ -v
                    """
                }
            }
        }
        stage('Build and push image') {
            steps {
                script {
                    sh """
                        docker login -u ${env.DOCKER_USER} -p ${env.TOKEN}
                        docker build -t ${env.REGISRTY}:${params.TAG} ./
                        docker push ${env.REGISRTY}:${params.TAG}
                        docker logout
                    """
                }
            }
        }
        stage('Run app'){
            steps {
                script {
                    sh """
                        docker rm ${env.PRJ_NAME} -f || true
                        docker run -d -it --name ${env.PRJ_NAME}
                    """
                }
            }
        }
    }
}
