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

        stage('Artifact Archive') {
            steps {
                archiveJar()
            }
        }

        stage('Artifact Upload') {
            steps {
                uploadArtifact()
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