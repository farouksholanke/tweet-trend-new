pipeline {
    agent {
        node {
            label 'maven'
        }
    }
    environment {
        // Prepend the Maven bin directory to the PATH for all stages
        PATH = "/opt/apache-maven-3.9.6/bin:${env.PATH}"
    }
    tools {
        // Ensure JDK17 is defined in Jenkins Global Tool Configuration
        jdk 'JDK17'
    }
    stages {
        stage("Build") {
            steps {
                // Explicitly set JAVA_HOME and PATH for the Maven command
                sh """
                    export JAVA_HOME=\$(echo \${JAVA_HOME})
                    export PATH=\$JAVA_HOME/bin:\$PATH
                    echo "JAVA_HOME = \$JAVA_HOME"
                    echo "PATH = \$PATH"
                    mvn clean deploy
                """
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
                        // Ensure Maven is correctly recognized
                        mvn -version
                        ${scannerHome}/bin/sonar-scanner
                    """
                }
            }
        }
    }
}
