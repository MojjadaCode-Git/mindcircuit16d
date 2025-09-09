pipeline {
    agent any

    environment {
        TOMCAT_URL = 'http://54.165.35.212:8080/manager/text'
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout your source code
                git 'https://github.com/MojjadaCode-Git/mindcircuit16d.git'
            }
        }

        stage('Build WAR') {
            steps {
                // Build the WAR file with Maven
                sh 'mvn clean package'
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                // Use Jenkins credentials to securely pass username/password to Maven cargo plugin
                withCredentials([usernamePassword(credentialsId: 'tomcat-deploy-creds', usernameVariable: 'TOMCAT_USER', passwordVariable: 'TOMCAT_PASS')]) {
                    sh '''
                        mvn deploy \
                          -Dcargo.remote.username=$TOMCAT_USER \
                          -Dcargo.remote.password=$TOMCAT_PASS \
                          -Dcargo.remote.uri=${TOMCAT_URL}
                    '''
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment succeeded!'
        }
        failure {
            echo 'Deployment failed!'
        }
    }
}

