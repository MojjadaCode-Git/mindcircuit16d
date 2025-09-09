pipeline {
    agent any

    environment {
        TOMCAT_URL = 'http://54.165.35.212:8080/manager/text'
    }

    tools {
        maven 'Maven 3.8.5'   // Replace with your configured Maven version name in Jenkins
        jdk 'JDK 17'          // Replace with your configured JDK version name in Jenkins
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://your-repo-url.git'  // Replace with your Git repo
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
