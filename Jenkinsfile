pipeline {
    agent any
    environment {
        DOCKER_REGISTRY = "docker.io"
        DOCKER_IMAGE_NAME = "vadakkan01/notesapp"
        DOCKER_IMAGE_TAG = "0.1"
    }
    stages {
        stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: 'master']], userRemoteConfigs: [[url: 'https://github.com/adithyanrk67/notesapp.git']]])
            }
        }
        stage('Build') {
            steps {
                sh "docker-compose build"
            }
        }
        stage('Test') {
            steps {
                sh "docker-compose up -d"
                sh "docker-compose exec -ti web pytest"
            }
        }
        stage('Push') {
            steps {
                withCredentials([string(credentialsId: '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', variable: 'GITHUB_TOKEN'), string(credentialsId: 'dckr_pat_vtIhNrEeCB4oxKh4U3YFLGFpnck', variable: 'DOCKER_PASSWORD')]) {
                    sh "docker login -u vadakkan01 -p $DOCKER_PASSWORD $DOCKER_REGISTRY"
                    sh "docker tag ${DOCKER_IMAGE_NAME}:${DOCKER_IMAGE_TAG} ${DOCKER_REGISTRY}/${DOCKER_IMAGE_NAME}:${DOCKER_IMAGE_TAG}"
                    sh "docker push ${DOCKER_REGISTRY}/${DOCKER_IMAGE_NAME}:${DOCKER_IMAGE_TAG}"
                }
            }
        }
    }
}
