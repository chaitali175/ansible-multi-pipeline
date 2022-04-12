pipeline {
    agent any 
     tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "localmaven"
        git "Default"
    }
    stages {
        stage('SCM Checkout') {
            steps{
                git branch: 'mychatapp', credentialsId: 'git-token', url: 'https://github.com/chaitali175/mychatapp.git'
            }
        }
         stage('MVN Build') {
            steps{
                
                sh "mvn -Dmaven.test.failure.ignore=true clean package"
                sh "mv target/websocket-demo-0.0.1-SNAPSHOT.jar target/chatapp.jar"
            }
        }
        
         stage('MVN Testing') {
          steps{
             sh "mvn test"
          }
       }
       stage('test report') {
          steps{
             junit 'target/surefire-reports/*.xml'
          }
       }
       stage('archive') {
          steps{
             archiveArtifacts 'target/*.jar'
              
          }
       }
              
        stage('Get ansible code') 
          steps{
             
                 git branch: 'master', credentialsId: 'git-token', url: 'https://github.com/chaitali175/tomcatsetup.git'
          }
       }
       stage('execute ansible') {
            
          steps{
             ansiblePlaybook credentialsId: 'keypair', disableHostKeyChecking: true, installation: 'localAnsible', inventory: 'dev.inv', playbook: 'tomcat-setup.yml'
                
              }

          }
       
  
    }
  
        
    }
    
