pipeline {
    agent any

    tools {
        maven 'Maven 3'
        jdk 'JDK 11'
    }

    stages {
        stage('Build') {
            steps {
                dir('DevOps-Project-18/spring-boot-app') {
                    sh 'mvn clean package'
                }
            }
        }

        stage('Test') {
            steps {
                dir('DevOps-Project-18/spring-boot-app') {
                    sh 'mvn test'
                }
            }
        }

        stage('Deploy') {
            steps {
                echo 'Add your deploy steps here'
            }
        }
    }

    post {
        always {
            junit 'DevOps-Project-18/spring-boot-app/target/surefire-reports/*.xml'
        }
    }
}
