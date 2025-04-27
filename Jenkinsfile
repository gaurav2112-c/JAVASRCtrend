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
            steps {                           // 5  // Defines the steps that will be executed in this stage
                echo "----------- build started ----------"
                                              // Logs a message indicating the start of the build
                sh 'mvn clean deploy -Dmaven.test.skip=true'
                                              // Runs Maven clean and deploy commands, skipping tests
                echo "----------- build completed ----------"

            }
        }
        stage("test") {                       // 6  // Creates a stage named 'test'
            steps {                           // 7  // Defines the steps that will be executed in this stage
                echo "----------- unit test started ----------"
                                              // Logs a message indicating the start of unit tests
                sh 'mvn surefire-report:report'
                                              // Runs the Maven Surefire report to execute unit tests
                echo "----------- unit test completed ----------"
                                              // Logs a message indicating unit test completion
            }                                 // 7  // Ends the steps block for 'test' stage
        }                                     // 6  // Ends the 'test' stage
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
