pipeline {
    agent {
        node {
            label 'maven'
        }
    }
    environment {
        // Set global environment variables here if needed
    }
    tools {
        // Ensure JDK17 is defined in Jenkins Global Tool Configuration
        jdk 'JDK17'
    }
    stages {
        stage("Build") {
            steps {
                sh 'mvn clean deploy'
            }
        }
        stage('SonarQube analysis') {
            steps {
                withSonarQubeEnv('santi-sonarqube-server') {
                    // Directly use the JAVA_HOME and PATH variables provided by the Jenkins JDK tool
                    sh """
                        export JAVA_HOME=\$(echo \${JAVA_HOME})
                        export PATH=\$JAVA_HOME/bin:\$PATH
                        echo "Using Java at \$JAVA_HOME with version:"
                        java -version
                        ${scannerHome}/bin/sonar-scanner
                    """
                }
            }
        }
    }
}

