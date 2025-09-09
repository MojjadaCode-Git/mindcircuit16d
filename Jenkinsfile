pipeline {
    agent any

    environment {
        TOMCAT_URL = 'http://54.165.35.212:8080/manager/text'
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/MojjadaCode-Git/mindcircuit16d.git'
            }
        }

        stage('Build WAR') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'tomcat-deploy-creds', usernameVariable: 'TOMCAT_USER', passwordVariable: 'TOMCAT_PASS')]) {
                    sh """
                    mvn cargo:deploy \
                        -Dcargo.remote.username=$TOMCAT_USER \
                        -Dcargo.remote.password=$TOMCAT_PASS \
                        -Dcargo.remote.uri=${TOMCAT_URL}
                    """
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
