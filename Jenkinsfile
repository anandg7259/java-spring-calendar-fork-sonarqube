pipeline {
    agent  { label 'suprith2' }   

    tools {
        jdk 'jdk17'
        maven 'Maven3'
    }

    stages {
        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }
stage('Deploy to Tomcat') {
    steps {
        sh '''
            WAR_FILE="/home/ubuntu/workspace/Assignment_pipeline/target/*.jar"
            SERVER_IP="172.31.39.195"
            USER_NAME="ubuntu"
            TMP_DIR="/tmp/App/"
            TOMCAT_DIR="/opt/tomcat/webapps/"

            # Create temp dir on remote
            ssh ${USER_NAME}@${SERVER_IP} "mkdir -p ${TMP_DIR}"

            # Copy artifact
            scp ${WAR_FILE} ${USER_NAME}@${SERVER_IP}:${TMP_DIR}

            # Move artifact into Tomcat webapps
            ssh ${USER_NAME}@${SERVER_IP} "sudo mv ${TMP_DIR/*.jar} ${TOMCAT_DIR}"
        '''
       }
        }
    }
}
