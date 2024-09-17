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
       deploy adapters: [tomcat9(credentialsId: 'tomcat', path: '', url: 'http://localhost:9090/')], contextPath: '/webapps/', war: '**/*.war'
      }
      
    }
  }

  
}
