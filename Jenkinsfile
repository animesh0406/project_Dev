pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/animesh0406/project_Dev.git']])
            }
        }
        
        stage('Client Tests') {
              agent{
        docker {
            // Use the official Node.js image from Docker Hub
            image 'node:latest'
            // Set up other Docker-related options if needed
            // For example, you can specify additional volumes or environment variables
            // Additional options can be added as needed
            args '-u root:root' // (optional) Run Docker container as root user
        }
    }

            steps {
                dir('client') {
                    sh 'npm install'
                    // Add your npm test step here
                }
            }
        }
        
        stage('Server Tests') {
               agent{
        docker {
            // Use the official Node.js image from Docker Hub
            image 'node:latest'
            // Set up other Docker-related options if needed
            // For example, you can specify additional volumes or environment variables
            // Additional options can be added as needed
            args '-u root:root' // (optional) Run Docker container as root user
        }
    }
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
