pipeline{
agent any
tools{
  maven 'Maven'
  jdk "jdk" 
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
    
    stage('Check Dependency'){
      steps{
       dependencyCheck additionalArguments: ''' --scan ./ --format HTML --failOnCVSS 8 --nvdApiKey 63ef571c-a22c-4196-b12c-f8f833e72274''', odcInstallation: 'DP-Check'
      }
    }
   
     stage ('SAST') {
       tools{
  jdk "jdk" 
}
       environment {
        scannerHome = tool 'sonar' // the name you have given the Sonar Scanner (Global Tool Configuration)
    }
      steps {
        withSonarQubeEnv('sonar') {
          sh 'mvn sonar:sonar'
          sh 'cat target/sonar/report-task.txt'
        }
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
       deploy adapters: [tomcat9(credentialsId: 'tomcat', path: '', url: 'http://172.20.10.11:9090/')], contextPath: '/', war: '**/*.war'
        
    }
  }

  } 
}
