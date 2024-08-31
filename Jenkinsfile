pipeline{
agent any
tools{
  maven 'Maven'
}
  stages{
    stage ('Initialized'){
      step{
        sh '''
              echo "PATH =${PATH}"
              echo "M2_HOME = ${M2_HOME}"
        '''
      }
    }
    stage('Build'){
      sh 'mvn clean package'
    }    
  }

  
}
