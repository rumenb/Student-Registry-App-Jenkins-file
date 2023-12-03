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
                withCredentials([usernamePassword(credentialsId: '686767b9-b3fa-4c71-a7e2-c511b6eaf625', passwordVariable: 'docker_pass', usernameVariable: 'docker_user')]) {
                    bat """docker build -t rumenbalev/studen-reg-app:1.0.0 .
                           docker push rumenbalev/studen-reg-app:1.0.0"""
                }
            }
        }
        
        stage('Deploy Image') {
            steps {
              script {
                    // Prompt for input approval
                    input("Deploy to production?") 
                }
                withCredentials([usernamePassword(credentialsId: '686767b9-b3fa-4c71-a7e2-c511b6eaf625', passwordVariable: 'docker_pass', usernameVariable: 'docker_user')]) {
                    bat """docker pull rumenbalev/studen-reg-app:1.0.0
                    docker run -d -p 8081:8081 rumenbalev/studen-reg-app:1.0.0 """
                }
            }
        }
    }
}