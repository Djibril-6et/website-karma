pipeline {
    environment {
        ID_DOCKER = "${ID_DOCKER_PARAMS}"
        IMAGE_NAME = "website-karma"
        IMAGE_TAG = "latest"
    }
    agent none
    stages {
        stage('Build image') {
            agent any
            steps {
                script {
                    bat 'docker build -t %ID_DOCKER%/%IMAGE_NAME%:%IMAGE_TAG% .'
                }
            }
        }
        stage('Run container based on built image') {
            agent any
            steps {
                script {
                    bat '''
                    echo "Clean Environment"
                    docker rm -f %IMAGE_NAME% || echo "container does not exist"
                    docker run --name %IMAGE_NAME% -d -p %PORT_EXPOSED%:80 -e PORT=80 %ID_DOCKER%/%IMAGE_NAME%:%IMAGE_TAG%
                    timeout /t 5 /nobreak > NUL
                    '''
                }
            }
        }
        stage('Test image') {
            steps {
                script {
                    echo "Exécution des tests"
                }
            }
        }
        stage('Clean Container') {
            agent any
            steps {
                script {
                    bat '''
                    docker stop %IMAGE_NAME%
                    docker rm %IMAGE_NAME%
                    '''
                }
            }
        }
        stage ('Login and Push Image on docker hub') {
            agent any
            environment {
                DOCKERHUB_PASSWORD  = "${PASS_DOCKER_PARAMS}"
            }
            steps {
                script {
                    bat '''
                    docker push %ID_DOCKER%/%IMAGE_NAME%:%IMAGE_TAG%
                    '''
                }
            }
        }    
    }
}
