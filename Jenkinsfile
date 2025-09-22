pipeline {
    agent any

    tools {
        maven 'MAVEN3'   // Jenkins-managed Maven
        jdk 'jdk17'      // Jenkins-managed JDK
    }

    environment {
        SONARQUBE_SERVER = 'My sonarQube'  // Name must match your SonarQube configuration in Jenkins
    }

    stages {
        stage('Build') {
            steps {
                echo 'Building the project...'        
                sh 'mvn clean install'            
            }
        }

        stage('SonarQube Analysis') {
            steps {
                echo 'Running SonarQube analysis...'
                withSonarQubeEnv(SONARQUBE_SERVER) {  
                    sh 'mvn sonar:sonar'                // Step = Actual code analysis on Jenkins node [fetches the Quality profile rules from sonarqube server]+ sending results to SonarQube.
                }
            }
        }

        stage('Quality Gate') {
            steps {
                echo 'Checking SonarQube Quality Gate...'
                timeout(time: 15, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true    // Step  = Just waiting for SonarQube’s PASS/FAIL decision (no new analysis).
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully. Check SonarQube report.'
        }
        failure {
            echo 'Pipeline failed. Check logs for errors.'
        }
    }
}
