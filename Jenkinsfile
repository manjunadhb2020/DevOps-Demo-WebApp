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
                // sh "mv target/*.war target/QAWebapp.war"
            }
        }   
        
        stage('Deploy-test') {
                  steps {
                      sshagent(['tomcat-test-server-deploy']) {
                  sh 'scp -o StrictHostKeyChecking=no target/AVNCommunication-1.0.war testserver@40.88.6.134:/var/lib/tomcat8/webapps/QAWebapp.war'
                   sh 'scp -o StrictHostKeyChecking=no -r target/AVNCommunication-1.0 testserver@40.88.6.134:/var/lib/tomcat8/webapps/QAWebapp'
                   
                
                }
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

