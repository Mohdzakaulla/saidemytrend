pipeline {
    agent any

    environment {
        PATH = "/opt/maven/bin:$PATH"
    }

    stages {
        stage("Build") {
            steps {
                echo "----------- Build started ----------"
                sh 'mvn clean deploy -Dmaven.test.skip=true'
                echo "----------- Build completed ----------"
            }
        }

        stage("Test") {
            steps {
                echo "----------- Unit test started ----------"
                sh 'mvn surefire-report:report'
                echo "----------- Unit test completed ----------"
            }
        }

        stage("SonarQube Analysis") {
            environment {
                scannerHome = tool 'SonarQube Scanner'
            }
            steps {
                withSonarQubeEnv('SonarQube servers') {
                    sh "${scannerHome}/bin/sonar-scanner"
                }
            }
        }
    }

    post {
        always {
            echo "Pipeline completed"
        }
        failure {
            echo "Pipeline failed"
        }
        success {
            echo "Pipeline succeeded"
        }
    }
} // Ensure this closing brace ends the `pipeline` block

