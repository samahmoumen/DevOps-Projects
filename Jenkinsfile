pipeline {
    agent any

    tools {
        maven 'Maven 3'
        jdk 'JDK 11'
    }

    environment {
        SONARQUBE_SERVER = 'SonarQubeServer' // Adjust this to your SonarQube server configuration name in Jenkins

  }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                dir('DevOps-Project-18/spring-boot-app') {
                    sh 'mvn clean compile'
                }
            }
        }

stage('Unit Tests') {
    steps {
        sh 'pwd'
        sh 'ls -l DevOps-Project-18/spring-boot-app'

        dir('DevOps-Project-18/spring-boot-app') {
            sh 'mvn test'
        }
    }
    post {
        always {
            junit 'DevOps-Project-18/spring-boot-app/target/surefire-reports/*.xml'
        }
    }
}


        stage('SonarQube Analysis') {
            environment {
                scannerHome = tool 'SonarQubeScanner' // Adjust to your SonarQube Scanner tool name in Jenkins
            }
            steps {
                dir('DevOps-Project-18/spring-boot-app') {
                    withSonarQubeEnv('SonarQubeServer') {
                        sh "${scannerHome}/bin/sonar-scanner"
                    }
                }
            }
        }

        stage('Package') {
            steps {
                dir('DevOps-Project-18/spring-boot-app') {
                    sh 'mvn package -DskipTests'
                }
            }
        }

  
      
 
    }

    post {
        always {
            echo 'Pipeline finished.'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}

