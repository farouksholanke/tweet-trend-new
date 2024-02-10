pipeline {
    agent {
        node {
            label 'maven'
        }
    }
environment {
    PATH = "/opt/apache-maven-3.9.6/bin:$PATH"
}
    stages {
        stage("build"){
            steps {
                sh 'mvn clean deploy'
            }
        }

    stage('SonarQube analysis') {
    tools {
        jdk "JDK17" 
    }
    environment{
        scannerHome = tool 'santi-sonar-scanner'
        JAVA_HOME = "${tool 'JDK17'}" // Set JAVA_HOME to the JDK 17 installation provided by Jenkins tool configuration
            }
    }
    steps{
    withSonarQubeEnv('santi-sonarqube-server') { // If you have configured more than one global server connection, you can specify its name
      sh """
                       export JAVA_HOME=${JAVA_HOME}
                       export PATH=$JAVA_HOME/bin:$PATH
                       ${scannerHome}/bin/sonar-scanner
                       """
    }
    }
  }    
    }

