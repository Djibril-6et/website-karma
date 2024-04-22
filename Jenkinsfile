pipeline {
    agent any

    environment {
        // Variables d'environnement nécessaires
        ID_DOCKER = "${ID_DOCKER_PARAMS}"
        IMAGE_NAME = "website-karma"
        IMAGE_TAG = "latest"
        DOCKERHUB_PASSWORD = "${DOCKERHUB_PASSWORD_PSW}"
        RENDER_API_TOKEN = credentials('RENDER_API_TOKEN')
        RENDER_SERVICE_ID = "srv-coh65v6v3ddc73fi6pu0"
        RENDER_DEPLOY_HOOK_URL_FINAL1 = credentials('RENDER_DEPLOY_HOOK_URL_FINAL1')
    }

    triggers {
        // Vérifie le dépôt pour des changements toutes les 2 minutes
        pollSCM('H/2 * * * *')
    }

    stages {
        stage('Build image') {
            steps {
                script {
                    sh 'docker build -t ${ID_DOCKER}/${IMAGE_NAME}:${IMAGE_TAG} .'
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
                    sh '''
                        echo ${DOCKERHUB_PASSWORD} | docker login -u ${ID_DOCKER} --password-stdin
                        docker push ${ID_DOCKER}/${IMAGE_NAME}:${IMAGE_TAG}
                    '''
                }
            }
        }

       

    }

   
}
