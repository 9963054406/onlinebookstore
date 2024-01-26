pipeline {
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
                    // sh "docker build -t booksimage:$BUILD_NUMBER ."
                    sh "docker run -d -p 80:80 booksimage:$BUILD_NUMBER"
                }
            }
        } 
        stage ('K8S Deploy') {
          steps {
            script {
                withKubeConfig([credentialsId: 'k8s', serverUrl: '']) {
                sh ('kubectl apply -f  deployment.yaml')
                    }
                }
            }
         }
    }
}
