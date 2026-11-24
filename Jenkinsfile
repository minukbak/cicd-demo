
@Library('cicd-library@main') _

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
                gradleBuild()
            }
        }

        stage('Test') {
            steps {
                gradleTest()
            }
        }

        stage('Artifact') {
            steps {
                archiveJar()
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