pipeline {
    agent any

    environment {
        // Set JAVA_HOME if needed (optional, Jenkins may detect automatically)
        JAVA_HOME = tool name: 'JDK17', type: 'jdk' // Replace with your Jenkins JDK tool name
        PATH = "${env.JAVA_HOME}/bin:${env.PATH}"
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the main branch from your GitHub repo
                git branch: 'main', url: 'https://github.com/MojjadaCode-Git/mindcircuit16d.git'
            }
        }

        stage('Build') {
            steps {
                // Run Maven clean package
                withMaven(maven: 'Maven3') {  // Replace 'Maven3' with your Jenkins Maven tool name
                    sh 'mvn clean package'
                }
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                // Deploy WAR using Cargo plugin
                // You need to configure cargo plugin in your pom.xml with credentials & Tomcat URL
                sh 'mvn cargo:deploy'
            }
        }
    }

    post {
        success {
            echo 'Build and Deployment Successful!'
        }
        failure {
            echo 'Build or Deployment Failed.'
        }
    }
}
