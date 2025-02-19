pipeline {
    agent any

    environment {
        SONARQUBE_SERVER = 'SonarQube-Server'
    }

    parameters {
        string(name: 'BRANCH_NAME', defaultValue: 'main', description: 'Branch to build')
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm: [
                    $class: 'GitSCM',
                    branches: [[name: "*/${params.BRANCH_NAME}"]],
                    userRemoteConfigs: [[url: 'https://github.com/errachidy10/jenkins-pipeline-demo.git']]
                ]
            }
        }
        stage('Build') {
            steps {
                script {
                    if (fileExists('pom.xml')) {
                        sh 'mvn clean install'
                    } else {
                        echo 'No pom.xml found, skipping build'
                    }
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    if (fileExists('pom.xml')) {
                        sh 'mvn test'
                    } else {
                        echo 'No pom.xml found, skipping tests'
                    }
                }
            }
        }
        stage('SonarQube Analysis') {
            steps {
                script {
                    if (fileExists('pom.xml')) {
                        withSonarQubeEnv('SonarQube-Server') {
                            sh 'mvn sonar:sonar'
                        }
                    } else {
                        echo 'No pom.xml found, skipping SonarQube analysis'
                    }
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    // Déploiement sur un serveur (par exemple, via SCP ou rsync)
                    sh 'scp target/your-app.jar user@server:/path/to/deploy/'
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished.'
        }
        success {
            echo 'Pipeline succeeded.'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}