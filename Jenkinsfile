pipeline{
agent any
tools{
  maven 'Maven'
  jdk 'jdk17'
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
    
  
   
 stage('SonarQube Analsyis') {
   tools{
  jdk 'jdk17'
}
   environment {
        scannerHome = tool 'sonar' // the name you have given the Sonar Scanner (Global Tool Configuration)
    }
            steps {
                withSonarQubeEnv('sonar') {
                    sh ''' $scannerHome/bin/sonar-scanner -Dsonar.projectName=Webapps -Dsonar.url=http://172.28.208.1:9000/ \
                    -Dsonar.login=squ_e7bbf58f47b1bbafab0230566948dc32fd329618 -Dsonar.projectKey=Webapps -Dsonar.java.binaries=. '''
                }
            }
        }

   
     
    stage('Build'){
      steps{
       sh 'mvn clean package'
      }
     
    }
    stage('Deploy to tomcat server'){
      steps{
       deploy adapters: [tomcat9(credentialsId: 'tomcat', path: '', url: 'http://172.28.208.1:9090/')], contextPath: '/', war: '**/*.war'
        
    }
  }

  } 
}
