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
        stage('Checkout K8S manifest SCM'){
            steps {
                git credentialsId: 'github-credentials', 
                url: 'https://github.com/Reddylab36/Hiring-app-argocd.git',
                branch: 'main'
            }
        } 
        stage('Update K8S manifest & push to Repo'){
            steps {
                script{
                   withCredentials([usernamePassword(credentialsId: 'github-credentials', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) { 
                        sh '''
                        cat deployment.yaml
                        sed -i "s/5/${BUILD_NUMBER}/g" deployment.yaml
                        cat deployment.yaml
                        git add .
                        git commit -m 'Updated the deploy yaml | Jenkins Pipeline'
                        git remote -v
                        git push https://github.com/Reddylab36/Hiring-app-argocd.git HEAD:main
                        '''                        
                      }
                  }
            }   
        } 
    }
} 
