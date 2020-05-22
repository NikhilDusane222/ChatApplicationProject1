pipeline {
    agent any
    stage ('Build') {
        	sh "echo 'shell scripts to build project...'"
               }
         
         stage ('Tests') {
        	sh "echo 'shell scripts to Testing..'"
               }
    
     stages {
         stage('Sonarqube') {
           environment {
                scannerHome = tool 'SonarScanner'
                }
         steps {
            withSonarQubeEnv('sonarqube') {
                sh "${scannerHome}/bin/sonar-scanner"
            }
            timeout(time: 5, unit: 'MINUTES') {
                waitForQualityGate abortPipeline: true
            }
          }
         }
         
         
         stage('Deploy') { 
               steps {
                 sh ''' #! /bin/bash 
                 aws deploy create-deployment --application-name TFChatApp --deployment-group-name TFCodeDeployGroup --deployment-config-name CodeDeployDefault.AllAtOnce --github-location repository=NikhilDusane222/ChatApplicationProject1,commitId=${GIT_COMMIT}
                 '''
                }
            }

         stage('status'){
                steps {
                sh ''' #! /bin/bash
                echo Deployment started
                '''
                }  
            }

        }
        post { 
            always { 
                echo 'Stage is success'
            }
        }    
    }
