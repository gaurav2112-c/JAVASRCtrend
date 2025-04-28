pipeline {
    agent any

    tools {
        jdk 'JAVA_HOME'   // <-- This must match the JDK name set in Jenkins -> Global Tool Configuration
    }

    environment {
        PATH = "/opt/maven/bin:$PATH"
    }

    stages {
        stage("build") {
            steps {
                echo "----------- build started ----------"
                sh 'mvn clean deploy -Dmaven.test.skip=true'
                echo "----------- build completed ----------"
            }
        }

        stage("test") {
            steps {
                echo "----------- unit test started ----------"
                sh 'mvn test'   // <- replaced with actual test command
                echo "----------- unit test completed ----------"
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

        stage("Quality Gate") {
            steps {
                script {
                    timeout(time: 5, unit: 'MINUTES') {   // <- changed to 5 min for faster feedback
                        def qg = waitForQualityGate()
                        if (qg.status != 'OK') {
                            echo "Warning: Quality gate failed but continuing pipleline: ${qg.status}"
                        }
                    }
                }
            }
        }
    }
}
