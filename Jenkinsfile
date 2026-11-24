pipeline {
    agent any

    options {
        skipDefaultCheckout()
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Environment Check') {
            steps {
                sh 'java -version'
                sh 'chmod +x gradlew'
                sh './gradlew --version'
            }
        }

        stage('Build') {
            steps {
                sh './gradlew clean assemble'
            }
        }

        stage('Test') {
            steps {
                sh './gradlew test'
            }
            post {
                always {
                    junit 'app/build/test-results/test/*.xml'
                }
            }
        }

        stage('Artifact Check') {
            steps {
                sh 'ls -lh app/build/libs/'
            }
        }
    }

    post {
        success {
            echo 'CI Pipeline SUCCESS'
        }
        failure {
            echo 'CI Pipeline FAILED'
        }
    }
}