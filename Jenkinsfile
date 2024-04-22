pipeline {
    agent any

    environment {
        // Variables d'environnement nécessaires
        ID_DOCKER = "%ID_DOCKER_PARAMS%"
        IMAGE_NAME = "website-karma"
        IMAGE_TAG = "latest"
        DOCKERHUB_PASSWORD = "%PASS_DOCKER_PARAMS%"
    }

    stages {
        stage('Build image') {
            steps {
                script {
                    bat 'docker build -t %ID_DOCKER%/%IMAGE_NAME%:%IMAGE_TAG% .'
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

        stage('Login and Push Image on docker hub') {
            steps {
                script {
                    bat '''
                        echo %DOCKERHUB_PASSWORD% | docker login -u %ID_DOCKER% --password-stdin
                        docker push %ID_DOCKER%/%IMAGE_NAME%:%IMAGE_TAG%
                    '''
                }
            }
        }

    }
}
