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
                
            }
            steps {
                withSonarQubeEnv('santi-sonarqube-server') { 
                    sh """
                        export JAVA_HOME=${JAVA_HOME}
                        export PATH=$JAVA_HOME/bin:$PATH
                        ${scannerHome}/bin/sonar-scanner
                    """
                }
            }
        }
    }
}

