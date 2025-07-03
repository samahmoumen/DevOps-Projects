pipeline {
    agent any

    tools {
        maven 'Maven 3'
        jdk 'JDK 11'
    }

    environment {
        SONARQUBE_SERVER = 'SonarQubeServer' // Adjust this to your SonarQube server configuration name in Jenkins
        HELM_CHART_PATH = 'helm/chart/path' // Adjust this path to your Helm chart location
        KUBE_CONFIG = credentials('kubeconfig') // Example credential for kubeconfig if needed
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

        stage('Deploy to Test Environment') {
            steps {
                script {
                    // Example Helm deploy command - adjust according to your Helm setup
                    sh "helm upgrade --install spring-boot-app-test ${env.HELM_CHART_PATH} --namespace test-env --kubeconfig ${env.KUBE_CONFIG}"
                }
            }
        }

        stage('User Acceptance Tests') {
            steps {
                echo 'Run your UAT scripts here (e.g., integration tests, API tests)'
                // For example, you might run a shell script or invoke testing tools here
            }
        }

        stage('Promote to Production') {
            steps {
                script {
                    // Example Argo CD CLI command to promote the app to production
                    sh 'argocd app sync spring-boot-app-prod'
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

