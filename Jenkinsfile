pipeline {
    agent any

    stages {
        stage('checkout Code') {
            steps {
                checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[credentialsId: 'shir-key', url: 'https://gitlab.com/shirs-group/trainmefordevsecops.git']])
            }
        }    
        stage('SAST') {
            steps {
                sh ''  
            }
        }
        stage('build and tag') {
            steps {
                sh ''  
            }
        }
        stage('image and vulunerabillity scan') {
            steps {
               sh '' 
            }
        }
        stage('post to docker hub') {
            steps {
               sh ''   
            }
        }
        stage('pull image server') {
            steps {
              sh ''  
            }
        } 
        stage('DAAST') {
            steps {
               sh ''   
            }
        }
    }
}
