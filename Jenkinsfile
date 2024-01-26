pipeline {
    environment {
        AWS_ACCOUNT_ID="408579600952"
        AWS_DEFAULT_REGION="us-east-1"
        IMAGE_REPO_NAME="onlinebook1"
        IMAGE_TAG="$BUILD_NUMBER"
        REPOSITORY_URI = "408579600952.dkr.ecr.us-east-1.amazonaws.com/onlinebook1"
    }
    agent any
    tools {
        maven 'maven-3.9.6'
    }
    stages {
        stage('cloning the git') {
            steps {
                git credentialsId: 'user', url: 'git@github.com:9963054406/onlinebookstore.git', branch: 'master'
            }
        }

        stage('maven build') {
            steps {
                script {
                    sh "mvn clean install"
                }
            }
        }
        stage('Dockerbuild') {
            steps {
                script {
                     sh "docker build -t ${IMAGE_REPO_NAME}:$BUILD_NUMBER ."
                    // sh "docker run -d -p 80:80 booksimage:$BUILD_NUMBER"
                }
            }
        }
        stage('Logging into AWS ECR') {
            steps {
                script {
                    sh """aws ecr get-login-password --region ${AWS_DEFAULT_REGION} | docker login --username AWS --password-stdin ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com"""
                }
                 
            }
        }
        stage('Pushing to ECR') {
            steps{  
                script {
                    sh """docker tag ${IMAGE_REPO_NAME}:${IMAGE_TAG} ${REPOSITORY_URI}:$IMAGE_TAG"""
                     sh """docker push ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/${IMAGE_REPO_NAME}:${IMAGE_TAG}"""
         }
        }
      }
      /*  stage ('K8S Deploy') {
          steps {
            script {
                withKubeConfig([credentialsId: 'k8s', serverUrl: '']) {
                sh ('kubectl apply -f  deployment.yaml')
                    }
                }
            }
         } */
    }
}
