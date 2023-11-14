node {
  stage('SCM') {
    checkout scm
  }
  stage('SonarQube Analysis') {
    def scannerHome = tool 'SonarScanner';
    withSonarQubeEnv() {
      sh "${scannerHome}/bin/sonar-scanner"
    }
  }
  stage('Build') { 
     steps { 
       script{
         app = docker.build("underwater")
        }
      }
    }
   stage('Deploy') {
     steps {
       script{
         docker.withRegistry('https://685769770343.dkr.ecr.ap-south-1.amazonaws.com/testrepo', 'ecr:us-east-2:aws-credentials') {
           app.push("${env.BUILD_NUMBER}")
           app.push("latest")
         }
       }
     }
   }
  
}
