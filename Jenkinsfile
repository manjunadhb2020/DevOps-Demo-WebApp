pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'github', url: 'https://github.com/manjunadhb2020/DevOps-Demo-WebApp.git']]])
                  }
            }
         stage('Sonarqube') {
            environment {
                scannerHome = tool 'sonarqubescanner'
            }
            steps {
                withSonarQubeEnv('sonarqube') {
                    sh """${scannerHome}/bin/sonar-scanner"""
                }
                timeout(time: 10, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
                               
       stage('build') {
            steps {
                echo 'building the applicaiton...'
                sh 'mvn clean install' 
            }
        }   
        
        stage('Deploy - Staging') {
            steps {
                sh './deploy staging'
               echo 'deploying the application'
            }
        }
        
        stage('test') {
            steps {
                echo 'testing the application...'
               echo 'testing'
            }
        }        
    }
}
