pipeline {
    agent any

    tools {
        // Define the Node.js tool installation
        nodejs 'node'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/animesh0406/project_Dev.git']])
            }
        }
        
        stage('Client Tests') {
            steps {
                dir('client') {
                    sh 'npm install'
                    // Add your npm test step here
                }
            }
        }
        
        stage('Server Tests') {
            steps {
                dir('server') {
                    sh 'npm install'
                    // Add your npm test step here
                }
            }
        }
        
        stage('Build Images') {
            steps {
                sh 'docker build -t animesh0406/productivity-app:client-latest client'
                sh 'docker build -t animesh0406/productivity-app:server-latest server'
            }
        }
        
        stage('Push Images to DockerHub') {
            steps {
                withCredentials([usernamePassword(credentialsId: '3bda2239-38ef-4f58-985c-fce764f0f67a', passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
                    sh 'docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD'
                    sh 'docker push animesh0406/productivity-app:client-latest'
                    sh 'docker push animesh0406/productivity-app:server-latest'
                }
            }
        }
    }
}
