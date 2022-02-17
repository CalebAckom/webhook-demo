pipeline {
  agent any 
    
  tools {nodejs "nodejs"}
    
  stages {     
     stage('Build') {
       steps {
         sh 'npm install'
       }
     }

    stage('SonarQube Analysis') {
      steps {
        withSonarQubeEnv('SonarQube') {
          script {
            def scannerHome = tool 'SonarScanner';
            sh "${scannerHome}/bin/sonar-scanner"
          }
        }
      }
    }

    stage('Prepare Deploy') {
      when {
        branch 'main'
      }
      steps {
         sh """
            """
          }
        }
      }
    }

    stage('Deploy To Development') {
      when {
        branch 'dev'
      }
      steps {
        withAWS(region:'eu-west-1',credentials:'aws-cred') {
          script {
            sh """
                aws deploy create-deployment \
                --application-name ${appname} \
                --deployment-config-name CodeDeployDefault.OneAtATime \
                --deployment-group-name ${deploy_group} \
                --file-exists-behavior OVERWRITE \
                --s3-location bucket=${s3_bucket},key=${s3_filename}.zip,bundleType=zip
              """
          }
        }
      }
    }
  
    stage('Clean WS') {
      steps {
        cleanWs()
      }
    }

  }
