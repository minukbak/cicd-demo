@Library('cicd-library@main') _

pipeline {
    agent any

    options {
        skipDefaultCheckout()
        disableConcurrentBuilds()
        timestamps()
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
                sh 'aws --version'
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

        stage('Artifact Archive') {
            steps {
                archiveJar()
            }
        }

        stage('Artifact Upload & Verify') {
            steps {
                uploadArtifact()
            }
        }

        stage('Deploy & Verify') {
            steps {
                deployApp()
            }
        }
    }

    post {
        success {
            echo 'CI/CD Pipeline SUCCESS'
        }

        failure {
            echo 'CI/CD Pipeline FAILED'
        }

        cleanup {
            script {
                try {
                    notifySlack(currentBuild.currentResult)
                } catch (Exception e) {
                    echo '[WARNING] Slack notification failed'
                }
            }
        }
    }
}