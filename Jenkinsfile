pipeline {
    agent any

    parameters {
        string(name: 'main', defaultValue: 'main', description: 'Branch to build')
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm: [
                    $class: 'GitSCM',
                    branches: [[name: "*/${params.main}"]],
                    userRemoteConfigs: [[url: 'https://github.com/errachidy10/jenkins-pipeline-demo.git']]
                ]
            }
        }
        stage('Build') {
            steps {
                script {
                    if (fileExists('pom.xml')) {
                        bat 'mvn clean install'
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
                        bat 'mvn test'
                    } else {
                        echo 'No pom.xml found, skipping tests'
                    }
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    // Déploiement sur un serveur (par exemple, via SCP ou rsync)
                    bat 'echo Déploiement simulé'
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
