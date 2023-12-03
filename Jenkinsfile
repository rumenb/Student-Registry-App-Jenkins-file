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
                withCredentials([usernamePassword(credentialsId: '686767b9-b3fa-4c71-a7e2-c511b6eaf625', passwordVariable: 'DOCKER_PASS', usernameVariable: 'DOCKER_USER')]) {
                    bat """docker build -t dimosoftuni/student:1.0.0 .
                        docker login -u %DOCKER_USER% --password %DOCKER_PASS%
                        docker push dimosoftuni/student:1.0.0"""
                }
            }
        }
        
        stage('Deploy Image') {
            steps {
              script {
                    // Prompt for input approval
                    input("Deploy to production?") 
                }
                withCredentials([usernamePassword(credentialsId: '686767b9-b3fa-4c71-a7e2-c511b6eaf625', passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
                    bat """docker pull dimosoftuni/student:1.0.0
                    docker run -d -p 8081:8081 dimosoftuni/student:1.0.0 """
                }
            }
        }
    }
}