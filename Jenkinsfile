pipeline {
    agent any
    environment {
        APP_NAME = "space-connect-app"
        APP_PORT = "5000"
        CONTAINER_NAME = "space-connect-running"
    }
    stages {
        stage('Build') {
            steps {
                git branch: 'main', url: 'https://github.com/Lamataa/space-connect-app.git'
                echo 'Build da imagem Docker...'
                sh 'docker build -t ${APP_NAME}:latest .'
            }
        }
        stage('Test') {
            steps {
                echo 'Executando testes...'
                sh '''
                    docker stop space-connect-test || true
                    docker rm space-connect-test || true
                    docker run -d --name space-connect-test -p 5001:5000 ${APP_NAME}:latest
                    sleep 5
                    curl -f http://172.17.0.1:5001/ || exit 1
                    curl -f http://172.17.0.1:5001/health || exit 1
                    echo "Testes passaram!"
                    docker stop space-connect-test
                    docker rm space-connect-test
                '''
            }
        }
        stage('Deploy Simulado') {
            steps {
                echo 'Realizando deploy...'
                sh '''
                    docker stop ${CONTAINER_NAME} || true
                    docker rm ${CONTAINER_NAME} || true
                    docker run -d \
                        --name ${CONTAINER_NAME} \
                        -p ${APP_PORT}:5000 \
                        ${APP_NAME}:latest
                    echo "Deploy realizado!"
                '''
            }
        }
    }
    post {
        success { echo 'Pipeline executado com sucesso!' }
        failure { echo 'Pipeline falhou. Verifique os logs.' }
    }
}