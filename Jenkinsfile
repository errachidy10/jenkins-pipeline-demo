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
                    if (fileExists('src/main/java/com/example/App.java')) {
                        bat 'javac src/main/java/com/example/App.java'
                    } else {
                        echo 'No App.java found, skipping build'
                    }
                }
            }
        }
        stage('Run') {
            steps {
                script {
                    if (fileExists('src/main/java/com/example/App.class')) {
                        bat 'java -cp src/main/java com.example.App'
                    } else {
                        echo 'No App.class found, skipping run'
                    }
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
