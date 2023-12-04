pipeline {
    agent any

    stages {
        stage("Repo checkout") {
            steps {
                checkout scm
            }
        }

        stage('NPM Install') {
            steps {
                bat "npm install"
            }
        }

        stage('Run integration tests') {
            steps {
                bat "npm run test"
            }
        }

        stage('Build Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: '5518b6f1-d4be-455e-a97c-fcc5fc0bf9fa', passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
                    bat """docker build -t veselin88/student:1.0.0 .
                        docker login -u %DOCKER_USERNAME% --password %DOCKER_PASSWORD%
                        docker push veselin88/student:1.0.0"""
                }
            }
        }
        
        stage('Deploy Image') {
            steps {
              script {
                    // Prompt for input approval
                    input("Deploy to production?") 
                }
                withCredentials([usernamePassword(credentialsId: '5518b6f1-d4be-455e-a97c-fcc5fc0bf9fa', passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
                    bat """docker pull veselin88/student:1.0.0
                    docker run -d -p 8081:8081 veselin88/student:1.0.0 """
                }
            }
        }
    }
}