pipeline {
    environment {
        ID_DOCKER = "djibrilci"
        IMAGE_NAME = "website-karma"
        IMAGE_TAG = "latest"
        DOCKER_PASSWORD = 'Lmdp$Pdoc1'
        PORT_EXPOSED = 80
    }
    agent any
    stages {
        stage('Build image') {
            steps {
                script {
                    bat 'docker build -t %ID_DOCKER%/%IMAGE_NAME%:%IMAGE_TAG% .'
                }
            }
        }
        stage('Run container based on built image') {
            steps {
                script {
                    bat '''
                    echo "Clean Environment"
                    docker rm -f %IMAGE_NAME% || echo "container does not exist"
                    docker run --name %IMAGE_NAME% -d -p %PORT_EXPOSED%:80 -e PORT=80 %ID_DOCKER%/%IMAGE_NAME%:%IMAGE_TAG%
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
            steps {
                script {
                    bat '''
                    docker stop %IMAGE_NAME%
                    docker rm %IMAGE_NAME%
                    '''
                }
            }
        }
        stage('Login and Push Image on docker hub') {
            steps {
                script {
                    bat '''
                        docker login -u %ID_DOCKER% -p %DOCKER_PASSWORD%
                        docker push %ID_DOCKER%/%IMAGE_NAME%:%IMAGE_TAG%
                    '''
                }
            }
        }

        stage('Push image in staging and deploy it') {
            when {
                expression { GIT_BRANCH == 'origin/master' }
            }
            agent any
            environment {
                RENDER_STAGING_DEPLOY_HOOK = credentials('render_karma_key')
            }
            steps {
                powershell '''
                Write-Output "Staging"
                Write-Output $env:RENDER_STAGING_DEPLOY_HOOK
                Invoke-WebRequest -Uri $env:RENDER_STAGING_DEPLOY_HOOK
                '''
            }
        }

    }
}
