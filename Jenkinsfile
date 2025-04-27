pipeline {
    agent any

    tools {
        jdk 'JAVA_HOME'   // <-- This is the name you set in Jenkins -> Global Tool Configuration
    }

    environment {
        PATH = "/opt/maven/bin:$PATH"
    }

    stages {
        stage("build") {
            steps {
                echo "----------- build started ----------"
                sh 'java -version'       // (optional: this will print Java 21 version during build)
                sh 'mvn clean deploy'
                echo "----------- build completed ----------"
            }
        }

        stage('SonarQube analysis') {
            environment {
                scannerHome = tool 'gaurav-sonar-scanner'
            }
            steps {
                withSonarQubeEnv('JAVA-sonarqube-server') {
                    sh "${scannerHome}/bin/sonar-scanner"
                }
            }
        }
    }
}
