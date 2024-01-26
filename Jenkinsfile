pipeline {
    environment {
        //once you sign up for Docker hub, use that user_id here
        registry = "408579600952.dkr.ecr.us-east-1.amazonaws.com/onlinebook1"
        //- update your credentials ID after creating credentials for connecting to Docker Hub
        registryCredential = '408579600952'
        dockerImage = ''
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
                     sh "docker build -t booksimage:$BUILD_NUMBER ."
                    // sh "docker run -d -p 80:80 booksimage:$BUILD_NUMBER"
                }
            }
        }
        stage('Docker Image push') {
            steps {   
                script {
                     docker.withRegistry( '', registryCredential ) {
                     dockerImage.push()
                    }    
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
