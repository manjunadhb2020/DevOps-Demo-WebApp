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
                sh "mv target/*.war target/casestudyweb.war"
            }
        }   
        
        stage('Deploy-test') {
                   steps {
                       sshagent(['tomcat-test-server-deploy']) {
                   sh 'scp -o StrictHostKeyChecking=no target/casestudyweb.war testserver@40.88.6.134:/opt/tomcat8/webapps/'
                   sh 'ssh testserver@40.88.6.134 /opt/tomcat8/bin/shutdown.sh'
                   sh 'ssh testserver@40.88.6.134 /opt/tomcat8/bin/startup.sh'  
                
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
