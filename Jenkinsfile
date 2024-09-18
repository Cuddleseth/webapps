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
    
    stage('Check Dependency'){
      steps{
       dependencyCheck additionalArguments: ''' --scan ./ --format HTML --failOnCVSS 8 --nvdApiKey 63ef571c-a22c-4196-b12c-f8f833e72274''', odcInstallation: 'DP-Check'
      }
    }
    stage('SonarQube') {
            steps {
    withSonarQubeEnv('sonar') {
  sh "ls ${scannerHome}"
  sh "echo ${scannerHome}"
}}}
     stage('SonarQube Analsyis') {
            steps {
                withSonarQubeEnv('sonar') {
                    sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=Webapps -Dsonar.url=http://172.20.10.11:9000/ \
                    -Dsonar.login=squ_e7bbf58f47b1bbafab0230566948dc32fd329618 -Dsonar.projectKey=Webapps -Dsonar.java.binaries=. '''
                }
            }
        }
     stage ('SAST') {
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
