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
        stage("Build") {
            steps {
                sh 'mvn clean deploy'
            }
        }

        stage('SonarQube analysis') {
            environment {
                scannerHome = tool 'santi-sonar-scanner'
                JAVA_HOME = tool 'JDK17' // This will set JAVA_HOME to the JDK 17 installation provided by Jenkins tool configuration
            }
            steps {
                withSonarQubeEnv('santi-sonarqube-server') { 
                    sh """
                       
                        ${scannerHome}/bin/sonar-scanner
                    """
                }
            }
        }
    }
}

