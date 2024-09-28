pipeline {
    agent any

    environment {
        SONARQUBE_SERVER = 'SonarQube' // SonarQube Server name configured in Jenkins
    }

    stages {
        stage('Build') {
            steps {
                // Example of using Maven to build the project
                sh 'mvn clean package'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') { // Inject SonarQube environment variables
                    // Run SonarQube Scanner with Maven or Gradle
                    sh 'mvn sonar:sonar -Dsonar.projectKey=myproject -Dsonar.host.url=http://localhost:9000 -Dsonar.login=sqp_d855881e05fac64ef4fb4d06fd4444e34ba3ea67'
                }
            }
        }

        stage('Quality Gate') {
            steps {
                script {
                    // Wait for SonarQube Quality Gate result
                    def qualityGate = waitForQualityGate()
                    if (qualityGate.status != 'OK') {
                        error "Pipeline aborted due to SonarQube quality gate failure: ${qualityGate.status}"
                    }
                }
            }
        }
    }

    post {
        always {
            echo 'Cleaning up...'
            cleanWs()
        }
    }
}
