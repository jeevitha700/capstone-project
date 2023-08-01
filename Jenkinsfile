pipeline {
    agent any
    tools {
        nodejs 'nodejs'
    }
    stages {
        stage('Build') {
            steps {
                checkout scmGit(branches: [[name: '*/dev']], extensions: [], userRemoteConfigs: [[credentialsId: 'Github', url: 'https://github.com/jeevitha700/capstone-project.git']])
                sh 'npm install'
                // sh 'npm run build'
            }
        }
        stage('Test') {
            steps {
                // sh 'npm run test'
                echo "Test"
            }
        }
       stage('Build Image') {
            steps {
                sh 'docker image prune'
                sh 'docker build -t reactimage .'
                sh 'docker tag reactimage:latest jeevithals25/dev:latest'
            }    
       }
       stage('Docker login') {
            steps { 
                withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                sh "echo $PASS | docker login -u $USER --password-stdin"
                sh 'docker push jeevithals25/dev:latest'
                }
            }
       }
       stage('Deploy') {
            steps {  
                script {
                   def dockerCmd = 'docker run -itd --name My-first-container -p 80:5000 jeevithals25/dev:latest'
                   sshagent(['3.109.181.189']) {
                   sh "ssh -o StrictHostKeyChecking=no ubuntu@ $13.235.128.224{dockerCmd}"
                   }
                }
            }
       }
    }
}
