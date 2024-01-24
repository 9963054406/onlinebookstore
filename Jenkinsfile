pipeline {
    agent any
    tools {
        maven 'maven-3.9.6'
    }
    stages {
        stage('cloning the git') {
            steps {
                git credentialsId: 'user', url: 'git@github.com:9963054406/sprint-deployment.git', branch: 'master'
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
                    sh "docker build -t testimage:v1 ."
                }
            }
        }
    }
}
