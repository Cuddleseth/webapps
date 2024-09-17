pipeline{
agent any
tools{
  maven 'Maven'
}
  stages{
    stage ('Initialized'){
      steps{
        sh '''
              echo "PATH =${PATH}"
              echo "M2_HOME = ${M2_HOME}"
        '''
      }
    }
    stage('Check-Git-Secret'){
      steps{
        sh'rm trufflehog || true'
        sh'docker run gesellix/trufflehog --json https://github.com/Cuddleseth/webapps.git > trufflehog'
        sh 'cat trufflehog'
      }
    }
    stage('Build'){
      steps{
      sh 'mvn clean package'
      }
      post{
        success{
          echo "Archiving the artifact"
          archiveArtifacts artifacts:'**/*.war'
        }
      }
    }
    stage('Deploy to tomcat server'){
      steps{
       deploy adapters: [tomcat9(credentialsId: 'tomcat', path: '', url: 'http://72.20.224.1:9090/')], contextPath: '/', war: '**/*.war'
        
    }
  }

  } 
}
