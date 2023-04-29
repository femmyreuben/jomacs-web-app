pipeline{
  agent any
  tools{
    maven "maven3.8.8"
  }

    stages{
      stage("1. Git clone from repo"){
        steps{
         sh "echo start of git clone"
         git branch: 'main', url: 'https://github.com/femmyreuben/jomacs-web-app.git'
         sh "echo end of git clone"
        }
      }
      
      stage("2. Build from Maven"){
        steps{
          sh "echo start building from Maven"
          sh "mvn clean package"
          sh "echo end of build"
        }  
      }
      
      stage("3. Code Scan"){
        steps{
          sh "echo start of code scan"
          sh "mvn sonar:sonar"
          sh "echo end of code scan"
        }  
      }
      
      stage("4. Store Artifacts"){
        steps{
           sh "echo Deploy Artifact"
           sh "mvn deploy"
        }  
      }
      
      stage("5. Deploying to Tomcat in UAT"){
        steps{
           sh "echo start deploying to server in UAT Env"
           deploy adapters: [tomcat9(credentialsId: 'tomcat_cred', path: '', url: 'http://18.117.162.68:9090')], contextPath: null, war: 'target/*.war'
        }  
      }
      
      stage("6. Email Notification"){
        steps{
            sh "echo Email Notification to DevOps Team"
            emailext body: 'The Deployment is Successful', subject: 'Deployment Success', to: 'info@jomacsit.com'
        }  
      }
    }
}
