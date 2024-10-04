pipeline {
    agent any

        environment {
        IMAGE_TAG = "${BUILD_NUMBER}"
    }

    stages {
        
       
        stage('Docker Build') {
            steps {
                sh "docker build . -t rajashekhar36/new-argo-image:$IMAGE_TAG"
            }
        }
        stage('Docker Push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'hubPwd')]) {
                   sh '''
                       echo "${hubPwd}" | docker login -u ${DOCKER_USERNAME} --password-stdin
                   '''
                   sh "docker push rajashekhar36/new-argo-image:${IMAGE_TAG}"
                }
            }
        }
    }
} 
